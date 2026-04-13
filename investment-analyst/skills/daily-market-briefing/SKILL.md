---
name: daily-market-briefing
description: >
  Generate a structured daily market briefing for equity and macro investment analysts.
  Use this skill whenever the user asks for a morning briefing, market summary, daily
  rundown, overnight recap, what happened in markets, or wants to know what's moving
  today. Also trigger when the user says "daily brief", "morning brief", "what's moving",
  "market update", "macro update", or anything implying they want a structured daily
  digest of market conditions before starting their research day.
metadata:
  version: "0.2.0"
---

# Daily Market Briefing

Produce a concise, structured morning market briefing for an equity and macro analyst. The goal is to surface the signals that matter — not rehash everything. A great briefing fits on one screen and tells the analyst exactly what to pay attention to today.

## Before Writing

1. Check the workspace for `schemas/research_schemas.md` — use the **Daily Market Briefing** template as the canonical output format if it exists.
2. Check `watchlist.json` in the workspace. After the general market section, add a **Watchlist Pulse** covering any notable moves, news, or catalysts for Level 1 tickers specifically.
3. Check `daily-summaries/` in the workspace for recent prior briefings — use them for continuity on themes already in play.
4. **Read `preferences.json`** in the workspace. If `timezone` is set (e.g. `"Asia/Singapore"`), use it to convert all event times in the briefing to local time. Show times as `HH:MM ET / HH:MM [TZ_ABBR]`. If `timezone` is null or the file doesn't exist, default to ET only and add a one-line prompt at the top of the briefing: *"💡 Tip: tell me your timezone once and I'll show all times in local time going forward."*

## Workflow

### 1. Gather inputs

Ask the user (or infer from context) the following if not already provided:
- Date/session (default: today)
- Any specific regions, sectors, or names they want highlighted
- Any ongoing positions or themes to watch (pull from memory if available)

### 2. Research — fetch in parallel

Use web search aggressively. Prioritize recency (last 24 hours). Search for:

- **Overnight macro**: US futures, Asian + European market closes, USD moves, rates (10Y, 2Y), commodities (oil, gold), crypto if relevant
- **Key data/events today**: Scheduled releases (CPI, PMI, jobs, Fed speakers, central bank decisions), earnings before/after market
- **Equity movers**: Pre-market movers >3%, notable gaps, sector rotations
- **Geopolitical / macro headlines**: Anything that could shift risk sentiment

Good search queries to run:
- `"market briefing [date]"` or `"morning market update [date]"`
- `"US futures premarket [date]"`
- `"economic calendar [date]"`
- `"earnings today [date]"`

If a market data MCP is connected (Bigdata, Quartr, Aiera, Fiscal.ai), use it for live quotes, sentiment, and events calendar. Otherwise fall back to web search.

### 3. Write the briefing

Use this exact structure:

```
# 📊 Daily Market Briefing — [Day, Date]

## 🌍 Macro Pulse
[3–5 bullet points. Cover: index direction, rates, USD, key commodity. One line each. Lead with the number, then the implication.]

## 📅 What to Watch Today
[Scheduled catalysts: data releases, Fed/central bank events, major earnings. Show times as "HH:MM ET / HH:MM LOCAL" if timezone is set in preferences.json, otherwise ET only. Brief note on expected impact for each.]

## 🔥 Equity Movers
[Top 5–7 pre-market movers. Ticker + move + one-line reason. Flag if it's sector-wide vs. company-specific.]

## 🧭 Macro Themes in Play
[2–3 sentences on the dominant narrative: are we risk-on/off? Any regime shift? What's the market pricing in?]

## 📌 Watchlist Pulse
[For each Level 1 ticker in watchlist.json: flag any overnight developments, analyst actions, or proximity to key levels. Skip Level 3 unless something notable is happening.]

## 👁 Analyst's Focus
[1–3 bullets on what specifically deserves attention today given the analyst's ongoing positions/themes. Personalized if context is available, generic if not.]
```

### 4. Tone and length

- **Concise**: each section should be skimmable in under 30 seconds
- **Lead with numbers**: "$DXY +0.4% to 104.2" beats "the dollar strengthened"
- **Flag risk clearly**: use ⚠️ for anything that could meaningfully shift positions
- **No filler**: skip sections where there's nothing material to say
- Total length: 400–700 words excluding the Watchlist Pulse table

## After Writing

Save to `daily-summaries/briefing_[YYYY-MM-DD].md` in the workspace. The `daily-summaries/` folder is the canonical home for all daily briefings — do not save to the workspace root or `outputs/` folder.
