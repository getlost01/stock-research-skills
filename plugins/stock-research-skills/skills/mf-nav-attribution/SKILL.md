---
name: mf-nav-attribution
description: Estimate today's mutual fund NAV move before the AMC publishes it — last-disclosed holdings weighted by each constituent's live % change. Use when the user asks how a fund is moving today or what its NAV will do. Read-only; an estimate, never the published NAV.
---

# MF NAV Attribution

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (disclosure, data efficiency) apply.

This produces an **estimate**. NAV is struck once daily after market
close off the AMC's own valuation — say that up top, every time. No
broker MCP carries a fund's holdings-with-weights either; INDmoney's
fund-details capability is the only broker source of fund facts, so
expect to fall back to the AMC factsheet via `WebSearch`/`WebFetch`.

## Steps

1. **Establish the fund and the clock.** Fund from the user; note "now"
   as the anchor for both live prices and holdings staleness.

2. **Pull the last disclosed portfolio** — monthly factsheet (AMC site,
   Value Research, Morningstar), date-anchored. Report the **disclosure
   date** prominently: factsheets run 15-45 days stale and weights drift
   after a large flow or an active manager's trade. Compute the coverage
   gap (`100% − Σ listed weights`) — unpriced long-tail equity plus
   cash/debt — and carry it explicitly rather than treating it as zero.

3. **Classify constituents**: listed Indian equity (live via LTP);
   international equity (out of live-pricing scope — take last %move
   from the web, labelled); commodities (no broker has spot — use the
   matching domestic ETF's LTP as proxy and say so); cash/debt sleeve
   (~0% intraday unless the fund is debt-heavy and rates moved, then
   direction only, no invented number).

4. **Batch-price the listed names** — resolve symbols once via the
   symbol-search capability, then one batched LTP/quote call, never one
   per holding. Per name: `(LTP − prev close) / prev close`.

5. **Weight and sum.** `contribution = weight% × %change` per name;
   sum, plus the commodity proxy, holding the coverage gap at 0 as a
   stated assumption. Where that gap exceeds ~15-20%, give a range —
   gap flat vs. gap moving with the broad market.

6. **Present** a table sorted by |contribution|: name, weight%,
   %change, contribution. Then the summed estimate, coverage %, and the
   disclosure date restated, closing with: estimate off a [date]
   portfolio, not the published NAV — check the AMC or AMFI after close.
   Disclosure block only if a view rides on top (e.g. "is this dip a
   buy"); a bare mechanical estimate carries no view.

7. **ETFs are the wrong target** — their live price already arbitrages
   in constituent moves, so quote the LTP instead of reconstructing it.
   Fund quality questions (fees, alpha, overlap) go to
   `mutual-fund-analysis`.
