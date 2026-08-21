---
name: bond-ladder-planner
description: Plan the user's whole fixed-income book — maturity ladder, reinvestment risk, credit mix, gaps vs. plan targets. Use when the user asks about laddering bonds/FDs, what matures when, where to deploy fixed-income money next, or reviewing their debt allocation. Read-only.
---

# Bond Ladder Planner

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (bond framework, external-source and
freshness rules, disclosure) apply.

Where `bond-analysis` judges one instrument, this plans the set. Groww's
MCP exposes no direct-bond holdings: the inventory, tenor preference,
credit floor, and reinvestment default all come from the **Fixed-income
inventory** section of `PORTFOLIO-PLAN.md`. Debt funds come from there
too — Groww's MCP returns no fund data (see **Tool availability** in
`reference/RESEARCH-STANDARDS.md`).

## Steps

1. **Assemble the book.** Direct bonds/NCDs/FDs/SGBs from the plan's
   inventory (issuer, amount, coupon, maturity, rating) — if empty or
   stale, offer `portfolio-plan-builder` or get the list from the user;
   never invent an inventory. Debt funds from the same section, noting
   category, duration profile and credit quality — fund facts
   web-sourced and labelled, since the MCP has none. Total the book
   against the plan's fixed-income target %.

2. **Ladder view.** Bucket by maturity year (funds by duration bucket:
   liquid/short/medium/long). Show ₹ maturing per year and the weighted
   average yield locked per bucket.

3. **Curve context.** `WebSearch` for the prevailing G-Sec curve and
   top-rated corporate/FD rates across the relevant tenors — the
   reference for every judgment below. Label as external, with date.

4. **Diagnose:** gaps and lumps (years with nothing maturing vs. years
   with too much at once); reinvestment risk, quantified roughly
   (₹ maturing × rate gap); credit mix below the plan's floor plus
   issuer/sector concentration; overall duration vs. horizon and rate
   outlook (coordinate with recent `rate-watch` findings).

5. **Suggest moves** — which tenor bucket new money fills, what to do at
   each upcoming maturity, per the completeness checklist. This skill
   picks the *slot*; specific instruments route to `bond-analysis`.

6. **Present:** ladder table (year, ₹ maturing, instruments, locked
   yield), then flags, then moves, then the disclosure block and the
   external-data freshness note. Formal version: Bond Ladder in
   `reference/REPORT-TEMPLATES.md`.
