# Broker Capabilities

Skills call **capabilities** ("equity holdings", "LTP", "F&O greeks"),
never a raw tool name directly. This file maps each capability to the
concrete MCP tool per broker. `BROKERS.md` (see `BROKERS.example.md`)
says which broker(s) are active and in what preference order; a skill
resolves a capability by checking active brokers in that order and
using the first one that has it.

**Prefer an MCP tool over the web.** Where a capability below covers
what's needed, call it — `WebSearch`/`WebFetch` is the fallback for
what no broker exposes (news, bonds, fund factsheets, US fundamentals),
never a substitute for a working tool. Label web-sourced figures as
such, per `RESEARCH-STANDARDS.md`.

Read `READ-ONLY-POLICY.md` first — **Write tools (never call)** below is
the enumerated version of that rule, per broker.

Only Groww's rows are live-verified. Every other broker's tool names
come from its own docs and are unconfirmed against a real account —
verification status, per-broker setup docs, researched-but-unwired
brokers, and the procedure for adding one all live in
`BROKER-RESEARCH.md`.

## Capability map

| Capability | Groww (`growwmcp`) | Zerodha (`kite`) | INDmoney (`indmoney`) | Upstox (`upstox`) | Notes |
|---|---|---|---|---|---|
| Equity holdings | `get_equity_portfolio_holdings` | `get_holdings` | `networth_holdings` (filter to equity) | `get-holdings` | |
| Mutual fund holdings | — (no fund data at all) | `get_mf_holdings` | `networth_holdings`, `mf_sips` | — | INDmoney is the only broker here with real MF holdings data |
| Net-worth / all-asset snapshot | — | — | `networth_snapshot`, `networth_allocation_breakdown` | — | INDmoney-only, broader than equities/F&O (gold, credit cards, loans, credit score) — pull only what's needed |
| Open F&O/intraday positions | `get_my_trading_positions_today`, `get_specific_stock_position` | `get_positions` | — | `get-positions`, `get-mtf-positions` | |
| Order status/history (read) | `get_order_details` — **dead** | `get_orders`, `get_trades`, `get_order_history`, `get_order_trades` | — | `get-order-book`, `get-order-details`, `get-order-history`, `get-order-trades`, `get-trades` | Kite is the only live-proven source; Upstox's is doc-verified only |
| GTT orders (read) | — | `get_gtts` | — | — | |
| LTP | `get_ltp` | `get_ltp` | `get_indian_stocks_details` | — | |
| Quote + depth | `get_quotes_and_depth` | `get_quotes` | `get_indian_stocks_details` | — | |
| OHLC / historical candles | `fetch_historical_candle_data` | `get_ohlc`, `get_historical_data` | `indian_stocks_ohlc` | — | |
| Symbol/instrument search | `curate_symbols` | `search_instruments` | `lookup_ind_keys` | — | Resolve identifiers with this before most other calls |
| Stock fundamentals (single name) | `fetch_stocks_fundamental_data` | — | `get_indian_stocks_details` (partial: analyst views, news) | — | |
| Fundamentals screener (peer set) | `fetch_fundamentals_screener` | — | — | — | Groww-only; no substitute for building a peer set live |
| Technical indicators | `get_historical_technical_indicators` | — | — | — | Groww-only |
| Candlestick patterns | `get_historical_candlestick_patterns` | — | — | — | Groww-only |
| Technical screener | `fetch_technical_screener` — **dead** | — | — | — | |
| ETF screener | `fetch_etf_screener` — **dead** | — | — | — | |
| Mutual fund details / category screen | `get_mutualfund_details` — **dead** | — | `get_mf_funds_details`, `get_mf_by_category` | — | INDmoney is the only working source of MF facts |
| MF SIP status | — | — | `mf_sips`, `indian_stocks_sips` | — | |
| Saved watchlist | — | — | `user_watchlist` | — | |
| Option chain | — | — | `get_indian_stocks_option_chain` | — | INDmoney-only |
| F&O contract search | `fno_mcx_contracts_search_tool` | `search_instruments` (F&O segment) | — | — | |
| F&O curated liquid list | `fetch_curated_fno` | — | — | — | Groww-only |
| Greeks — per contract | `get_greeks_for_fno_contract` | — | `get_indian_stocks_greeks_history` (time-series) | — | |
| Greeks — per symbol | `get_greeks_for_fno_symbol` — **dead** | — | — | — | |
| Open interest analysis | `get_open_interest_analysis` | — | — | — | Groww-only |
| ATM straddle chart | `get_atm_straddle_chart` | — | — | — | Groww-only |
| Payoff chart | `get_payoff_chart_steps` | — | — | — | Groww-only |
| Equity margin calculator | `calculate_equity_margin` | — | — | — | |
| F&O margin calculator | `calculate_fno_margin` | — | — | — | |
| Available margin | `get_available_margin_details` | `get_margins` | — | `get-funds-margin` | |
| Account profile | — | `get_profile` | — | `get-profile` | |
| Market movers / trending | `fetch_market_movers_and_trending_stocks_funds` | — | — | — | Groww-only |
| Market calendar / timing | `resolve_market_time_and_calendar` | — | — | — | Groww-only |
| Budget announcement | `get_budget_announcement` | — | — | — | Groww-only |
| IPO listings | `fetch_ipo_listings` | — | — | `get-ipos` | |
| IPO details | `fetch_ipo_details` — **dead** | — | — | `get-ipo-details` | |
| IPO application status (read) | — | — | — | `get-ipo-orders`, `get-ipo-order-details` | Upstox-only, found **2026-09-01**; no skill wires this yet |
| US stock data | — | — | `get_us_stocks_details` | — | Used by `us-stock-research`; no fundamentals screener/technical equivalent on any broker |
| Generic calculator | `calculator` | — | — | — | `log()` is **natural** log, see `RESEARCH-STANDARDS.md` |
| News | — | — | — | — | No news tool on any broker — always `WebSearch`/`WebFetch` |

A blank cell means that broker's MCP has no equivalent — not that it's
broken. When every active broker is blank for a capability the skill
needs, say so rather than silently dropping the section.

## Write tools (never call)

Backing the hard rule in `READ-ONLY-POLICY.md`. Also denied in
`.claude/settings.json` as a backstop — extend that list when adding a
broker.

| Broker | Write tools |
|---|---|
| Groww (`growwmcp`) | `place_order`, `modify_order`, `cancel_order`, `place_gtt_order`, `execute_order`, `create_order`, `place_mutualfund_order`, `start_sip`, `cancel_sip`, `modify_sip` |
| Zerodha (`kite`) | `place_order`, `modify_order`, `cancel_order`, `place_gtt_order`, `modify_gtt_order`, `delete_gtt_order` |
| INDmoney (`indmoney`) | None published — read-only by design. Recheck periodically |
| Upstox (`upstox`) | None in the official repo's tool list (all 14 read-only) — still treat any order-shaped tool name as denied by default until the hosted server's live tool-list is confirmed |
| Angel One | N/A — not wired. Enumerate a community server's write tools here *before* adding it to `.mcp.json` |

`kite`'s `login` tool isn't a write/order tool — it's the auth handshake
(generates a sign-in link). It's fine to call.
