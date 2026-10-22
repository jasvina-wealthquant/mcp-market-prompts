# Short Straddle Bot

**What it does:** Builds and runs a Python bot that sells a NIFTY monthly ATM short straddle on Zerodha Kite Connect, with pattern based entries, a 50 EMA stop, a decay target, and full risk controls. Defaults to dry run.

**Needs:** Kite Connect API credentials and Claude Code (so Claude can write and run the project).

**Read first:** This sells options, which carries unlimited risk if unmanaged. See the [disclaimer](../../DISCLAIMER.md). Keep it in dry run and paper trade it before anything else.

## The prompt

```
ROLE
You are my trading-systems engineer. Build and run a Python trading bot that connects to Zerodha Kite Connect and trades a NIFTY monthly ATM short straddle using the rules below. Follow them exactly. Do not add or change any rule without asking me.

Before writing any code, give me a short summary of your understanding of the strategy and list every assumption you are making. Wait for my confirmation.

1. SETUP
- Python project using kiteconnect, pandas, and mplfinance or plotly for charts.
- Read KITE_API_KEY, KITE_API_SECRET and the daily ACCESS_TOKEN from a .env file. Never hard-code or print credentials.
- Provide a small login helper for the daily Kite login to generate the access token.
- Config file with: LOTS=[1], MAX_LOSS_PER_TRADE_INR=[5000], TARGET_DECAY_PCT=[35], MAX_ENTRIES_PER_DAY=[2], ROLLOVER_DAYS=7, DRY_RUN=true.

2. EXPIRY AND CONTRACT SELECTION
- Use only NIFTY monthly options. Always take the actual expiry date from kite.instruments("NFO") so trading holidays are handled.
- If 7 or fewer calendar days remain to the current monthly expiry, the current month is no longer tradable. Square off any open position in it and switch all analysis to next month's expiry.
- Read the lot size from the instruments dump. Never hard-code it.

3. ATM STRIKE
- ATM = NIFTY 50 spot LTP rounded to the nearest 50.
- While flat, recalculate ATM at every 15 min candle close and chart that strike.
- While in a position, keep tracking the strikes actually held until exit.

4. STRADDLE CHART
- Fetch 1 minute historical candles for the ATM CE and PE for at least the last 10 trading days.
- Build the combined straddle series per minute: CE close + PE close, matched on timestamp.
- Resample to 15 minute OHLC aligned to the 9:15 market candles.
- Calculate the 50 EMA on the 15 min close (span 50, adjust=False).
- Every 15 minutes, save a chart image showing candles, the 50 EMA, and entry and exit markers.

5. ENTRY RULES (SELL THE STRADDLE)
Evaluate only on fully closed 15 min candles. Enter when ALL are true:
a) The signal candle's straddle close is below the 50 EMA.
b) The latest closed candles form a bearish reversal pattern: bearish engulfing, shooting star, or evening star.
c) Time is between 9:30 AM and 3:00 PM.
d) No open straddle position exists, confirmed via kite.positions().
e) Entries today are below MAX_ENTRIES_PER_DAY.
f) Margin is sufficient, checked via kite.basket_order_margins() for both legs.
Action: SELL LOTS x lot size of the ATM CE and the ATM PE, product NRML.

6. EXIT RULES (whichever comes first)
- Stop loss: a 15 min straddle candle closes above the 50 EMA. Exit both legs.
- Hard max loss: live MTM loss reaches MAX_LOSS_PER_TRADE_INR, checked every minute. Exit immediately.
- Target: combined premium falls to entry premium x (1 - TARGET_DECAY_PCT/100). Exit both legs.
- Rollover: 7 days before expiry, exit and shift to next month per section 2.
- Re-entry only on a fresh signal that forms after the exit candle.

7. ORDER EXECUTION AND RISK
- LIMIT orders only, priced near LTP with a small buffer, rounded to the 0.05 tick.
- If a leg is unfilled within 30 seconds, modify toward market up to 3 times.
- If only one leg fills and the other cannot, square off the filled leg immediately and alert me.
- Reconcile with kite.orders() and kite.positions() after every order. Kite is the source of truth. Never double enter.
- Save state to state.json and reconcile with Kite on every restart.
- Kill switch: if a file named STOP exists, or daily loss exceeds 2x MAX_LOSS_PER_TRADE_INR, square off everything and halt.
- Keep order rate within exchange and SEBI limits for retail algos, and place orders only from my broker-registered static IP.

8. MODES
- DRY_RUN=true by default: run everything, simulate fills at LTP, log them, place no real orders.
- Switch to live only when I explicitly say GO LIVE. Then confirm the config with me once before the first live order.

9. LOGGING AND REPORTING
- signals.csv: every 15 min evaluation.
- trades.csv: entry and exit details with P&L and exit reason.
- End of day: a short summary of trades, P&L, rules triggered and any errors.

Build this in small, testable modules (data, chart, signals, execution, risk, runner). Test each, show me the results, then move to the next.
```

## Notes

- Forward test, which means systematic paper trades, before you put in money.
- The checking frequency matters. A 15 or 30 minute cycle is comfortable. If you cannot keep it running and watched, do not run it live.
