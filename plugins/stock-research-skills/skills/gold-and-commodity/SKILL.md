---
name: gold-and-commodity
description: Review the gold/silver sleeve — SGB premium or discount to gold value and remaining tenor, gold/silver ETF tracking and liquidity, sleeve size vs. the plan's target. Use when the user asks about gold, silver, SGBs, gold ETFs, or a commodity allocation. Read-only.
---

# Gold & Commodity Sleeve

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (peer framework for passive vehicles,
data efficiency, completeness checklist, disclosure) apply. Steps name
capabilities — resolve each against `reference/BROKER-CAPABILITIES.md`.

No broker MCP exposes gold or silver **spot**, an ETF's iNAV, or SGB
issue terms, so the reference prices here are web-sourced — label and
date each one per the standards. Tax treatment of both SGBs and gold
funds has changed in recent Budgets: **verify it live rather than
quoting a rate from memory.**

## Steps

1. **Load the plan.** `PORTFOLIO-PLAN.md` — **Target allocation** (the
   ETF/gold bucket, and which bucket the user counts gold funds in), the
   **fixed-income inventory** (SGBs are usually recorded there, and no
   MCP can see them), **Rebalancing rules** (a "never touch" line on an
   SGB tranche held for maturity is decisive), and the **Income goal**,
   since the SGB coupon is real income.

2. **Pull live state.** Equity holdings from every active broker — gold
   and silver ETFs appear as ordinary equity lines; gold FoFs sit in the
   fund sleeve (**SIP register**, or INDmoney's fund holdings if active).
   Demat-held SGBs may or may not surface in the holdings payload: check,
   fall back to the plan inventory, and don't count a tranche twice.
   One batched LTP call for everything listed.

3. **Establish the reference price** — domestic gold (₹/10g, 24k) and
   silver, plus international spot, from the web, dated. Every
   premium/discount figure below is only as good as this number, so cite
   it once, prominently.

4. **Per SGB tranche**: issue price, remaining tenor and maturity date,
   next coupon date, and the 2.5% coupon — which is paid **on issue
   price**, so state yield-on-issue and yield-on-current-price
   separately; they diverge a lot after a gold rally. Then market price
   vs. the tranche's underlying gold value → **premium or discount %**.
   A discount with a short tenor left is the classic case for holding to
   maturity; a premium is why buying one on the exchange is often worse
   than it looks. Note that fresh issuance has been discontinued —
   secondary market only — and confirm that status via the web rather
   than asserting it.

5. **Per ETF / FoF**: expense ratio, tracking difference vs. the gold or
   silver price over 1Y, AUM and trend, and current premium/discount to
   iNAV. Then quote + depth on each — thin gold ETFs trade meaningfully
   away from iNAV intraday, and that spread can dwarf the TER gap
   between two candidates. Groww's ETF screener is dead (see **Tool
   availability**) and no other configured broker replaces it, so fees
   and tracking come from the AMC factsheet / AMFI / Value Research.
   For a like-for-like contest between same-index vehicles, that's
   `etf-tracking-quality`'s job — point there instead of duplicating it.

6. **Get the tax treatment current, from the web, dated** — SGB
   redemption at maturity vs. a secondary-market sale are treated
   differently, and the holding-period rules for gold ETFs and gold funds
   moved in the 2024 Budget. Hand the resulting numbers to
   `tax-capital-gains` rather than computing a liability here.

7. **Size the sleeve.** Sleeve % vs. the plan's target, with the honest
   framing: gold is ballast and a diversifier with no cash flow (bar the
   SGB coupon), so judge it on the role it plays in a drawdown, not on
   trailing returns — a sleeve that just outran everything is *more*
   likely to be over-target, not validated. Any view on the gold path
   itself gets both branches, never a direction stated as knowledge.

8. **Present.** One table: instrument, type, ₹ value, weight,
   premium/discount, tenor or maturity, coupon or TER. Then findings, then
   a view + horizon per instrument type (a maturing SGB and a costly ETF
   deserve different verdicts), the reference price and its date, and the
   disclosure block. Formal version:
   `reference/templates/gold-and-commodity.md`.

9. **Hand off.** `bond-ladder-planner` (SGB maturities are ladder rungs),
   `rate-watch` (real rates are the main driver), `rebalancing-planner`
   for sizing, `portfolio-stress-test` for the diversification claim
   under an actual shock.
