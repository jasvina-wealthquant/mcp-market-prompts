# Iron Condor Bot

**What it does:** Builds and runs a Python bot that trades a NIFTY weekly Iron Condor on Zerodha Kite Connect. The wings cap the loss, so the risk is defined. Defaults to dry run.

**Needs:** Kite Connect API credentials and Claude Code.

**Read first:** Options can lose money quickly, defined risk or not. See the [disclaimer](../../DISCLAIMER.md). Keep it in dry run and paper trade it first.

## The prompt

```
ROLE
You are my trading-systems engineer. Build and run a Python bot that connects to Zerodha Kite Connect and trades a NIFTY weekly Iron Condor using the rules below. Follow them exactly. Do not add or change a rule without asking me.

Before writing code, give me a short summary of your understanding of the strategy and list every assumption. Wait for my confirmation.

1. SETUP
- Python with kiteconnect, pandas, numpy, and plotly for charts.
- Read KITE_API_KEY, KITE_API_SECRET and the daily ACCESS_TOKEN from a .env file. Never hard-code or print credentials.
- A small login helper for the daily Kite access token.
- Config: LOTS=[1], SHORT_DELTA=[0.16], WING_WIDTH_POINTS=[200], TARGET_PROFIT_PCT=[50], STOPLOSS_ON_CREDIT_X=[2.0], MAX_ENTRIES_PER_WEEK=[1], ENTRY_DAY="MON", EXIT_BUFFER_DAYS=1, DRY_RUN=true.

2. EXPIRY AND CONTRACTS
- NIFTY weekly options only. Take the actual weekly expiry from kite.instruments("NFO") so holidays are handled. Read lot size from the dump, never hard-code it.
- Trade the nearest weekly expiry. One position at a time.

3. STRIKE SELECTION
- Spot = NIFTY 50 LTP.
- Sell the CE and PE closest to SHORT_DELTA on each side. Use greeks from kite quotes, or approximate with distance from spot if greeks are unavailable, and tell me which you used.
- Buy the protective CE and PE exactly WING_WIDTH_POINTS further out. This caps the loss and defines the risk.

4. ENTRY
- Enter once per week, on ENTRY_DAY, between 9:45 AM and 11:00 AM, only if no position is open and entries this week are below MAX_ENTRIES_PER_WEEK.
- Skip entry if the ATM straddle implied volatility is in the bottom 20 percent of its last 20 sessions. Log the IV reading either way.
- Confirm margin with kite.basket_order_margins() for all four legs before placing.
- Action: SELL the two short strikes, BUY the two wings, product NRML. Record the net credit.

5. EXIT (whichever comes first)
- Target: net premium to close falls to credit x (1 - TARGET_PROFIT_PCT/100). Close all four legs.
- Stop loss: net premium to close rises to credit x STOPLOSS_ON_CREDIT_X. Close all four legs.
- Time stop: close everything EXIT_BUFFER_DAYS before expiry regardless.
- Breach alert: if spot touches either short strike, alert me, and close early only if I have configured that. Default is alert only.

6. EXECUTION AND RISK
- LIMIT orders only, near LTP with a small buffer, rounded to the 0.05 tick. Place the BUY wings first so you are never naked.
- If a leg is unfilled in 30 seconds, modify toward market up to 3 times. If a short leg fills but its wing cannot, close that short leg immediately and alert me.
- Reconcile with kite.orders() and kite.positions() after every action. Kite is the source of truth. Never double enter.
- Save state to state.json and reconcile with Kite on every restart.
- Kill switch: if a file named STOP exists, or weekly loss exceeds the max risk of one condor, square off and halt.
- Respect exchange and SEBI rate limits for retail algos. Place orders only from my broker-registered static IP.

7. MODES
- DRY_RUN=true by default: simulate fills at LTP, log everything, place no real orders.
- Go live only when I type GO LIVE, and confirm the config with me once before the first live order.

8. LOGGING
- signals.csv: every evaluation with time, spot, strikes, IV reading, decision, reason.
- trades.csv: entry and exit details, all four strikes, credit, P&L, exit reason.
- End of week: a short summary of trades, P&L and any errors.

Build this in small, testable modules (data, strikes, signals, execution, risk, runner). Test each, show me results, then move on.
```

## Notes

- The defined risk is the point. The wings mean you always know the worst case on a single condor before you enter.
- Build the same condor in a payoff tool like Sensibull to see the shape before you run anything live.
