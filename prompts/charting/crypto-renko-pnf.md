# Crypto Renko and PnF Scanner

**What it does:** Builds a Python toolkit that draws log based Renko and Point and Figure charts for crypto, marks patterns on the bricks and columns, compares each symbol to BTC for relative strength, and scans for setups. It is for analysis and your own discretionary decisions. It does not place orders.

**Needs:** Python and Claude Code. Pulls public crypto data through ccxt, so no exchange keys are required for the charting itself.

**Why noiseless charts:** Renko and PnF strip out time and small wiggles, so the trend and the patterns are cleaner and your rules stay objective for backtesting and forward testing. Log based bricks keep a brick the same percentage move at every price level.

## The prompt

```
ROLE
You are my charting and research engineer. Build a Python toolkit that creates log based Renko and Point and Figure (PnF) charts for crypto, identifies patterns on them, and scans for setups. This is for analysis and discretionary decisions. It does not place any orders.

Before writing code, summarise your understanding and list every assumption. Wait for my confirmation.

1. SETUP
- Python with ccxt (crypto OHLC), pandas, numpy, and plotly for charts.
- Pull 1 minute OHLC from a liquid exchange (for example Binance) through ccxt. Respect rate limits.
- Config: EXCHANGE="binance", QUOTE="USDT", RENKO_BRICK_PCT=0.25, PNF_BOX_PCT=0.1, PNF_REVERSAL=3, EMA_SPANS=[20,50], SUPERTREND_PERIOD=10, SUPERTREND_MULT=3.

2. LOG BASED BRICKS
- Build Renko on a log price scale so each brick is a fixed percentage move (RENKO_BRICK_PCT), not a fixed dollar amount. This keeps bricks consistent across price levels.
- Build PnF the same way, box size as a percentage (PNF_BOX_PCT) on log price, with a PNF_REVERSAL box reversal.
- Document the brick and box math clearly so I can audit it.

3. CHARTS, three panel layout
- Trend chart: 1 minute log Renko with the EMAs and Supertrend. Top left.
- Entry chart: 1 minute log PnF with columns and the EMAs. Top right.
- Ratio chart: log Renko of the symbol divided by BTC, to show relative strength versus BTC. Bottom right.

4. PATTERN IDENTIFICATION
- On Renko, mark swing lows near the EMA and pullback entries where price returns to the moving average after an up move.
- On PnF, detect a bear trap variation, a false breakdown that reverses back up, and mark its invalidation level as a stop reference.
- Label every detected pattern on the chart with its type and the brick or column it formed on.

5. SYMBOL SELECTION AND SCANNING
- Rank all QUOTE pairs by rolling volume. Start scanning from the second symbol by volume.
- For each symbol, compute: is it above its EMAs, is the Renko trend up, is it strong versus BTC on the ratio chart, and is there a fresh PnF bear trap or a clean pullback.
- Output a ranked shortlist of symbols that meet trend and relative strength and an entry pattern together.

6. TARGET AND STOP REFERENCE, for my discretion only
- Target: project 100 percent of the last anchor column breakout on the PnF.
- Stop: the pattern invalidation level.
- Present these as reference levels only. Do not place or manage any orders.

7. OUTPUT
- Save the three chart images for each shortlisted symbol.
- A scan.csv with symbol, trend state, relative strength versus BTC, pattern found, suggested target and stop reference, and a timestamp.
- Refresh on a schedule I set, for example every 15 minutes.

Build in small, testable modules: data, renko, pnf, indicators, patterns, scanner. Test each and show me the results before moving on.
```

## Notes

- This builds the tools and the shortlist. You take the trades yourself. Keeping execution out keeps it simple and lower risk.
- Crypto trades around the clock and moves fast, so the differences between backtest and live can be large. Forward test the signals on paper before trusting them.
- The same idea works for Indian equities. Swap ccxt for your broker or data feed and keep the log based bricks.
