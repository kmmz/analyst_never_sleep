---
name: daily-market-briefing
description: >
  Generate a structured daily market briefing for investment analysts.
  Use this skill whenever the user asks for a morning briefing, market summary,
  daily rundown, overnight recap, what happened in markets, or wants to know
  what's moving today. Also trigger when the user says "daily brief", "morning
  brief", "what's moving", "market update", "macro update", or anything implying
  they want a structured daily digest of market conditions.
metadata:
  version: "0.2.0"
---

# Daily Market Briefing

Produce a concise, structured morning briefing covering macro moves, key themes, and what to watch.

## Before Writing

1. Check the workspace for `schemas/research_schemas.md` — use the **Daily Market Briefing** template as the canonical output format.
2. Check `watchlist.json` in the workspace. After the general market section, add a **Watchlist Pulse** section covering any notable moves, news, or catalysts specifically for tickers the user is tracking.
3. Check `knowledge-base/market-notes/` for any recent prior briefings for continuity of theme tracking.

## Research Process

Use web search to get current data. If a market data MCP is connected, pull live quotes, sentiment, and events calendar. Today's date is available in context — use it to identify what's on the economic calendar.

## Briefing Structure

Follow the schema in `schemas/research_schemas.md`. Key sections:

**Macro Pulse** — key index levels, 10Y/2Y yields (and curve shape), DXY, oil, gold, BTC. One-line implication for each.

**What to Watch Today** — economic data releases and events with consensus, prior, and what the surprise would mean. Flag the one or two that matter most.

**Equity Movers** — two to five names with notable moves (earnings reactions, analyst actions, news). Ticker, move size, one-sentence reason.

**Macro Themes in Play** — two to three sentences on the dominant market narrative. What are traders focused on? Where is the debate?

**Watchlist Pulse** — for each Level 1 ticker in `watchlist.json`, flag any overnight developments, analyst actions, or proximity to key price levels. Skip Level 3 unless something notable is happening.

**Analyst's Focus** — one to two specific things to watch given ongoing positions/thesis. Personalized, not generic.

## After Writing

Save the briefing to `knowledge-base/market-notes/` using the naming convention: `briefing_YYYY-MM-DD.md`.

## Style Guidelines

- Direct and opinionated — not a neutral data dump, but an analyst's read on what matters
- Keep section headers; write each section as prose, not bullet points where possible
- Total length: 400–700 words (excluding the Watchlist Pulse table)
- If market data MCP is available, open with live prices rather than estimates
