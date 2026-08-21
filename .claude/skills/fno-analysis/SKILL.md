---
name: fno-analysis
description: Analyze the user's F&O (futures & options) positions and candidate trades — greeks, open interest, payoff diagrams, straddle levels, and margin requirements — using live Groww data. Use when the user asks about options/futures positions, greeks, open interest, payoff, straddle, or F&O margin. Never places, modifies, or cancels any order.
---

# F&O Position & Strategy Analysis

Read-only options/futures analysis. See root `CLAUDE.md` for the hard
read-only rule — this never places/modifies/cancels an order, including
for F&O. See `RESEARCH-STANDARDS.md` for the disclosure block.

## Steps

1. **Pull current exposure.**
   - `get_my_trading_positions_today` for all open intraday/F&O positions.
   - `get_specific_stock_position` for a named underlying.
   - `get_order_details` for status of any resting order the user asks
     about (read-only lookup).

2. **Contract/market data.**
   - `fno_mcx_contracts_search_tool` to resolve the right contract
     (strike/expiry) if the user names one loosely.
   - `fetch_curated_fno` for a curated view of liquid/relevant contracts.
   - `get_quotes_and_depth`, `get_ltp` for live pricing.
   - `resolve_market_time_and_calendar` for expiry/settlement timing.

3. **Risk/greeks.**
   - `get_greeks_for_fno_contract` / `get_greeks_for_fno_symbol` — delta,
     theta, gamma, vega exposure per position and aggregated.
   - `get_open_interest_analysis` — OI buildup/unwinding, PCR context.
   - `get_atm_straddle_chart` — implied move context.

4. **Strategy shape.**
   - `get_payoff_chart_steps` for the payoff profile of a position or
     candidate strategy (existing or hypothetical) — describe max
     profit/loss, breakevens, and how it behaves if the user is
     considering a hedge/adjustment.

5. **Capital.**
   - `calculate_fno_margin` for margin requirement of an existing or
     candidate position.
   - `get_available_margin_details` to check headroom against that
     requirement.

6. **Event/news check (optional).** For positions sensitive to a known
   near-term event (RBI policy, budget, earnings before expiry), a
   `WebSearch` check is worth it — skip it for routine position reviews
   with no event in the window.

7. **Present:** current exposure summary (net delta/theta, notional) first,
   then per-position detail, then any candidate-strategy analysis the user
   asked about, with margin impact noted. Flag anything expiring soon or
   carrying outsized directional/theta risk relative to the account. Close
   with the `RESEARCH-STANDARDS.md` disclosure block when a view/strategy
   suggestion is given (not needed for a plain position lookup). Optional:
   use the F&O Position Review template in `REPORT-TEMPLATES.md` for a
   formal/saveable version — save under `reports/` per its naming
   convention if the user wants it kept.
