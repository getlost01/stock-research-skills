---
name: stock-research
description: Deep dive on one stock, ETF, or fund — fundamentals, technicals, peers, current news, and fit against the user's portfolio. Use when the user asks "should I buy <symbol>", wants research on a specific instrument, or a buy/hold/avoid view. Read-only.
---

# Single-Instrument Research

Read-only deep dive on one symbol. `reference/READ-ONLY-POLICY.md` (hard
rule) and `reference/RESEARCH-STANDARDS.md` (technical framework, peer
framework, news sourcing, completeness checklist, disclosure) apply —
follow those methods, don't freelance a different one.

## Steps

1. **Identify the instrument type** (stock / ETF / mutual fund / F&O
   contract) — the tools differ.

2. **Pull data.**
   - Price: `get_ltp`, `get_quotes_and_depth`.
   - Stock fundamentals: `fetch_stocks_fundamental_data`,
     `fetch_fundamentals_screener` — build a real peer set (same
     sector/market-cap band) and compare against it, not just against
     the stock's own history.
   - Technicals: `get_historical_technical_indicators`,
     `get_historical_candlestick_patterns`,
     `fetch_historical_candle_data` — the full multi-timeframe / trend /
     momentum / volatility / volume framework over several horizons
     (1M/6M/1Y), not one indicator.
   - **News (mandatory):** `WebSearch` for results, management
     commentary, rating actions, corporate actions, regulatory issues.
     Date-anchored, last-30-days preference, 1–2 searches, cite outlet +
     date, and factor anything material into the view.
   - ETF: `curate_symbols(entity_type='etf')` + `get_ltp`, fees and
     tracking error from the web. Mutual fund: route to
     `mutual-fund-analysis` — the MCP has no fund data and no working ETF
     screener (see **Tool availability**).
   - F&O: `fno_mcx_contracts_search_tool`,
     `get_greeks_for_fno_contract`, `get_open_interest_analysis`.

3. **Check fit against the user's portfolio.**
   - `get_equity_portfolio_holdings`, plus the fund list in
     `PORTFOLIO-PLAN.md` — does
     the user already hold this, or something highly overlapping (another
     fund with the same top holdings)?
   - Cross-check `PORTFOLIO-PLAN.md`: concentration limits (would this
     breach a single-stock or sector cap?), the **exclusions** list, the
     **decision log** (already ruled out — and has anything changed?),
     and **deployable capital** for step 4's sizing.
   - On a buy/accumulate verdict, offer to record the thesis and its
     invalidator via `portfolio-plan-builder` — that's what lets
     `earnings-watch` and `portfolio-review` test this view later rather
     than just watching the price.

4. **Form a view, reasoning shown:** valuation (vs. peers and own
   history), quality/growth trend, technical posture, portfolio fit, then
   net buy / accumulate / hold / reduce / avoid and roughly what size
   makes sense given the portfolio. It must satisfy the completeness
   checklist — horizon, numeric basis, 2–3 specific invalidators,
   position disclosure, data as-of.

5. **Present:** one-paragraph verdict up top, then supporting detail
   (fundamentals table, technical read, peer comparison, news, fit note),
   then the disclosure block. Formal version: Research Note in
   `reference/REPORT-TEMPLATES.md`.
