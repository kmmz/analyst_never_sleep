---
name: earnings-prep
description: >
  Prepare for an upcoming earnings event or analyze a just-released earnings report.
  Use this skill whenever the user mentions earnings — "earnings prep for X", "X reports
  this week", "help me prepare for [company] earnings", "what should I watch in X's
  earnings", "X just reported, what happened?", "analyze X's Q[N] results", "post-earnings
  on X", or any variation. Also trigger if the user asks for a question list for an
  earnings call, wants to build a pre-earnings expectations framework, or wants to
  understand what the market is pricing in ahead of results.
metadata:
  version: "0.2.0"
---

# Earnings Prep

Help the analyst prepare for or analyze an earnings event. There are two modes — pre-earnings (prepare) and post-earnings (analyze). Detect which one the user needs, or ask if unclear.

## Before Writing

1. Check the workspace for `schemas/research_schemas.md` — use the **Earnings Preview** or **Earnings Recap** template as the canonical output format.
2. Check `watchlist.json` — if the ticker is on the watchlist, read the current thesis and any prior notes. The earnings assessment should explicitly address whether the print supports or challenges the thesis.
3. Check `knowledge-base/companies/[TICKER].md` for prior deep dive notes and `knowledge-base/earnings/` for prior earnings files on this name.
4. **Read `preferences.json`** in the workspace. If `timezone` is set (e.g. `"Asia/Singapore"`), convert the earnings call time and all event times to local time. Display as `HH:MM ET / HH:MM [TZ_ABBR]` wherever a time appears. If `timezone` is null or the file doesn't exist, use ET only and include a one-line note: *"💡 Set your timezone in preferences.json to see times in local time."*

---

## Mode A: Pre-Earnings Prep

Produce an expectations framework before the earnings release.

### Step 1: Gather context
- Company / ticker (required)
- Earnings date (look it up if not provided)
- User's current thesis or position (ask or pull from memory)
- Key questions they're going in with (optional)

### Step 2: Research — run in parallel

**Consensus expectations:**
- What is Wall Street expecting? EPS estimate, revenue estimate, key metrics
- Web search: `"[ticker] earnings estimates consensus Q[N] [year]"`
- If MCPs available: Bigdata (`bigdata_events_calendar`), Aiera (`get_event`, `get_equity_summaries`), Quartr (`list_events`, `get_event`)

**Recent company narrative:**
- What did management guide to last quarter?
- What are the 2–3 metrics management has flagged as most important?
- Web search: `"[company] last quarter guidance [recent date]"` or last earnings transcript

**What's priced in:**
- How has the stock moved into earnings? (vs. sector, vs. market)
- Implied move from options (if findable)
- Web search: `"[ticker] options implied move earnings"`

**Key debates:**
- What are the 2–3 things the market is most divided on?
- What would constitute a "beat" vs. "miss" in terms of market reaction (not just EPS)?

### Step 3: Write the pre-earnings brief

Follow the **Earnings Preview** schema from `schemas/research_schemas.md`. Key sections:

```
# [Company] ([TICKER]) — Earnings Preview
*[Quarter / Fiscal Period] | Earnings Date: [Date] [Time ET / Time LOCAL if timezone set] | Current Price: $X*

## The Setup
[2–3 sentences: what's the narrative going in? Is this a "show me" quarter, a recovery story, a guide-up event?]

## Consensus Expectations
| Metric | Consensus Est. | Prior Quarter Actual | YoY Change |
|--------|---------------|----------------------|------------|
| Revenue ($M) | | | |
| EPS | | | |
| [Key Metric 1] | | | |
| [Key Metric 2] | | | |

## What Management Guided To
[Bullet points from last quarter's guidance. Note any specific numbers or ranges.]

## The Key Questions
[5–7 questions the analyst should have answered by end of call. Ordered by importance. Each question should be specific and testable.]

1. 
2. 
3. 
...

## Scenario Framework
| Scenario | Condition | Expected Reaction |
|----------|-----------|-------------------|
| Beat & raise | Rev >X%, guide >Y | +5–8% |
| In-line | Meets estimates, guide flat | Flat to ±2% |
| Miss or guide down | Rev <X% or cuts guide | -8–12% |

[Adjust numbers to the specific situation.]

## My Going-In View
[Space for the analyst to record their thesis before the print — what do they think will happen vs. consensus?]
```

---

## Mode B: Post-Earnings Analysis

Rapidly synthesize what actually happened vs. what was expected.

### Step 1: Gather the results
- Pull actual results from web search: `"[ticker] Q[N] earnings results [date]"`
- If MCPs available: Quartr or Aiera for transcript/document access

### Step 2: Write the post-earnings summary

Follow the **Earnings Recap** schema from `schemas/research_schemas.md`. Key sections:

```
# [Company] ([TICKER]) — Earnings Recap
*[Quarter] | Reported: [Date] | Stock reaction: [+/-X%]*

## The Print vs. Expectations
| Metric | Actual | Consensus | Beat/Miss |
|--------|--------|-----------|-----------|
| Revenue ($M) | | | |
| EPS | | | |
| [Key Metric 1] | | | |
| [Key Metric 2] | | | |

## Guidance
[What did management guide for next quarter / full year? Any changes vs. prior guide?]

## Key Takeaways from the Call
[3–5 bullets. Focus on what was new, changed, or surprising — not what was expected.]

## Why the Stock Moved
[One to two sentences on the actual driver of the reaction — guidance and narrative delta matter more than headline numbers.]

## Thesis Update
[Explicitly state: does this print strengthen, weaken, or leave unchanged the investment thesis? Flag which assumptions need revision.]
```

---

## General guidance

- Always distinguish between an EPS/revenue "beat" and what actually drove stock movement — the market reacts to guidance and the delta in the narrative, not just the headline
- Flag any discrepancies between GAAP and non-GAAP metrics prominently
- If the call transcript is available (via MCPs), pull 2–3 verbatim management quotes on the most important topics

## After Writing

Save to `knowledge-base/earnings/` using the naming convention:
- Pre-earnings: `[TICKER]_earnings_pre_YYYY-MM-DD.md`
- Post-earnings: `[TICKER]_earnings_post_YYYY-MM-DD.md`

If the ticker is on the watchlist, offer to update the thesis or notes field in `watchlist.json` to reflect the new information.
