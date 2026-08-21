---
name: rebalancing-planner
description: Compare the user's actual portfolio allocation against their target allocation/risk profile in PORTFOLIO-PLAN.md and produce specific rebalancing suggestions (what's over/under-weight and by how much). Use when the user asks to rebalance, check if they're over/under-weight somewhere, or align their portfolio to a target. Never places any order — suggestions only.
---

# Rebalancing Planner

Read-only rebalancing analysis. See `reference/READ-ONLY-POLICY.md` for the hard
read-only rule and `reference/RESEARCH-STANDARDS.md` for the disclosure block
(this skill produces recommendations, so it applies).

## Steps

1. **Load the target.** Read `PORTFOLIO-PLAN.md` at the repo root for the
   target allocation, risk profile, and concentration limits. If it's
   empty/missing key values, ask the user for what's needed (or offer to
   fill it in together) rather than inventing a target.

2. **Pull actual holdings.**
   - `get_equity_portfolio_holdings` for stocks.
   - `get_mutualfund_details` for MF holdings.
   - `get_my_trading_positions_today` / `get_specific_stock_position` for
     open F&O exposure if the plan includes a derivatives bucket.
   - `get_ltp` / `get_quotes_and_depth` to mark everything to current
     market value (not cost basis) — allocation must be by current value.

3. **Compute current allocation** by bucket (matching the buckets in
   `PORTFOLIO-PLAN.md`: large-cap/mid-small-cap/ETF/MF/F&O/cash), and by
   single-stock and sector weight. Use `fetch_stocks_fundamental_data` /
   screener data for sector/market-cap classification where needed.

4. **Diff against target:**
   - Bucket-level: current % vs. target %, and the ₹ amount that over/under
     -weight represents.
   - Concentration checks: flag any single stock or sector past the limits
     in `PORTFOLIO-PLAN.md`.
   - Overlap checks: multiple funds/ETFs with heavily overlapping top
     holdings count against diversification even if buckets look fine.

5. **Suggest concrete moves**, sized in ₹ or %, e.g. "trim large-cap by
   ~₹X (currently N% over target)", "add ~₹Y to ETFs to close the gap" —
   framed as suggestions for the user to execute themselves in the Groww
   app, never as orders you place.

6. **Present:** a target-vs-actual table first, then concentration/overlap
   flags, then the suggested moves with brief reasoning each. Close with
   the `reference/RESEARCH-STANDARDS.md` disclosure block. Optional: use the
   Rebalancing Plan template in `reference/REPORT-TEMPLATES.md` for a formal/
   saveable version — save under `reports/` per its naming convention if
   the user wants it kept. If a suggested move is a
   specific instrument swap (not just "trim/add to this bucket"), consider
   pointing to `stock-research`/`mutual-fund-analysis` for that name before
   the user acts on it — a bucket-level gap doesn't dictate which specific
   stock/fund fills it.
