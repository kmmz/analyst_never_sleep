---
name: company-deep-dive
description: >
  Conduct a comprehensive investment research deep dive on a public company or asset.
  Use this skill whenever the user names a company, ticker, or stock and wants to
  understand it — even if they phrase it as "tell me about X", "research X",
  "what do you think of X stock", "deep dive on X", "help me build a thesis on X",
  "write up on X", or "summarize the bull/bear case for X". Also trigger when the
  user asks for a company tearsheet, a one-pager, or fundamentals on a name. If the
  user pastes financial data or a filing excerpt and asks to analyze it, use this
  skill as the framework.
metadata:
  version: "0.2.0"
---

# Company Deep Dive

Produce a structured, analyst-grade research note on the requested company.

## Before Writing

1. Check the workspace for `schemas/research_schemas.md` — use the **Company Deep Dive** template as the canonical output format.
2. Check the workspace `knowledge-base/companies/` folder for an existing file on this ticker (e.g., `NVDA.md`). If one exists, read it first and incorporate prior notes into this update rather than starting from scratch.
3. Check `watchlist.json` — if the ticker is on the watchlist, read the thesis and level before writing. The memo should reinforce or challenge the existing thesis.

## Research Process

Use web search for current data, financials, and recent news. If a market data MCP is connected, pull live quotes, fundamentals, and sentiment. Work through each section systematically.

## Output Structure

Follow the schema in `schemas/research_schemas.md` exactly. Key sections to cover:

**Snapshot** — ticker, exchange, sector, market cap, current price and 52-week range.

**Business Model** — how the company makes money: revenue streams, customer segments, key competitive moats (pricing power, switching costs, network effects, scale), and what is structurally differentiated vs. peers.

**Financial Snapshot** — use the table format from the schema. Cover the last 2–3 years: revenue growth trajectory, margin structure (gross/EBITDA/net) and trend, free cash flow generation, balance sheet health, and capital allocation priorities.

**Valuation** — P/E, EV/EBITDA, P/FCF, EV/Sales vs. historical averages and peers. Include implied growth rate the market is pricing in.

**Bull Case** — three to four specific, data-grounded reasons the stock could outperform. Not generic; each tied to a concrete catalyst or fundamental driver.

**Bear Case** — three to four specific risks. Be honest. Flag competitive threats, execution risk, balance sheet concerns, macro sensitivity.

**Key Catalysts** — specific upcoming events or data points with expected timing.

**Macro Exposure** — rate sensitivity, FX exposure, cycle position.

**Bottom Line** — directional stance (bullish / bearish / neutral) and what would change the view. No price targets.

## After Writing

Save the output to `knowledge-base/companies/[TICKER].md` in the workspace folder. Use the file naming convention from the schema: `[TICKER]_deep_dive_YYYY-MM-DD.md` if saving a dated snapshot; overwrite `[TICKER].md` as the evergreen research file.

If the ticker is on the watchlist, offer to update the thesis field in `watchlist.json` to reflect any new insights.

## Style Guidelines

- Write in the style of a buy-side research note: precise, opinionated, and grounded in data
- Avoid hedging language — state the case clearly
- Flag data gaps or uncertainty explicitly rather than papering over them
- If the user provides a specific angle (e.g., "focus on the China risk"), weight the write-up accordingly
