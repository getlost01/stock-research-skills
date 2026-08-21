---
name: new-investment-screener
description: Screen for new stock, ETF, or fund ideas by theme, criteria, or a gap in the portfolio. Use when the user asks to find new stocks/ETFs/funds, screen by criteria (value, growth, sector, dividend, momentum), or asks what to add. Read-only.
---

# New Investment Screener

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (peer framework, mandatory news on
finalists, disclosure) apply.

## Steps

1. **Clarify the brief** if it's vague: theme/sector, criteria
   (value/growth/quality/dividend/momentum), instrument type, and roughly
   how much they're deploying. Don't screen blind.

2. **Check what would actually help.** `get_equity_portfolio_holdings`
   plus `PORTFOLIO-PLAN.md`'s target allocation and fund list (the MCP
   returns no fund data) — is there a real underweight this screen
   should target, or is the user exploring?

3. **Screen.** `fetch_fundamentals_screener` for fundamental criteria;
   `curate_symbols` / `fetch_market_movers_and_trending_stocks_funds` for
   an open-ended starting universe. Technical setups (breakouts,
   momentum, oversold) can't be screened for — `fetch_technical_screener`
   is down — so screen fundamentally or by mover list first, then read
   the setups off `get_historical_technical_indicators` over the
   shortlist, 10 names per call. ETFs: `curate_symbols(entity_type='etf')`
   plus web-sourced fees and tracking error, since
   `fetch_etf_screener` errors (see **Tool availability**).

4. **Filter out bad fits.** Drop anything that would breach the plan's
   concentration limits, sits on its **exclusions** list, or that the
   **decision log** shows the user already declined. Size the shortlist
   to **deployable capital** and the preferred deployment style, not to a
   round number. Flag heavy overlap with existing holdings — the same
   exposure in a different wrapper.

5. **Rank the shortlist** (3–7 ideas) with reasoning per pick, not
   "screener said so". Pull `fetch_stocks_fundamental_data` /
   `get_historical_technical_indicators` for the finalists so the picks
   rest on real numbers rather than screener summary fields.

6. **News on the finalists (mandatory).** One `WebSearch` per
   shortlisted name for anything that would change the pick — results
   just out, a downgrade. Finalists only, not the screened universe.

7. **Present:** shortlist table (name, the metrics matching the brief,
   one-line rationale, material news), then a short "why not others" if
   useful, then the disclosure block. These are ideas for the user to
   evaluate and place themselves. Formal version: Screener Shortlist in
   `reference/REPORT-TEMPLATES.md` — worth it for several candidates, not
   for a two-name shortlist.
