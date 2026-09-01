---
name: tax-capital-gains
description: Estimate unrealized and realized LTCG/STCG on the user's holdings and surface tax-loss and gain-harvesting opportunities. Use when the user asks about capital gains, tax, LTCG, STCG, harvesting losses, or filing prep. Read-only — estimates, not tax advice.
---

# Capital Gains & Tax-Harvesting (estimates)

Read-only, best-effort estimation. `reference/READ-ONLY-POLICY.md` (hard
rule) and `reference/RESEARCH-STANDARDS.md` (disclosure) apply. Works
over whichever broker(s) `BROKERS.md` names active — resolve any
capability against `reference/BROKER-CAPABILITIES.md`.

**Always caveat clearly:** these are estimates from available data, not a
substitute for your broker's Capital Gains Statement or a qualified CA —
say so in the output, especially before any number could be used for
filing.

**State the FY and rate basis up front.** Name which financial year's
slabs/rates the estimate uses (LTCG/STCG rates and exemption thresholds
have changed across FYs — e.g. the FY24-25 Budget's equity LTCG/STCG
revision), with the as-of date, so a stale training-data rate never
silently substitutes for the current one. Where the acquisition date is
on or before the grandfathering cutoff (31 Jan 2018 for equity LTCG),
say whether grandfathered cost is being used or ignored — don't apply it
silently either way.

## Steps

1. **Holdings with cost basis.** Equity holdings capability, from every
   active broker (avg buy price, quantity). Fund units and cost basis
   come from `PORTFOLIO-PLAN.md`, unless INDmoney is active and covers
   them (see `BROKER-CAPABILITIES.md`). Lot-level detail is unavailable
   outright on Groww (`get_order_details` is broken) and unverified
   elsewhere, so gains are estimated off average cost: say so, and note
   that actual buy dates decide the LTCG/STCG split and any
   grandfathering the user must check in their broker's statement.

2. **Mark to market.** LTP / quote+depth capability for unrealized
   gain/loss per holding.

3. **Classify holding period** where determinable: equity and equity MF
   >12 months → LTCG, ≤12 months → STCG, against the FY stated above.
   Say explicitly when the exact purchase date isn't in the data pulled
   (e.g. only an average cost across lots) — never silently assume a
   holding period.

4. **Surface opportunities**, framed as "you could consider", never an
   instruction to sell:
   - **Loss harvesting** — positions at an unrealized loss that could
     offset gains this FY. Use the plan's tax context (realized STCG/LTCG
     so far, carried-forward losses, exemption used) and respect its
     **selling constraints** (lock-ins, LTCG-threshold holds,
     untouchables). Blank section → offer `portfolio-plan-builder` rather
     than netting against an assumed zero.
   - **Gain timing** — positions near the STCG→LTCG threshold where
     waiting would meaningfully change the rate.

5. **Present:** summary table (holding, qty, cost basis, current value,
   unrealized gain/loss, LTCG/STCG bucket where known), then harvesting
   observations, then the caveat about verifying against your broker's
   official statement and consulting a CA. Close with the disclosure block too —
   this is estimate territory twice over, on data completeness *and* on
   not being tax advice. Formal version:
   `reference/templates/tax-capital-gains.md`, which most users of this
   skill will want.
