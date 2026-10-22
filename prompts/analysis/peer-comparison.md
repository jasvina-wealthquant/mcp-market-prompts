# Peer Comparison

**What it does:** Places a company next to its real peers on the few ratios that actually matter for its line of work, and tells you where it is genuinely better or just optically cheaper.

**Needs:** A Firecrawl MCP to pull current figures from Screener, Tickertape and peer pages.

**How to use:** Name the company and, if you like, the peers. Otherwise let it choose the right comparison set.

## The prompt

```
You are my research partner. Compare a company to its true peers and tell me what the comparison actually means, not just the table.

COMPANY: [name], ticker [TICKER].
PEERS: [list them, or write "you choose the right ones"].

Steps:
1. Use the Firecrawl tools to pull current figures for the company and each peer from Screener and Tickertape.
2. Pick the three or four ratios that matter for this business type, and say why. For a lender use margin, return on assets and asset quality. For a manufacturer use operating margin, return on capital and working capital. For a consumer brand use growth, margin and return on capital. Do not force one size on all.
3. Build a clean comparison table with the company and its peers.
4. Then write the part that matters. Where is this company genuinely better, where is it worse, and is any cheapness real or a trap. If it looks cheap on one ratio but weak on the quality ratios, say so plainly.
5. End with one honest line on whether the valuation gap to peers is deserved.

Rules:
- Compare like with like. Never put a bank and a software company on the same ratio.
- Flag any figure you could not verify, and give the as of date.
- No dashes. Plain English only.
```

## Notes

- Useful right after a results day to check whether a move was company specific or sector wide.
- Ask it to add a one year and three year return column if you want the price story alongside the fundamentals.
