---
name: bond-analysis
description: Analyze a bond, NCD, or debt fund — yield vs. G-Sec, credit rating, duration, issuer concentration, liquidity. Use when the user asks about a bond, NCD, debt fund, or fixed-income allocation. Read-only.
---

# Bond / Debt Instrument Analysis

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (bond framework, freshness, sources,
disclosure) apply.

Groww's MCP exposes no bond tools, so this leans on `WebSearch`/`WebFetch`
more than the other skills — and debt funds are web-sourced too, since
the MCP returns no fund data. Say explicitly which numbers came from
Groww live vs. the web; don't blur the two.

## Steps

1. **Debt mutual fund?** Use `mutual-fund-analysis` instead or first.
   This skill is for direct bonds/NCDs; use both for a mix.

2. **Identify the instrument.** Issuer, ISIN if known, coupon, maturity/
   tenor, and whether government (G-Sec/SDL), PSU, or corporate — if
   corporate, secured/unsecured and seniority.

3. **Pull external data** via `WebSearch`/`WebFetch`:
   - Current YTM and price (Groww's bonds page for the ISIN, or NSE/BSE
     debt segment).
   - Credit rating *and the agency's latest rationale* (CRISIL/ICRA/CARE)
     — outlook and recent action, not just the letter grade.
   - G-Sec yield at comparable tenor, to compute the spread offered.
   - Issuer financials/news if corporate — results, leverage, sector stress.

4. **Analyze:** spread vs. G-Sec (fair compensation for the credit and
   liquidity risk, or too thin?); duration and what a rate move does to
   price; issuer/sector concentration in the user's fixed-income book
   after this; liquidity — say plainly when the search finds no active
   trading data.

5. **View:** attractive / fair / avoid at the current yield, with the
   spread and credit reasoning shown, plus size guidance relative to the
   fixed-income allocation in `PORTFOLIO-PLAN.md`.

6. **Present:** instrument facts first, labeled by source, then
   yield/spread/duration, then the view with the disclosure block — and
   flag where external data is of unknown freshness. Formal version: Bond
   Note in `reference/REPORT-TEMPLATES.md`.
