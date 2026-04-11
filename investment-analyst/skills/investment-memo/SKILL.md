---
name: investment-memo
description: >
  Write a formal investment memo or one-pager for a position or trade idea.
  Use this skill when the user wants to write up their investment thesis as a
  structured document — "write an investment memo on X", "draft a one-pager for X",
  "help me write up my thesis on X", "I want to present my idea on X", or similar.
  Also trigger when preparing for a portfolio review, an investment committee
  presentation, or when the user has done their research and wants to crystallize
  it into a shareable document.
metadata:
  version: "0.2.0"
---

# Investment Memo

Produce a formal investment memo that could be shared in a portfolio review, investment committee, or with a collaborator.

## Before Writing

1. Check the workspace for `schemas/research_schemas.md` — use the **Investment Memo** template as the canonical output format.
2. Check `knowledge-base/companies/[TICKER].md` for prior research on this name. If a deep dive exists, use it as source material — do not repeat research already done.
3. Check `watchlist.json` — if the ticker is on the watchlist, read the thesis, level, and any target/stop prices. Incorporate these into the memo's sizing and exit criteria sections.

## Gather Context First

Before writing, confirm (if not already clear from conversation):
1. The company / ticker / asset
2. Long or short
3. Target holding period (tactical / position / long-term hold)
4. Any constraints — portfolio mandate, risk limits, sizing context
5. Key pieces of the thesis the user already has in mind

If the user has already shared research in the conversation, use it. Do not ask for information already provided.

## Memo Structure

Follow the **Investment Memo** schema in `schemas/research_schemas.md`. Key sections:

**Header Block** — company, ticker, sector, direction, market cap, current price, date, and author.

**Executive Summary** — two to three sentences. The whole pitch in a paragraph. State the thesis in plain language: what is the market missing, and why does that create an opportunity?

**Investment Thesis** — three to four numbered reasons, each grounded in specific data. Include: *Why Does This Opportunity Exist?* — what is the market missing or mispricing?

**Financials & Valuation** — key metrics plus the math behind the price target or return expectation. Show your work.

**Catalysts** — specific events or data points that will close the gap, with expected timing.

**Key Risks** — three to four honest risks, each with: what it is, estimated probability, and downside impact. Do not downplay risks.

**Exit Criteria** — two entries: (1) Stop: the condition that means the thesis is wrong and the position should be cut; (2) Target: the condition that means the thesis has played out and it's time to take profit.

## After Writing

Save the memo to `knowledge-base/theses/[TICKER]_investment_memo_YYYY-MM-DD.md` in the workspace.

If the ticker is not already on the watchlist, offer to add it with the thesis summary and relevant price levels from the memo.

## Style Guidelines

- Confident, direct, and evidence-based — every claim grounded in a specific data point
- Avoid jargon without explanation — readable by a sophisticated non-expert
- Length: one to two pages when printed. Be disciplined.
- For a shorter one-pager: compress to Executive Summary, Investment Thesis, Key Risks, and Exit Criteria only
