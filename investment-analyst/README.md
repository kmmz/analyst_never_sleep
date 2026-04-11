# Investment Analyst Plugin

A full investment research toolkit for Cowork. Covers the core workflows of an equity and macro analyst — from morning briefings to deep dives, earnings prep, investment memos, and watchlist tracking.

---

## Skills

| Skill | What it does | How to trigger |
|---|---|---|
| **Company Deep Dive** | Full research note on any stock — business model, financials, bull/bear case, key metrics | "Deep dive on NVDA", "research Apple", "what's the bull case for TSLA?" |
| **Daily Market Briefing** | Structured morning note covering macro, rates, what's moving, and key events today | "Give me my morning briefing", "what's moving today?", "market update" |
| **Earnings Prep & Recap** | Pre-earnings framework (key questions, what to watch) or post-earnings analysis | "Earnings prep for META", "MSFT just reported — what happened?", "help me prep for AMZN earnings" |
| **Investment Memo** | Formal one-pager or IC memo for a long or short idea | "Write an investment memo on UBER", "draft a one-pager for my short on X" |
| **Watchlist Manager** | Add, remove, update, and view your personal ticker watchlist | "Add NVDA to my watchlist", "show me what I'm watching", "update my thesis on AMD" |

---

## Recommended Data Connectors (MCPs)

The skills work with Claude's built-in web search, but connecting a market data MCP will significantly improve the quality of live prices, fundamentals, sentiment, and events data. Here are the most useful connectors to set up:

### Tier 1 — Strongly Recommended

**BigData MCP**
The best all-in-one source for equity research in Cowork. Provides company tearsheets, market data, ETF data, sentiment analysis, events calendar, and company search.
- Install via the Cowork plugin marketplace or MCP registry
- No API key required for basic access

**Polygon.io / Alpha Vantage**
For real-time and historical price data, options pricing (useful for implied move calculations in earnings prep), and intraday quotes.
- Requires a free or paid API key from polygon.io or alphavantage.co
- Set environment variable: `POLYGON_API_KEY` or `ALPHAVANTAGE_API_KEY`

### Tier 2 — Useful Additions

**Financial Modeling Prep (FMP)**
Deep fundamental data: income statements, balance sheets, cash flow, ratios, and analyst estimates going back many years. Very useful for the financial profile section of deep dives.
- API key required: financialmodelingprep.com
- Set environment variable: `FMP_API_KEY`

**Benzinga / NewsAPI**
For real-time financial news and headlines — powers the "What's Moving" section of the daily briefing.
- API key required from the respective provider
- Set environment variable: `BENZINGA_API_KEY` or `NEWS_API_KEY`

### Tier 3 — Nice to Have

**FRED (Federal Reserve Economic Data)**
Macroeconomic time series — rates, inflation, GDP, employment. Useful for macro context in briefings.
- Free, no key required for many endpoints

**SEC EDGAR**
Direct access to company filings (10-K, 10-Q, 8-K). Useful when you want to go to the primary source in a deep dive.
- Free, no key required

---

## Watchlist Storage

The Watchlist Manager skill stores your watchlist in a file called `watchlist.json` in your Cowork workspace folder. This file is created automatically the first time you use the watchlist. You can back it up, move it, or share it — it's just JSON.

---

## Tips

- **Start with the daily briefing** to orient yourself each morning before diving into names
- **Combine skills**: run a deep dive first, then use the investment memo skill to crystallize it into a shareable document
- **Earnings workflow**: add a name to your watchlist before earnings, do an earnings prep the day before, and run a post-earnings recap right after the print to capture your thoughts while they're fresh
- **The skills work best with context**: the more you tell Claude about your angle or concern, the more targeted the output

---

*Plugin created with Cowork. Share freely.*
