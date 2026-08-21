---
name: bond-analysis
description: Analyze a bond/NCD/debt instrument or debt mutual fund — yield vs. G-Sec benchmark, credit rating, duration/interest-rate risk, issuer concentration, liquidity — using web research since Groww's MCP has no bond-specific data. Use when the user asks about a bond, NCD, debt fund, or fixed-income allocation. Never places any order.
---

# Bond / Debt Instrument Analysis

Read-only, and mostly **external research** — Groww's MCP exposes no
bond-specific tools, so this leans on `WebSearch`/`WebFetch` more than the
other skills. See root `CLAUDE.md` for the hard read-only rule and
`RESEARCH-STANDARDS.md` for the bond peer-framework and disclosure block
and preferred sources (CRISIL/ICRA/CARE for ratings, RBI for G-Sec/policy
context).

Say explicitly in the output which numbers came from Groww (if any, e.g.
a debt mutual fund via `get_mutualfund_details`) vs. external web sources
— don't blur the two.

## Steps

1. **If it's a debt mutual fund**, use `mutual-fund-analysis`'s approach
   instead/first (`get_mutualfund_details`) — this skill is for direct
   bonds/NCDs, or use both together if the user holds a mix.

2. **Identify the instrument.** Issuer, ISIN if known, coupon, maturity/
   tenor, whether it's government (G-Sec/SDL), PSU, or corporate
   (and if corporate, secured/unsecured, seniority).

3. **Pull external data via WebSearch/WebFetch:**
   - Current YTM and price (Groww's own bonds platform page for the ISIN
     if listed there, or NSE/BSE debt segment).
   - Credit rating and, importantly, the rating agency's latest rationale
     (CRISIL/ICRA/CARE) — look for outlook (stable/negative) and any
     recent rating action, not just the letter grade.
   - Prevailing G-Sec yield of comparable tenor (RBI/financial press) to
     compute the credit spread being offered.
   - Issuer financials/news if corporate — recent results, leverage,
     sector stress.

4. **Analyze** (see `RESEARCH-STANDARDS.md` → Bonds):
   - Spread over G-Sec of similar tenor — is the extra yield reasonable
     compensation for the credit/liquidity risk, or too thin/too rich?
   - Duration — how much the price would move for a given rate change;
     flag if this is a long-duration instrument in a rate-uncertain
     environment.
   - Issuer concentration — how much of the user's fixed-income allocation
     would sit with one issuer/sector after this.
   - Liquidity — thinly traded corporate bonds can be hard to exit before
     maturity; say so plainly if the search doesn't turn up active
     trading data.

5. **Form a view**: attractive / fair / avoid at the current
   yield/price, with the spread and credit reasoning shown — and size
   guidance relative to the user's overall fixed-income allocation
   (`PORTFOLIO-PLAN.md` if filled in), never an instruction to buy.

6. **Present:** instrument facts (issuer, coupon, maturity, rating) first
   — labeled by source (Groww vs. web) — then the yield/spread/duration
   analysis, then the view. Close with the `RESEARCH-STANDARDS.md`
   disclosure block, and flag explicitly that this relied on external
   data of unknown freshness where that's the case. Optional: use the
   Bond Note template in `REPORT-TEMPLATES.md` for a formal/saveable
   version — save under `reports/` per its naming convention if the user
   wants it kept.
