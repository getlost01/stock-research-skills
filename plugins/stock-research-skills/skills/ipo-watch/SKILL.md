---
name: ipo-watch
description: Review upcoming or ongoing IPOs against listed peers and give a subscribe/avoid view. Use when the user asks about IPOs, an upcoming listing, or whether to apply for one. Read-only — never applies or subscribes.
---

# IPO Watch

Read-only — never applies or subscribes on the user's behalf.
`reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (peer framework, news sourcing,
disclosure) apply.

## Steps

1. **List what's live/upcoming.** `fetch_ipo_listings` for the calendar,
   or the specific issue named.

2. **Detail.** `fetch_ipo_details` — price band, lot size, dates, issue
   size, and available business/financial detail.

3. **Benchmark against listed peers.** `fetch_fundamentals_screener` /
   `fetch_stocks_fundamental_data` on comparable listed companies, per
   the peer framework, to sanity-check the valuation being asked.

4. **News (mandatory).** `WebSearch` for recent coverage — subscription
   status and GMP chatter (label clearly as informal sentiment, not
   fact), anchor investors, and red flags raised by the business press
   ahead of listing (litigation, auditor issues, promoter background).

5. **Portfolio fit.** `get_equity_portfolio_holdings` — real
   diversification, or already overweight this sector? Cross-check
   `PORTFOLIO-PLAN.md`: sector and single-stock limits, the
   **exclusions** list (an excluded IPO gets one line, not research), and
   **deployable capital** for sizing.

6. **View:** subscribe (with sizing rationale — "small, speculative"
   vs. "core-sized") / neutral / avoid, showing the valuation-vs-peers,
   news, and fit reasoning. Note listing-day volatility risk where
   relevant.

7. **Present:** key facts up top (dates, band, lot size), then peer
   valuation, then news, then the view with the disclosure block — and
   remind the user they apply themselves via Groww. Formal version: IPO
   Note in `reference/REPORT-TEMPLATES.md`.
