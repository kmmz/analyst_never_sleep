---
name: company-deep-dive
description: >
  Conduct a comprehensive investment research deep dive on a public company or asset.
  Use this skill whenever the user names a company, ticker, or stock and wants to
  understand it — even if they phrase it as "tell me about X", "research X", "what do
  you think of X stock", "deep dive on X", "help me build a thesis on X", "write up
  on X", or "summarize the bull/bear case for X". Also trigger when the user asks
  for a company tearsheet, an investment memo, a one-pager, or fundamentals on a name.
  If the user pastes financial data or a filing excerpt and asks you to analyze it,
  use this skill as your framework.
metadata:
  version: "0.2.0"
---

# Company Deep Dive

Produce a structured investment research brief on a public company. The output should read like a well-organized analyst note: clear thesis, grounded in data, with explicit bull and bear cases and a bottom line.

## Before Writing

1. Check the workspace for `schemas/research_schemas.md` — use the **Company Deep Dive** template as the canonical output format if it exists.
2. Check `knowledge-base/companies/` for an existing file on this ticker (e.g., `NVDA.md`). If one exists, read it and incorporate prior notes into this update rather than starting from scratch.
3. Check `watchlist.json` — if the ticker is on the watchlist, read the thesis and level before writing. The deep dive should reinforce or explicitly challenge the existing thesis.

## Workflow

### 1. Establish scope

Confirm with the user (or infer from context):
- **Company name / ticker** — required
- **Research angle**: is this a fresh look, or are they stress-testing an existing thesis?
- **Depth**: quick tearsheet (15 min) vs. full deep dive (comprehensive)?
- **Market cap / sector** — note it, it shapes what's material

### 2. Research — fetch in parallel

Use multiple sources simultaneously. Prioritize recency and primary sources.

**Business fundamentals:**
- What does the company actually do? Revenue model, customers, products
- Web search: `"[company] business model investor presentation"`
- If MCPs available: Bigdata (`bigdata_company_tearsheet`), Quartr (`get_company`, `list_documents`), Fiscal.ai (`get_company_fundamentals`), Aiera (`get_equity_summaries`)

**Financial snapshot:**
- Revenue growth, gross margin, EBITDA/net income, FCF, debt/equity, cash
- Last 2–3 years + most recent quarter
- Web search: `"[ticker] financials revenue earnings"`
- Pull annual report or 10-K summary if accessible

**Valuation:**
- Current price, market cap, EV
- P/E, EV/EBITDA, P/FCF, P/S vs. sector comps
- Web search: `"[ticker] valuation multiples peers"`

**Recent developments:**
- Last earnings results vs. consensus
- Management changes, acquisitions, guidance updates
- Web search: `"[company] latest news site:bloomberg.com OR site:reuters.com OR site:wsj.com"` or recent dates

**Competitive position:**
- Who are the main competitors?
- What's the moat (if any)?

**Macro / sector context:**
- Any tailwinds or headwinds from macro (rates, FX, commodity exposure)?
- Sector cycle position

### 3. Write the deep dive

Use this structure:

```
# [Company Name] ([TICKER]) — Investment Deep Dive
*As of [date] | [Sector] | Market Cap: $Xbn*

## TL;DR
[2–3 sentence summary: what the company does, current situation, overall stance. This is the one paragraph someone reads if they have 30 seconds.]

## Business Overview
[What does it do? How does it make money? Key products/segments. Who are the customers. 3–5 bullet points or 1 short paragraph.]

## Financial Snapshot
| Metric | FY[N-2] | FY[N-1] | FY[N] | LTM / Most Recent |
|--------|---------|---------|-------|-------------------|
| Revenue ($M) | | | | |
| Rev Growth (%) | | | | |
| Gross Margin (%) | | | | |
| EBITDA Margin (%) | | | | |
| FCF ($M) | | | | |
| Net Debt ($M) | | | | |

## Valuation
| Multiple | Current | Sector Avg | Notes |
|----------|---------|------------|-------|
| P/E | | | |
| EV/EBITDA | | | |
| P/FCF | | | |

[1–2 sentences interpreting the valuation: cheap, fair, stretched? vs. what?]

## Competitive Position
[Moat assessment. Key competitors. Market share dynamics. 3–5 bullets.]

## Bull Case 🟢
[3 specific, quantifiable reasons to own it. Each should be testable.]

## Bear Case 🔴
[3 specific, quantifiable risks. Include what would need to happen for each to materialize.]

## Key Catalysts to Watch
[Upcoming events, data points, or decisions that could re-rate the stock. Include approximate timing where known.]

## Macro Exposure
[How does the macro environment (rates, USD, cycle) affect this name? Short paragraph.]

## Bottom Line
[Investment stance in 2–3 sentences. What would change the view?]
```

### 4. Quality checks before finalizing

- Every number should have a source (year, filing, or search result)
- Bull and bear cases should be specific, not vague ("execution risk" is not a bear case)
- Flag any data gaps explicitly rather than glossing over them
- If valuation data is unavailable, say so and note what the analyst should look up

## After Writing

Save to `knowledge-base/companies/[TICKER].md` as the evergreen research file. Use `[TICKER]_deep_dive_YYYY-MM-DD.md` for dated snapshots.

If the ticker is on the watchlist, offer to update the thesis field in `watchlist.json` to reflect any new insights from this research.
