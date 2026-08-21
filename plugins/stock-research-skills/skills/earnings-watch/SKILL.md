---
name: earnings-watch
description: Track upcoming quarterly results dates for the user's stock holdings and review results already announced — actual vs. expectations, price/technical reaction, and whether the holding thesis still stands. Use when the user asks about upcoming earnings/results, "who reports this week/quarter", or how a holding's results were. Never places any order.
---

# Earnings Watch

Read-only, forward-looking event tracking on names the user actually owns.
See `reference/READ-ONLY-POLICY.md` for the hard read-only rule and `reference/RESEARCH-STANDARDS.md`
for the disclosure block, news freshness rules, and data-efficiency rules.

Groww's MCP has no earnings-calendar tool — results dates and results
content come from `WebSearch` (NSE/BSE announcements, company IR pages,
business press), tied back to live Groww price/technical data.

## Steps

1. **Scope to what's owned.** `get_equity_portfolio_holdings` — the
   watch covers held stocks (plus any specific name the user adds).
   Don't build a market-wide earnings calendar.

2. **Upcoming results.** `WebSearch` (date-anchored, e.g. "Q2 FY26
   results date <company>") for the next results date per holding —
   batch this sensibly: one search for the portfolio's larger names
   ("<sector/index> results calendar <month year>") before per-name
   searches for the rest. Present as a dated calendar: who reports when,
   nearest first.

3. **For results already announced** (user asks "how were X's results",
   or a holding reported since the last check):
   - `WebSearch` the actual numbers — revenue/EBITDA/PAT vs. consensus or
     YoY, management commentary, guidance changes. Cite outlet + date.
   - Price reaction: `get_ltp` (batched) and, if the reaction matters to
     the verdict, `get_historical_technical_indicators` /
     `fetch_historical_candle_data` (daily) for the post-result move vs.
     prior trend — a good result sold into is a different signal than a
     good result bought.
   - **Thesis check**: does the result confirm or dent the reason this
     is held? Say which, concretely.

4. **Flag what needs action.** Only holdings where the result (or an
   imminent one plus stretched positioning) changes the picture get a
   view — hold/add/reduce per the Recommendation completeness checklist
   in `reference/RESEARCH-STANDARDS.md`. Routine in-line results are one line each.

5. **Present:** upcoming calendar first (date, name, weight in
   portfolio), then reviewed results (one line each; detail only for
   flagged names), then any views with the disclosure block. Point to
   `stock-research` for a full re-underwrite of any single name.
   Optional: Earnings Watch template in `reference/REPORT-TEMPLATES.md`, saved
   under `reports/` if the user wants it kept.
