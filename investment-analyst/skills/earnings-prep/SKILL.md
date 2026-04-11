---
name: earnings-prep
description: >
  Prepare for an upcoming earnings event or analyze a just-released earnings report.
  Use this skill whenever the user mentions earnings — "earnings prep for X",
  "X reports this week", "help me prepare for [company] earnings", "what should I
  watch in X's earnings", "X just reported, what happened?", "analyze X's Q[N]
  results", "post-earnings on X", or any variation. Also trigger if the user asks
  for a question list for an earnings call, wants to build a pre-earnings
  expectations framework, or wants to understand what the market is pricing in
  ahead of results.
metadata:
  version: "0.2.0"
---

# Earnings Prep & Recap

Produce an analyst-grade earnings briefing — either pre-earnings (what to expect) or post-earnings (what happened and what it means).

Determine from context whether this is **pre-earnings prep** or a **post-earnings recap**, and use the appropriate structure.

## Before Writing

1. Check the workspace for `schemas/research_schemas.md` — use the **Earnings Preview** or **Earnings Recap** template as the canonical output format.
2. Check `watchlist.json` — if the ticker is on the watchlist, read the current thesis and any prior notes before writing. The earnings assessment should explicitly address whether the print supports or challenges the thesis.
3. Check `knowledge-base/companies/[TICKER].md` for prior deep dive notes and `knowledge-base/earnings/` for prior earnings files on this name.

---

## Pre-Earnings Prep

Use when the user wants to prepare before results are released.

Follow the **Earnings Preview** schema from `schemas/research_schemas.md`. Key sections:

**The Setup** — report date, time (AMC/BMO), current consensus estimates for revenue, EPS, and any company-specific KPIs, plus what guidance the company gave last quarter.

**What the Market Is Pricing In** — implied move from options (use market data MCP if available), how the stock has traded into earnings, current sentiment positioning.

**Key Questions for the Call** — five to seven specific, pointed questions this earnings call needs to answer for the thesis. Not generic ("how was revenue?") but precise ("did North America revenue growth reaccelerate above the 8% implied by management's Q3 guide?").

**Consensus Expectations Table** — revenue, EPS, key KPIs with consensus, prior quarter, and YoY comparison.

**Scenario Framework** — beat & raise / in-line / miss scenarios with expected stock reaction for each.

**My Going-In View** — pre-print thesis and where the user's expectations sit vs. consensus.

---

## Post-Earnings Recap

Use when results have already been reported.

Follow the **Earnings Recap** schema from `schemas/research_schemas.md`. Key sections:

**The Print** — reported vs. consensus table: revenue, EPS, key KPIs. Beat/miss/in-line at a glance.

**Guidance** — next quarter and full year guidance vs. prior, and vs. what the Street expected.

**Key Call Takeaways** — most important commentary from prepared remarks and Q&A. What did they avoid or deflect?

**Why the Stock Moved** — one to two sentences on the actual driver of the reaction (not just the headline number).

**Thesis Update** — explicitly state: does this print strengthen, weaken, or leave unchanged the investment thesis? Flag which assumptions need revision.

---

## After Writing

Save to `knowledge-base/earnings/` using the naming convention from the schema:
- Pre-earnings: `[TICKER]_earnings_pre_YYYY-MM-DD.md`
- Post-earnings: `[TICKER]_earnings_post_YYYY-MM-DD.md`

If the ticker is on the watchlist, offer to update the thesis or notes field in `watchlist.json` to reflect the new information.
