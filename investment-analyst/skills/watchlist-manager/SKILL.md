---
name: watchlist-manager
description: >
  Manage a personal investment watchlist via natural language. Use this skill
  whenever the user wants to add a ticker to their watchlist, remove a name,
  update a price target or thesis, check what's on their watchlist, or see how
  their tracked names are performing. Trigger phrases include: "add X to my
  watchlist", "remove X from my list", "show me my watchlist", "update my thesis
  on X", "what am I watching?", "refresh my watchlist prices", or "what levels am
  I watching for X".
metadata:
  version: "0.2.0"
---

# Watchlist Manager

Maintain the user's investment watchlist. The watchlist is stored in two files in the workspace folder:
- `watchlist.json` — the structured data file (source of truth)
- `watchlist.md` — a human-readable markdown version for quick reference

Always read `watchlist.json` before making any changes. Never overwrite without reading first.

## Data Schema

The JSON schema matches the following structure. Preserve all existing fields when updating:

```json
{
  "schema_version": "1.1",
  "last_updated": "YYYY-MM-DD",
  "description": "...",
  "exchange_reference": { ... },
  "watchlist": [
    {
      "ticker": "AAPL",
      "name": "Apple Inc.",
      "rp_entity_id": "",
      "level": 1,
      "tags": ["tag1", "tag2"],
      "sector": "Technology",
      "country": "United States",
      "exchange": {
        "primary": "NASDAQ",
        "timezone": "America/New_York",
        "open": "09:30",
        "close": "16:00",
        "pre_market": "04:00",
        "after_hours": "20:00"
      },
      "thesis": "One to two sentence thesis summary.",
      "notes": "",
      "added": "YYYY-MM-DD",
      "last_updated": "YYYY-MM-DD"
    }
  ]
}
```

**Level definitions:**
- Level 1 — Core / high-conviction active holdings or imminent entries
- Level 2 — Active holdings or names under close watch
- Level 3 — Watching / speculative / early-stage interest

**Exchange reference:** Populate `exchange.primary` correctly (NASDAQ, NYSE, EURONEXT, SGX, etc.). Use the `exchange_reference` block already in the file for timezone and hours data.

## Operations

### Show Watchlist
Display all tickers grouped by level in a clean markdown table. Columns: Ticker, Name, Sector, Direction (if tracked), Thesis (one line). If a market data MCP is connected, add a current price column and flag any names that have moved significantly or are near key levels.

### Add a Ticker
Collect: ticker symbol, company name, sector, country, exchange, direction (long/short/neutral), level (1–3), and thesis (one to two sentences). Tags are optional — suggest relevant ones based on sector and thesis. Confirm the full entry with the user before writing. Append to the existing `watchlist` array — never replace it.

### Remove a Ticker
Confirm the ticker exists. Ask for confirmation before deleting. Remove from the JSON array, update `last_updated`, and sync `watchlist.md`.

### Update an Entry
Read the current entry first. Apply only the changed fields. Preserve all other fields. Update `last_updated` on the modified entry and the top-level `last_updated` field to today's date.

### Refresh Prices
If a market data MCP is connected, fetch current prices for all tickers and display a performance summary. Highlight any names that have moved more than 5% since last noted, or that are approaching key price levels from prior research.

## After Every Write Operation

After updating `watchlist.json`, regenerate `watchlist.md` as a clean, readable markdown file. The `.md` should:
- Group tickers by level with a header for each level
- Include a table with: Ticker, Name, Sector, Thesis
- Include a timestamp at the top: `*Last updated: YYYY-MM-DD*`
- Be scannable in 30 seconds — no raw JSON, no excessive detail

## Style Guidelines

- Always read `watchlist.json` before writing — never blindly overwrite
- Keep thesis entries in JSON to two sentences maximum; offer to write more detail in conversation if wanted
- If the user hasn't specified a level, ask — don't default silently
- When adding from an investment memo or deep dive already done in session, pre-populate fields from that research rather than asking the user to repeat themselves
