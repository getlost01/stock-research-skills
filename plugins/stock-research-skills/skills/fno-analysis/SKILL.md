---
name: fno-analysis
description: Analyze the user's F&O positions and candidate trades — greeks, open interest, payoff, straddle levels, margin. Use when the user asks about options/futures positions, greeks, open interest, payoff, straddle, or F&O margin. Read-only.
---

# F&O Position & Strategy Analysis

Read-only, F&O included — no order placement, modification, or
cancellation. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (disclosure) apply.

## Steps

1. **Current exposure.** `get_my_trading_positions_today` for open
   intraday/F&O positions; `get_specific_stock_position` for a named
   underlying; `get_order_details` to look up the status of a resting
   order (read-only).

2. **Contract/market data.** `fno_mcx_contracts_search_tool` to resolve
   strike/expiry when the user names one loosely; `fetch_curated_fno` for
   a curated liquid set; `get_quotes_and_depth` / `get_ltp` for live
   pricing; `resolve_market_time_and_calendar` for expiry/settlement.

3. **Risk/greeks.** `get_greeks_for_fno_contract` /
   `get_greeks_for_fno_symbol` — delta, theta, gamma, vega per position
   and aggregated. `get_open_interest_analysis` for OI buildup/unwinding
   and PCR. `get_atm_straddle_chart` for implied-move context.

4. **Strategy shape.** `get_payoff_chart_steps` for an existing or
   hypothetical position — max profit/loss, breakevens, and behaviour
   under a hedge or adjustment the user is weighing.

5. **Capital.** `calculate_fno_margin` for the requirement,
   `get_available_margin_details` for headroom against it.

6. **Event check (optional).** Worth a `WebSearch` for positions exposed
   to a known near-term event (RBI policy, budget, earnings before
   expiry); skip for routine reviews with nothing in the window.

7. **Present:** exposure summary first (net delta/theta, notional), then
   per-position detail, then any candidate strategy with its margin
   impact. Flag anything expiring soon or carrying outsized directional
   or theta risk relative to the account. Disclosure block when a view or
   strategy suggestion is given, not for a plain position lookup. Formal
   version: F&O Position Review in `reference/REPORT-TEMPLATES.md`.
