---
name: bond-ladder-planner
description: Plan and review the user's fixed-income book as a whole — maturity ladder across tenors, reinvestment risk, credit-quality mix, and gaps vs. the fixed-income targets in PORTFOLIO-PLAN.md. Use when the user asks about laddering bonds/FDs, what matures when, where to deploy fixed-income money next, or reviewing their overall debt allocation. Never places any order.
---

# Bond Ladder Planner

Read-only planning across the whole fixed-income book — where
`bond-analysis` judges one instrument, this plans the set. See root
`reference/READ-ONLY-POLICY.md` for the hard read-only rule and `reference/RESEARCH-STANDARDS.md` for
the bond framework, external-source rules, and disclosure block.

Groww's MCP exposes no direct-bond holdings — the direct bond/FD
inventory comes from the **Fixed-income inventory** table in
`PORTFOLIO-PLAN.md` (keep it current; ask the user to fill it if empty).
Debt mutual funds come live via `get_mutualfund_details`.

## Steps

1. **Assemble the book.**
   - Direct bonds/NCDs/FDs/SGBs: `PORTFOLIO-PLAN.md` inventory (issuer,
     amount, coupon, maturity, rating). If it's empty or stale, get the
     list from the user before planning — never invent an inventory.
   - Debt funds: `get_mutualfund_details` — note each fund's category,
     duration profile, and credit quality where available.
   - Total the fixed-income book and compare against the plan's
     fixed-income target %.

2. **Build the ladder view.** Bucket everything by maturity year (funds
   by duration bucket: liquid/short/medium/long). Show ₹ maturing per
   year and the weighted average yield locked per bucket.

3. **Current curve context.** `WebSearch` (per freshness rules) for the
   prevailing G-Sec yield curve and top-rated corporate/FD rates across
   the relevant tenors — the reference for every judgment below. Label
   as external data with source + date.

4. **Diagnose:**
   - **Gaps/lumps**: years with nothing maturing vs. years where too
     much matures at once (concentrated reinvestment risk).
   - **Reinvestment risk**: money maturing soon into a lower-rate
     environment — quantify roughly (₹ maturing × rate gap).
   - **Credit mix**: share below the plan's credit-quality floor;
     issuer/sector concentration per the bond framework.
   - **Duration fit**: overall duration vs. the user's horizon and the
     rate outlook (coordinate with `rate-watch` findings if recent).

5. **Suggest moves** — which tenor bucket new money should fill, what to
   do at each upcoming maturity, framed per the Recommendation
   completeness checklist (horizon is inherent; still give basis, key
   risks, position context). Specific instrument picks route through
   `bond-analysis` — this skill picks the *slot*, not the bond.

6. **Present:** ladder table first (year, ₹ maturing, instruments,
   locked yield), then diagnosis flags, then suggested moves, then the
   disclosure block plus the external-data freshness note. Optional:
   Bond Ladder template in `reference/REPORT-TEMPLATES.md`, saved under `reports/`
   if wanted.
