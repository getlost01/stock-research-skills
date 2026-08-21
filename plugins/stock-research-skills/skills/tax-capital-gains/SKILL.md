---
name: tax-capital-gains
description: Estimate unrealized/realized capital gains (LTCG/STCG) on the user's holdings and surface tax-loss/gain harvesting opportunities, using live Groww data. Use when the user asks about capital gains, tax, LTCG, STCG, harvesting losses, or CA/tax-filing prep. Read-only — estimates only, not filing-grade or professional tax advice.
---

# Capital Gains & Tax-Harvesting (estimates)

Read-only, best-effort estimation. See `reference/READ-ONLY-POLICY.md` for the hard
read-only rule. **Always caveat this clearly**: these are estimates from
available data, not a substitute for the actual Capital Gains
Statement/contract notes from Groww or advice from a qualified CA — say
so explicitly in the output, especially before any number gets used for
actual filing.

## Steps

1. **Pull holdings with cost basis.** `get_equity_portfolio_holdings` (avg
   buy price, quantity) and `get_mutualfund_details` for funds. Use
   `get_order_details` where available to refine buy dates/lots if the
   user needs lot-level (not just average) detail.

2. **Mark to market.** `get_ltp` / `get_quotes_and_depth` for current
   prices to compute unrealized gain/loss per holding.

3. **Classify holding period where determinable:**
   - Equity/equity MF: >12 months → LTCG bucket, ≤12 months → STCG bucket.
   - Note explicitly when the exact purchase date isn't available from the
     data pulled (e.g. only an average cost is available across multiple
     lots) — don't silently assume a holding period.

4. **Surface opportunities:**
   - **Loss harvesting**: positions sitting at an unrealized loss that
     could offset gains elsewhere this FY (per `PORTFOLIO-PLAN.md`'s tax
     context section if filled in).
   - **Gain timing**: positions near the STCG→LTCG threshold where waiting
     a bit longer would meaningfully change the tax rate.
   - Always frame as "you could consider..." — never as an instruction to
     sell, and never actually place any order.

5. **Present:** a summary table (holding, qty, cost basis, current value,
   unrealized gain/loss, LTCG/STCG bucket where known), then the harvesting
   observations, then the caveat about verifying against Groww's official
   statement and consulting a CA before filing. Also close with the
   standard `reference/RESEARCH-STANDARDS.md` disclosure block — this is estimate
   territory twice over (data completeness *and* not being tax advice).
   Optional: use the Capital Gains Snapshot template in
   `reference/REPORT-TEMPLATES.md`, which most users of this skill will actually
   want since it's inherently a "save/reference later" kind of output —
   save it under `reports/` per that file's naming convention.
