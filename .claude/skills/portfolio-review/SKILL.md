---
name: portfolio-review
description: Review the user's real Groww portfolio (stocks, ETFs, mutual funds, F&O) using live data via the growwmcp MCP server, then give allocation/risk analysis, buy/hold/avoid recommendations, and rebalancing suggestions. Use when the user asks to review their portfolio, get stock/ETF/MF suggestions, check allocation or diversification, or rebalance. Never places, modifies, or cancels any order.
---

# Portfolio Review & Recommendations

Read-only research/recommendation workflow over the user's live Groww
account. See `CLAUDE.md` at the repo root for the hard read-only rule —
this skill never calls any order-placing/modifying/cancelling tool. See
`RESEARCH-STANDARDS.md` for the technical/peer frameworks (used when a
holding gets flagged for deeper review) and the disclosure block.

## Steps

1. **Pull current state first, always.**
   - `get_equity_portfolio_holdings` for stock holdings.
   - `get_my_trading_positions_today` / `get_specific_stock_position` for
     open F&O or intraday positions if relevant to the ask.
   - `get_mutualfund_details` for MF holdings if the user has/asks about MFs.
   - Never estimate or assume holdings — always fetch them.

2. **Enrich with live market data** — following the Data efficiency rules
   in `RESEARCH-STANDARDS.md`: batch all held symbols into **one** `get_ltp`
   call (holdings data often already carries LTP — check before pulling at
   all), and pull per-name depth only for holdings the analysis actually
   flags, not every line item.
   - `get_ltp` (batched) for current prices; `get_quotes_and_depth` only
     where depth/spread matters.
   - `fetch_stocks_fundamental_data` / `fetch_fundamentals_screener` for
     valuation, growth, quality metrics — flagged/major holdings first.
   - `fetch_technical_screener` / `get_historical_technical_indicators` /
     `get_historical_candlestick_patterns` for technical context on
     flagged names.
   - `fetch_etf_screener` for ETF alternatives/comparisons.
   - `fetch_market_movers_and_trending_stocks_funds` for market context.
   - `fetch_historical_candle_data` for trend/drawdown checks over time
     (interval matched to horizon — see `RESEARCH-STANDARDS.md`).
   - `resolve_market_time_and_calendar` if timing (market open, expiry,
     settlement) matters to the answer.

3. **Analyze.**
   - **Allocation**: sector/market-cap/asset-class concentration, single-stock
     concentration risk, overlap between holdings (e.g. two funds/ETFs
     holding the same top names).
   - **Performance**: current P&L per holding and portfolio-wide, vs. a
     relevant benchmark (Nifty 50/Nifty 500 etc.) where data allows.
   - **Quality/valuation**: flag holdings that look expensive, deteriorating
     fundamentally, or technically broken — and ones that look attractively
     valued.
   - **Rebalancing**: compare current allocation to a sensible target (ask
     the user for their target/risk profile if not already known — don't
     assume one), and state concretely what's over/under-weight and by how
     much.

4. **News — optional, only for flagged holdings.** If a holding stands out
   (large weight, big move, or a fundamental/technical red flag), a
   `WebSearch` check for recent news on it is worth doing before the
   verdict. Don't news-check every line item in a large portfolio by
   default — that's what `stock-research` is for on a specific name.

5. **Recommend, don't execute.**
   - Give a clear buy/hold/reduce/avoid view per holding or candidate, with
     the *reasoning* (valuation, momentum, fundamentals, allocation fit) —
     not just a verdict. Each view meets the Recommendation completeness
     checklist in `RESEARCH-STANDARDS.md` (horizon, basis, key risks,
     position context, as-of).
   - For rebalancing, state suggested changes as "consider trimming X by
     ~N%, adding to Y" — the user places any trade themselves in the Groww
     app.
   - If asked something that implies placing a trade, give the analysis and
     say explicitly that you're not executing it.

6. **Present clearly.**
   - Lead with a short summary (portfolio value, overall skew, top 2-3
     findings) before the detail.
   - Use tables for holdings/allocation breakdowns when there are more than
     a few line items.
   - Always note the data is as of the time it was pulled (markets move).
   - Close with the `RESEARCH-STANDARDS.md` disclosure block since this
     includes recommendations.
   - Optional: if the user wants a formal/exportable report (or the
     holdings table is long enough that structure helps), use the
     Portfolio Review template in `REPORT-TEMPLATES.md` — otherwise a
     normal conversational answer is fine. Save it under `reports/` per
     that file's naming convention if the user wants it saved, not just
     shown.
   - Point to the dedicated skill for depth: `stock-research` for one name,
     `mutual-fund-analysis`/`bond-analysis` for those instrument types,
     `rebalancing-planner` for allocation moves, `fno-analysis` for
     derivatives, `tax-capital-gains` for gains/harvesting.
