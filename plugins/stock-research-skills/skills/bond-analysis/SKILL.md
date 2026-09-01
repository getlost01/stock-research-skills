---
name: bond-analysis
description: Analyze a bond, NCD, or debt fund — yield vs. G-Sec, credit rating, duration, issuer concentration, liquidity. Use when the user asks about a bond, NCD, debt fund, or fixed-income allocation. Read-only.
---

# Bond / Debt Instrument Analysis

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (bond framework, freshness, sources,
disclosure) apply.

Works over whichever broker(s) `BROKERS.md` names active — resolve any
capability against `reference/BROKER-CAPABILITIES.md`. No broker
configured there exposes bond tools, so this leans on
`WebSearch`/`WebFetch` more than the other skills — and debt funds are
web-sourced too unless INDmoney is active (it has real MF detail; check
it is unverified). Say explicitly which numbers came
from a broker live vs. the web; don't blur the two.

## Steps

1. **Debt mutual fund?** Use `mutual-fund-analysis` instead or first.
   This skill is for direct bonds/NCDs; use both for a mix.

2. **Identify the instrument.** Issuer, ISIN if known, coupon, maturity/
   tenor, and whether government (G-Sec/SDL), PSU, or corporate — if
   corporate, secured/unsecured and seniority.

3. **Pull external data** via `WebSearch`/`WebFetch`:
   - Current YTM and price (your active broker's bonds page for the
     ISIN if it has one, or NSE/BSE debt segment).
   - Credit rating *and the agency's latest rationale* (CRISIL/ICRA/CARE)
     — outlook and recent action, not just the letter grade; state how
     recent the action is (days/months) — a stale rationale is a weaker
     input than the letter grade implies.
   - G-Sec yield at comparable tenor, to compute the spread offered.
   - Issuer financials/news if corporate — results, leverage, sector stress.

4. **Analyze:** spread vs. G-Sec, stated **in basis points** at matched
   tenor (not "roughly 1%") — that number is the fair-compensation
   judgment, so show it explicitly rather than leaving it implied;
   duration and what a rate move does to price; issuer/sector
   concentration in the user's fixed-income book after this; liquidity —
   say plainly when the search finds no active trading data.

5. **View:** attractive / fair / avoid at the current yield, with the
   spread and credit reasoning shown, plus size guidance relative to the
   fixed-income allocation in `PORTFOLIO-PLAN.md`.

6. **Present:** instrument facts first, labeled by source, then
   yield/spread/duration, then the view with the disclosure block — and
   flag where external data is of unknown freshness. Formal version:
   `reference/templates/bond-analysis.md`.
