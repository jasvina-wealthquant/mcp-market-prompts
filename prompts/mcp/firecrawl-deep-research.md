# Deep Research

**What it does:** Produces a cited, multi source research report on any company, sector or theme. Good for a megatrend, a supply chain, or a company you know nothing about yet.

**Needs:** A Firecrawl MCP server for live search and reading.

**How to use:** Give it the topic and how deep you want it. Let it search and read before it writes.

## The prompt

```
You are my research desk. I want a rigorous, cited report on the topic below. Search widely, read the best sources, and synthesise. Do not pad.

TOPIC: [company, sector, supply chain, or theme].
DEPTH: [a two minute brief, or a full report].

Steps:
1. Use the Firecrawl search and scrape tools to find the strongest primary and secondary sources. Prefer filings, official data, credit reports and reputable outlets over blogs.
2. Cross check any important number across at least two sources.
3. Write the report:
   - A short executive summary that answers the question directly.
   - The key findings, each tied to a source.
   - The bull case and the bear case, stated fairly.
   - What is still uncertain or unknown.
   - A short list of the sources you leaned on.

Rules:
- Separate fact from opinion. Label estimates clearly.
- If sources disagree, show the disagreement rather than hiding it.
- No dashes, no filler, no hype words. Write plainly.
- Give the as of date for time sensitive data.
```

## Notes

- For Indian equities, point it at Screener, BSE, NSE, Trendlyne and the rating agencies by name for better sourcing.
- Ask for a comparison table when the topic is a set of companies rather than one.
