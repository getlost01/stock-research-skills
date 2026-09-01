---
name: ipo-analysis
description: Deep dive on one IPO — prospectus financials, valuation vs. listed peers, issue structure, subscription demand — ending in a subscribe/avoid view with sizing. Use when the user asks whether to apply for a specific IPO. Read-only — never applies, bids, or subscribes.
---

# IPO Analysis

Read-only. Never apply, bid, subscribe, or modify an application, however
the request is phrased ("apply for me", "put in one lot") — that is an
order, on any broker. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (peer framework, tool availability,
mandatory news, disclosure) apply. Steps below name capabilities —
resolve each against `reference/BROKER-CAPABILITIES.md` for the
broker(s) in `BROKERS.md`. IPO listings, the fundamentals screener, and
market calendar/timing are Groww-only today; if Groww isn't active, say
those gaps plainly and lean on `WebSearch`/exchange filings instead of
dropping the section.

Where `ipo-watch` sweeps the calendar, this goes deep on one issue.
Groww's IPO-details capability (`fetch_ipo_details`) is dead, so almost
everything below the basic facts is web-sourced — label it, date it, and
prefer the RHP/DRHP over commentary about it. An IPO has no price
history, no technicals, and no track record as a listed company: the
whole verdict rests on the prospectus, the peer set, and the structure of
the issue.

## Steps

1. **Anchor the issue.** IPO-listings capability, `view='open'` or
   `'upcoming'` (never `'all'`) for dates, price band, lot size and issue
   size. Market calendar/timing capability for how many days are left to
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
   pre-IPO placement pricing against the band, and **anchor/promoter/
   pre-IPO lock-in expiry dates** — name the exact dates and the number
   of shares they release; a clean listing story with a large lock-in
   cliff 30/90/180 days out is a supply risk the band price doesn't
   price in.

4. **Price it against listed peers.** This is the only live-data step:
   fundamentals-screener capability for same-sector, comparable-market-cap
   peers, then single-name fundamentals on ~5–8 of them, per the peer
   framework. Compute the implied P/E, P/B and EV/EBITDA **at both the
   lower and upper band price** off the prospectus numbers and compare
   both against the peer median and the peers' own ranges — a two-column
   table (lower band / upper band), not one blended number. State the
   premium or discount at each end, and whether anything in the business
   — growth, margins, moat — actually justifies it.

5. **Demand and sentiment (mandatory news).** Date-anchored `WebSearch`:
   subscription figures by category so far (QIB / NII / retail — QIB
   demand is the informative one), anchor investor names and allocation,
   analyst and business-press flags (auditor changes, promoter
   background, regulatory overhang), and grey-market premium *labelled
   plainly as informal, unregulated sentiment that predicts nothing*.
   Cite outlet and date; never state GMP as a return.

6. **Portfolio fit.** Equity holdings capability (every active broker)
   for real sector exposure and concentration. Check `PORTFOLIO-PLAN.md`:
   the **exclusions** list (an excluded name gets one line, not a
   research report), sector and single-stock limits, and deployable
   capital — an IPO application blocks funds until allotment, which
   matters if money is already earmarked. Available-margin capability
   (Groww or Kite) for what's actually free.

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
   two-column (lower/upper band) table, then lock-in expiry dates, then
   structure flags, then demand and news, then the view with the
   disclosure block. One line on which figures are live broker data
   (peers, holdings) and which are prospectus or press. Close by
   reminding the user they apply themselves, in their broker's app.
   Formal version: IPO Note in `reference/REPORT-TEMPLATES.md`.
