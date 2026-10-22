# Contributing

Thanks for wanting to add to this. The bar is simple: a prompt should be something you have actually used and found useful.

## How to add a prompt

1. Pick the right folder: `prompts/analysis`, `prompts/mcp`, or `prompts/algo-trading`.
2. Create a markdown file with a short, clear name.
3. Follow the house format used by the existing files:
   - A one line summary of what it does.
   - The MCP or tools it needs.
   - A short how to use.
   - The master prompt itself, inside a fenced code block.
   - Any notes or tips at the end.
4. Add a row for it in the table in `README.md`.

## Rules

- Never include an API key, secret, access token, or any real credential. Use placeholders that read from a local `.env`.
- Trading prompts must default to a dry run or paper mode and must carry a clear risk note.
- Keep it readable. Write for a person who is skilled but busy.

Open a pull request and describe what the prompt is for and how you tested it.
