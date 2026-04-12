---
name: investment-memo
description: >
  Write a formal investment memo or one-pager for a position or trade idea.
  Use this skill when the user wants to write up their investment thesis as a
  structured document — "write an investment memo on X", "draft a one-pager for X",
  "help me write up my thesis on X", "I want to present my idea on X", or similar.
  Also trigger when preparing for a portfolio review, an investment committee presentation,
  or when the user has done their research and wants to crystallize it into a shareable document.
metadata:
  version: "0.2.0"
---

# Investment Memo

Transform research and analysis into a structured investment memo or one-pager.

## When to use

This skill is for when the analyst has already done the research (or has a view) and needs to write it up. If they're still in the discovery phase, use the `company-deep-dive` skill first.

## Before Writing

1. Check the workspace for `schemas/research_schemas.md` — use the **Investment Memo** template as the canonical output format if it exists.
2. Check `knowledge-base/companies/[TICKER].md` for prior research on this name. If a deep dive exists, use it as source material — do not repeat research already done.
3. Check `watchlist.json` — if the ticker is on the watchlist, read the thesis, level, and any target/stop prices. Incorporate these into the memo's sizing and exit criteria sections.

## Workflow

### 1. Gather inputs
Ask for (or infer from context):
- Company / ticker
- Thesis summary (what's the core idea?)
- Time horizon (trade vs. long-term hold)
- Any research they've already done (paste or reference)
- Audience (internal IC, external LP, personal notes)

### 2. Write the memo

Follow the **Investment Memo** schema from `schemas/research_schemas.md`. Key sections:

```
# Investment Memo: [Company Name] ([TICKER])
**Date:** [Date]  
**Analyst:** [Name if provided]  
**Recommendation:** [Buy / Sell / Hold / Initiate]  
**Price Target:** $X ([N]% upside/downside)  
**Time Horizon:** [e.g., 12 months]  
**Position Size:** [e.g., 2–3% of portfolio — if relevant]

---

## Executive Summary
[3–4 sentences: what's the opportunity, why does it exist, what's the thesis in plain English.]

## Company Overview
[2–3 sentences: what they do, key metrics, market position. Reader should orient quickly.]

## Investment Thesis
[The core argument. 3–5 specific reasons this is an opportunity. Each reason should be:
- Grounded in a data point or observable fact
- Differentiated from consensus (why does the market have it wrong?)
- Testable (what would prove or disprove this?)]

### Why Does This Opportunity Exist?
[What is the market missing? Misunderstood growth, overlooked catalyst, sentiment overshoot, structural change not yet priced?]

## Financials & Valuation
[Key financial metrics table + valuation. Include target multiple and how you get to the price target.]

## Catalysts
[What will close the gap between current price and target? Include timing where possible.]

## Key Risks
[3–5 specific risks. For each: what's the risk, how likely, what's the downside scenario?]

## Variant View
[What does the market think? How is this thesis different? Why are you right and the market is wrong (or early)?]

## Exit Criteria
**Stop:** [The condition that means the thesis is wrong — cut the position]  
**Target:** [The condition that means the thesis has played out — take profit]
```

### 3. Tone

- Write for a skeptical, intelligent reader who hasn't followed this name
- Every claim should be backed by a number or fact
- Avoid buzzwords ("innovative", "disruptive") — say specifically what makes them good
- Be direct: "Buy at $X, target $Y, stop at $Z"
- Length: one to two pages when printed. Be disciplined.

## After Writing

Save to `knowledge-base/theses/[TICKER]_investment_memo_YYYY-MM-DD.md` in the workspace.

If the ticker is not already on the watchlist, offer to add it with the thesis summary and relevant price levels from the memo.
