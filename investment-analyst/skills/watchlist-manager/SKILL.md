---
name: watchlist-manager
description: >
  Manage the AnalystNeverSleep watchlist via natural language. Handles all CRUD operations
  on watchlist.json and watchlist.md: add tickers, remove tickers, change levels, update
  theses, edit tags, refresh prices, and show the current watchlist. Always reads the live
  watchlist.json before making any changes to preserve existing data.
compatibility: "claude-sonnet-4, claude-opus-4, claude-sonnet-3-7, claude-haiku-3-5"
metadata:
  version: "0.2.0"
  triggers: "add ticker to watchlist, remove ticker, move ticker to level, update thesis, refresh watchlist, show watchlist, watchlist status"
---

# Watchlist Manager Skill

You are managing the AnalystNeverSleep portfolio watchlist. The canonical data store is:
- **JSON (source of truth):** `watchlist.json` in the workspace folder
- **Markdown (rendered view):** `watchlist.md` in the workspace folder

Both files must be kept in sync on every write operation.

## File Locations

The watchlist files are in the workspace folder (the folder the user has selected in Cowork). Use the Read tool to locate them — do not hardcode any session paths. If unsure, `ls` the workspace root and look for `watchlist.json`.

## Initialization

When the skill is first loaded or the user asks to "initialize watchlist", check whether `watchlist.json` exists and is populated. If it is empty or missing, create it with the default schema (see Default Schema below). If it already exists, load and display the current state.

## Supported Operations

### ADD a ticker
**Triggers:** "add [TICKER]", "add [TICKER] to my watchlist as Level [N]", "track [TICKER]"

Steps:
1. Read current `watchlist.json`
2. Use `find_companies` MCP tool to get the `rp_entity_id` for the ticker (required for Bigdata lookups)
3. Use `bigdata_company_tearsheet` to pull current price, market cap, P/E, next earnings
4. Ask user (or infer from context): Level (1/2/3), thesis, tags
5. Append the new entry to the `watchlist` array in `watchlist.json`
6. Update `last_updated` field to today's date
7. Regenerate `watchlist.md` to reflect the addition

Entry template:
```json
{
  "ticker": "TICKER",
  "name": "Full Company Name",
  "rp_entity_id": "XXXXXX",
  "level": 2,
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
  "thesis": "One-sentence thesis.",
  "notes": "",
  "added": "YYYY-MM-DD",
  "last_updated": "YYYY-MM-DD"
}
```

For dual-listed stocks (e.g., ASML on NASDAQ + EURONEXT), add secondary exchange fields:
`"secondary_exchange"`, `"secondary_timezone"`, `"secondary_open"`, `"secondary_close"`

Exchange timezone reference:
- NASDAQ/NYSE → `America/New_York`, open 09:30, close 16:00, pre_market 04:00, after_hours 20:00
- EURONEXT → `Europe/Amsterdam`, open 09:00, close 17:30
- LSE → `Europe/London`, open 08:00, close 16:30
- SGX → `Asia/Singapore`, open 09:00, close 17:00
- TSE → `Asia/Tokyo`, open 09:00, close 15:30
- HKEX → `Asia/Hong_Kong`, open 09:30, close 16:00

---

### REMOVE a ticker
**Triggers:** "remove [TICKER]", "drop [TICKER] from watchlist", "delete [TICKER]"

Steps:
1. Read current `watchlist.json`
2. Find and remove the entry where `ticker == TICKER` (case-insensitive)
3. Update `last_updated`
4. Write back `watchlist.json`
5. Regenerate `watchlist.md`
6. Confirm removal to user

---

### MOVE / RECLASSIFY a ticker
**Triggers:** "move [TICKER] to Level [N]", "reclassify [TICKER] as Level [N]", "upgrade [TICKER]", "downgrade [TICKER]"

Steps:
1. Read current `watchlist.json`
2. Find the entry and update `"level"` field
3. Update `last_updated`
4. Write back `watchlist.json`
5. Regenerate `watchlist.md`
6. Confirm with: "[TICKER] moved to Level [N]"

---

### UPDATE THESIS
**Triggers:** "update my thesis on [TICKER]", "change thesis for [TICKER]", "my thesis on [TICKER] is now..."

Steps:
1. Read current `watchlist.json`
2. Find the entry and update the `"thesis"` field with the new text
3. Update `last_updated` on the entry and the top-level `last_updated`
4. Write back `watchlist.json`
5. Update the thesis line in `watchlist.md` for that ticker
6. Confirm update

---

### UPDATE TAGS
**Triggers:** "tag [TICKER] with [tags]", "add tag [tag] to [TICKER]", "remove tag [tag] from [TICKER]"

Steps:
1. Read current `watchlist.json`
2. Find the entry and modify the `"tags"` array (add or remove as instructed)
3. Write back `watchlist.json`
4. Update the tags line in `watchlist.md`
5. Confirm

---

### UPDATE NOTES
**Triggers:** "add note to [TICKER]", "note on [TICKER]", "update notes for [TICKER]"

Update the `"notes"` field for that ticker entry.

---

### SHOW / DISPLAY WATCHLIST
**Triggers:** "show my watchlist", "what's in my watchlist", "watchlist status", "portfolio overview"

Steps:
1. Read `watchlist.json`
2. If price data is stale (> 1 day) or user says "refresh", pull fresh tearsheets from Bigdata
3. Render a concise summary grouped by Level 1 / Level 2 / Level 3
4. Highlight any earnings in the next 30 days with urgency flags 🔴 (<7d) 🟡 (7–21d) 🟢 (>21d)

Summary format per ticker (one line):
`[TICKER] $PRICE | Mkt Cap $XB | P/E Xx | Earnings: DATE | [tags]`

---

### REFRESH PRICES
**Triggers:** "refresh my watchlist", "update watchlist prices", "pull latest prices"

Steps:
1. Read `watchlist.json` to get all `rp_entity_id` values
2. Call `bigdata_company_tearsheet` for each ticker (run in parallel where possible)
3. Update `watchlist.md` with fresh prices and the new refresh timestamp
4. Note: `watchlist.json` stores static metadata — live prices are rendered into `watchlist.md` only

---

## Level Definitions

| Level | Name | Description |
|-------|------|-------------|
| 1 | Core Holdings | High-conviction, closest monitoring, largest position sizing |
| 2 | Active Positions | Building or actively sizing; regular monitoring |
| 3 | Speculative / Watchlist | Early-stage, higher risk, or thesis under development |

---

## Output Style

- Confirm every action with a one-line summary of what changed
- After any write operation, always state: "watchlist.json and watchlist.md updated"
- For SHOW operations, render a clean table grouped by level
- Flag upcoming earnings prominently
- Never ask for confirmation unless removing a Level 1 holding (high-impact action)

---

## Default Schema (for initialization)

```json
{
  "schema_version": "1.1",
  "last_updated": "YYYY-MM-DD",
  "description": "AnalystNeverSleep investment watchlist",
  "exchange_reference": {
    "NASDAQ": { "timezone": "America/New_York", "open": "09:30", "close": "16:00", "pre_market": "04:00", "after_hours": "20:00", "utc_offset_summer": -4, "utc_offset_winter": -5 },
    "NYSE":   { "timezone": "America/New_York", "open": "09:30", "close": "16:00", "pre_market": "04:00", "after_hours": "20:00" },
    "EURONEXT": { "timezone": "Europe/Amsterdam", "open": "09:00", "close": "17:30", "utc_offset_summer": 2, "utc_offset_winter": 1 },
    "LSE":    { "timezone": "Europe/London", "open": "08:00", "close": "16:30", "utc_offset_summer": 1, "utc_offset_winter": 0 },
    "SGX":    { "timezone": "Asia/Singapore", "open": "09:00", "close": "17:00", "utc_offset": 8 },
    "TSE":    { "timezone": "Asia/Tokyo", "open": "09:00", "close": "15:30", "utc_offset": 9 },
    "HKEX":   { "timezone": "Asia/Hong_Kong", "open": "09:30", "close": "16:00", "utc_offset": 8 }
  },
  "watchlist": []
}
```
