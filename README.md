<div align="center">

# MCP Market Prompts

**A curated set of master prompts that turn Claude into a markets research analyst and a trading systems engineer, powered by MCP.**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
![Prompts](https://img.shields.io/badge/prompts-8-blue.svg)
![Focus](https://img.shields.io/badge/focus-markets%20%7C%20research%20%7C%20algo-orange.svg)

</div>

---

Most people use an AI chat like a search box. Connect it to the right MCP server and it becomes something closer to a junior analyst that can read ten financial websites for you, review your live portfolio, or build and paper trade a strategy on your broker API.

This repo is a collection of the master prompts I actually use for that. Each one is self contained. Copy the block, connect the MCP it asks for, and paste it into Claude Desktop or Claude Code. Every prompt is written to make Claude confirm its understanding and its assumptions before it does anything.

## What is inside

### Research and analysis

| Prompt | What it does | Needs |
| --- | --- | --- |
| [360 Stock Research](prompts/analysis/stock-360-research.md) | A full view of any listed company across ten research sites, written for a human, not an analyst | Firecrawl MCP |
| [Earnings Call Breakdown](prompts/analysis/earnings-call-breakdown.md) | Turns a concall transcript into guidance, risks, numbers and management tone | File or Firecrawl MCP |
| [Peer Comparison](prompts/analysis/peer-comparison.md) | Compares a company to its real peers on the ratios that matter for its business type | Firecrawl MCP |

### MCP powered workflows

| Prompt | What it does | Needs |
| --- | --- | --- |
| [Portfolio Review](prompts/mcp/kite-portfolio-review.md) | Reads your live holdings and gives a concentration, risk and diversification read | Zerodha Kite MCP |
| [Deep Research](prompts/mcp/firecrawl-deep-research.md) | A cited, multi source research report on any company, sector or theme | Firecrawl MCP |

### Algo trading, educational

| Prompt | What it does | Needs |
| --- | --- | --- |
| [Short Straddle Bot](prompts/algo-trading/short-straddle-bot.md) | Builds and paper trades a NIFTY monthly ATM short straddle with full risk controls | Kite Connect API, Claude Code |
| [Iron Condor Bot](prompts/algo-trading/iron-condor-bot.md) | A defined risk version, with wings, that caps the loss | Kite Connect API, Claude Code |

Read the [disclaimer](DISCLAIMER.md) before you touch the trading section.

### Charting and scanning

| Prompt | What it does | Needs |
| --- | --- | --- |
| [Crypto Renko and PnF Scanner](prompts/charting/crypto-renko-pnf.md) | Builds log based Renko and PnF charts for crypto, marks patterns, ranks symbols by relative strength versus BTC. Analysis only, no orders | Python, Claude Code |

## How to use

1. Open the prompt you want and copy the code block.
2. Connect the MCP it lists. For the Firecrawl prompts you need a Firecrawl MCP server, for the portfolio prompt you need the Zerodha Kite MCP, and the trading bots need the Kite Connect API through Claude Code.
3. Paste the prompt. Answer the clarifying questions it asks before it runs.
4. For anything that touches real money, keep it in dry run until you have watched it behave on paper.

## A note on MCP

MCP, the Model Context Protocol, is how an AI connects to outside tools and data in a structured way. A Kite MCP lets Claude read your positions. A Firecrawl MCP lets it read and search the live web. The prompts here assume you have the relevant server connected, and each one says which.

## Contributing

New prompts and improvements are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md). Keep the same format, state the MCP a prompt needs, and never commit an API key or a live token.

## License

[MIT](LICENSE). Use them, change them, share them.
