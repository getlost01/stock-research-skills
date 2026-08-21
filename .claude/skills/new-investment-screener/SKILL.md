---
name: new-investment-screener
description: Screen for new stock, ETF, or mutual fund ideas matching a theme, criteria, or gap in the user's portfolio, using live Groww fundamentals/technical screeners. Use when the user asks to find new stocks/ETFs/funds to invest in, screen by criteria (value, growth, sector, dividend, etc.), or asks "what should I add to my portfolio". Never places any order.
---

# New Investment Screener

Read-only idea generation. See root `CLAUDE.md` for the hard read-only
rule and `RESEARCH-STANDARDS.md` for the peer-comparison framework and
disclosure block.

## Steps

1. **Clarify the brief** if not already clear: theme/sector, criteria
   (value/growth/quality/dividend/momentum), instrument type (stock/ETF/MF),
   and roughly how much they're looking to deploy — don't screen blind if
   the ask is vague.

2. **Check what would actually help.** Pull `get_equity_portfolio_holdings`
   / `get_mutualfund_details` and, if it exists, `PORTFOLIO-PLAN.md`'s
   target allocation — is there a real gap (underweight bucket/sector) this
   screen should target, or is the user just exploring?

3. **Screen.**
   - `fetch_fundamentals_screener` for stock ideas by fundamental criteria.
   - `fetch_technical_screener` for technical setups (breakouts, momentum,
     oversold, etc.).
   - `fetch_etf_screener` for ETF ideas (expense ratio, tracking, AUM).
   - `curate_symbols` / `fetch_market_movers_and_trending_stocks_funds` for
     a broader curated/trending starting universe if the brief is open-ended.

4. **Filter out bad fits:**
   - Drop anything that would breach the concentration limits in
     `PORTFOLIO-PLAN.md`.
   - Flag heavy overlap with existing holdings (same underlying exposure
     via a different wrapper).

5. **Rank shortlist** (typically 3-7 ideas) with the reasoning per pick —
   why it fits the brief and the portfolio, not just "screener said so."
   Pull `fetch_stocks_fundamental_data` / `get_historical_technical_indicators`
   for the finalists to back up the pick with real numbers, not just the
   screener's summary fields.

6. **News on the finalists (mandatory, per RESEARCH-STANDARDS.md)** — a
   `WebSearch` check per shortlisted name for anything recent that would
   change the pick (bad results just out, a rating downgrade, etc.). Skip
   this for the broader screened universe, only the finalists.

7. **Present:** shortlist table (name, key metric(s) matching the brief,
   one-line rationale, any material news found), then a short "why not
   others" note if useful, then remind the user these are ideas to
   evaluate/place themselves. Close with the `RESEARCH-STANDARDS.md`
   disclosure block. Optional: use the Screener Shortlist template in
   `REPORT-TEMPLATES.md` — worth it once there are several candidates,
   a plain answer is fine for a 1-2 name shortlist. Save under `reports/`
   per its naming convention if the user wants it kept.
