---
name: rebalancing-planner
description: Diff the user's actual allocation against their PORTFOLIO-PLAN.md targets and produce sized rebalancing suggestions. Use when the user asks to rebalance, check if they're over/under-weight, or align to a target. Read-only — suggestions only.
---

# Rebalancing Planner

Read-only; suggestions only, never orders. `reference/READ-ONLY-POLICY.md`
(hard rule) and `reference/RESEARCH-STANDARDS.md` (disclosure — this
skill always produces recommendations) apply.

## Steps

1. **Load the target.** Read `PORTFOLIO-PLAN.md` for the target
   allocation and bands, risk limits, and **rebalancing rules** (cadence,
   action threshold, correction order, selling constraints,
   untouchables). Those rules decide whether a drift is even actionable
   and how to close it, so read them before computing anything. Missing
   or stale section → say so and offer `portfolio-plan-builder`; never
   invent a target.

2. **Pull actual holdings.** `get_equity_portfolio_holdings`, and
   `get_my_trading_positions_today` / `get_specific_stock_position` if
   the plan has a derivatives bucket. The MF sleeve comes from
   `PORTFOLIO-PLAN.md` — the MCP returns no fund data, so if that section
   is missing or stale the fund side of any target-vs-actual diff is
   unknown; say so rather than treating the equity book as the whole
   portfolio.
   Mark everything to current market value via `get_ltp` /
   `get_quotes_and_depth` — allocation is by current value, not cost.

3. **Compute current allocation** by the plan's own buckets
   (large-cap / mid-small-cap / ETF / MF / F&O / cash) and by
   single-stock and sector weight, using
   `fetch_stocks_fundamental_data` / screener data for sector and
   market-cap classification where needed.

4. **Diff against target:** bucket current % vs. target %, with the ₹ the
   gap represents; single-stock and sector limits breached; any thesis
   whose invalidator has been hit (a broken thesis changes what to trim
   first); overlap between funds/ETFs, which counts against
   diversification even when buckets look fine.

5. **Suggest concrete moves**, sized in ₹ or % — "trim large-cap by ~₹X
   (N% over target)", "add ~₹Y to ETFs to close the gap" — for the user
   to execute themselves in the Groww app.

6. **Present:** target-vs-actual table first, then concentration and
   overlap flags, then the moves with brief reasoning each, then the
   disclosure block. Where a move is a specific instrument swap rather
   than a bucket adjustment, point to
   `stock-research`/`mutual-fund-analysis` first — a bucket gap doesn't
   dictate which name fills it. Formal version: Rebalancing Plan in
   `reference/REPORT-TEMPLATES.md`.
