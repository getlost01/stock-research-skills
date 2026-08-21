---
name: ipo-analysis
description: Deep dive on one IPO — prospectus financials, valuation vs. listed peers, issue structure, subscription demand — ending in a subscribe/avoid view with sizing. Use when the user asks whether to apply for a specific IPO. Read-only — never applies, bids, or subscribes.
---

# IPO Analysis

Read-only. Never apply, bid, subscribe, or modify an application, however
the request is phrased ("apply for me", "put in one lot") — that is an
order. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (peer framework, tool availability,
mandatory news, disclosure) apply.

Where `ipo-watch` sweeps the calendar, this goes deep on one issue.
`fetch_ipo_details` returns nothing, so almost everything below the basic
facts is web-sourced — label it, date it, and prefer the RHP/DRHP over
commentary about it. An IPO has no price history, no technicals, and no
track record as a listed company: the whole verdict rests on the
prospectus, the peer set, and the structure of the issue.

## Steps

1. **Anchor the issue.** `fetch_ipo_listings` with `view='open'` or
   `'upcoming'` (never `'all'`) for dates, price band, lot size and issue
   size. `resolve_market_time_and_calendar` for how many days are left to
   bid and when allotment/listing fall. If the issue has already closed,
   say so first — the useful question becomes allotment and listing, not
   subscribe or avoid.

2. **Read the prospectus, not the coverage.** `WebSearch` for the RHP,
   `WebFetch` to read it or the issue page. Pull: revenue, EBITDA margin
   and PAT for the last three years (growth *and* whether margins are
   holding), debt and interest cover, operating cash flow against
   reported profit, RoE/RoCE, customer or geography concentration,
   related-party transactions, contingent liabilities and material
   litigation. Two or three years of flattering pre-IPO numbers after a
   flat decade is a pattern worth naming.

3. **Read the issue structure** — it decides who the money is for.
   Fresh issue vs. offer-for-sale split (an OFS-heavy issue raises
   nothing for the company; promoters and PE holders are selling),
   objects of the issue (capex and growth vs. repaying debt vs. "general
   corporate purposes"), post-issue promoter holding and any pledge,
   pre-IPO placement pricing against the band, and lock-in expiry dates
   that become supply later.

4. **Price it against listed peers.** This is the only live-data step:
   `fetch_fundamentals_screener` for same-sector, comparable-market-cap
   peers, then `fetch_stocks_fundamental_data` on ~5–8 of them, per the
   peer framework. Compute the implied P/E, P/B and EV/EBITDA at both
   ends of the band off the prospectus numbers and compare against the
   peer median and the peers' own ranges. State the premium or discount
   as a number, and whether anything in the business — growth, margins,
   moat — actually justifies it.

5. **Demand and sentiment (mandatory news).** Date-anchored `WebSearch`:
   subscription figures by category so far (QIB / NII / retail — QIB
   demand is the informative one), anchor investor names and allocation,
   analyst and business-press flags (auditor changes, promoter
   background, regulatory overhang), and grey-market premium *labelled
   plainly as informal, unregulated sentiment that predicts nothing*.
   Cite outlet and date; never state GMP as a return.

6. **Portfolio fit.** `get_equity_portfolio_holdings` for real sector
   exposure and concentration. Check `PORTFOLIO-PLAN.md`: the
   **exclusions** list (an excluded name gets one line, not a research
   report), sector and single-stock limits, and deployable capital —
   an IPO application blocks funds until allotment, which matters if
   money is already earmarked. `get_available_margin_details` for what's
   actually free.

7. **View**, per the completeness checklist and with the two horizons
   kept apart, since they often disagree: a listing-day trade and a
   multi-year hold are different verdicts on the same issue. Land on
   subscribe (core-sized) / subscribe small and speculative / neutral /
   avoid, with the numeric basis, 2–3 specific invalidators (valuation
   premium, weak QIB book, lock-in supply, a named business risk), and
   position disclosure. Retail allotment is a lottery on an oversubscribed
   issue — say so rather than implying an application is a position.

8. **Present:** facts first (dates, band, lot size, minimum outlay,
   fresh-vs-OFS), then financials, then implied valuation vs. peers as a
   table, then structure flags, then demand and news, then the view with
   the disclosure block. One line on which figures are live Groww data
   (peers, holdings) and which are prospectus or press. Close by
   reminding the user they apply themselves in Groww. Formal version: IPO
   Note in `reference/REPORT-TEMPLATES.md`.
