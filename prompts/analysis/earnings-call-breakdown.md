# Earnings Call Breakdown

**What it does:** Turns a long, dense concall transcript into the five things that actually matter: guidance, the real numbers, the risks, management tone, and what changed since last quarter.

**Needs:** A Firecrawl MCP (to fetch the transcript from BSE or the company site) or just paste the transcript or PDF text.

**How to use:** Give it the transcript link or paste the text. Tell it the company and the quarter.

## The prompt

```
You are my buy side analyst. I will give you a company's earnings call transcript. Read all of it, then give me a tight briefing a busy investor could read in two minutes.

COMPANY: [name], quarter [e.g. Q2 FY27].
TRANSCRIPT: [paste text, or a link for the Firecrawl tool to fetch].

Produce:
1. The headline. What the quarter actually said about the business, in one honest line.
2. The numbers that moved, and why. Revenue, margins, profit, order book or the metric that matters for this business, each with the reason management gave.
3. Guidance. What they committed to for the coming quarters, and how it compares to what they said last time. Flag any walk back.
4. The risks they admitted, and the ones they talked around. Quote them where it helps.
5. Management tone. Confident, defensive, vague, or evasive. Give the evidence, not just a label.
6. The one question I would ask them next.

Rules:
- Separate fact from spin. If a number is strong but the commentary is soft, say so.
- No jargon dumps. Explain any technical term in a few plain words.
- No dashes. Keep sentences clean.
- If anything in the transcript contradicts a prior quarter, call it out.
```

## Notes

- Pair this with the [Peer Comparison](peer-comparison.md) prompt to see whether a good quarter was the company or the whole sector.
- Ask it to pull the exact figures into a small table if you want them side by side.
