---
name: stock-research
description: Deep-dive research on a single stock, ETF, or mutual fund using live Groww data plus current news — fundamentals, structured technical analysis, peer comparison, and how it would fit the user's existing portfolio. Use when the user asks "should I buy/what do you think about <symbol>", asks for research/analysis on a specific stock/ETF/fund, or wants a buy/hold/avoid view on one instrument. Never places any order.
---

# Single-Instrument Research

Read-only deep dive on one symbol. See root `CLAUDE.md` for the hard
read-only rule, and `RESEARCH-STANDARDS.md` for the technical-analysis
framework, peer-comparison framework, news sourcing rules, and the
disclosure block — apply all three, don't freelance a different method.

## Steps

1. **Identify the instrument type** (stock / ETF / mutual fund / F&O
   contract) — the right tools differ.

2. **Pull data.**
   - Price/quote: `get_ltp`, `get_quotes_and_depth`.
   - Stock fundamentals: `fetch_stocks_fundamental_data`,
     `fetch_fundamentals_screener` — build a real peer set (same
     sector/market-cap band) per `RESEARCH-STANDARDS.md` and compare
     valuation/quality metrics against it, not just against the stock's
     own history.
   - Technicals: `fetch_technical_screener`,
     `get_historical_technical_indicators`,
     `get_historical_candlestick_patterns`, `fetch_historical_candle_data`
     (trend, drawdowns, volatility over multiple horizons — e.g. 1M/6M/1Y).
     Run the full multi-timeframe/trend/momentum/volatility/volume
     framework from `RESEARCH-STANDARDS.md`, not just one indicator.
   - **News (mandatory):** `WebSearch` for recent news on this name —
     results, management commentary, rating actions, corporate actions,
     regulatory issues. Follow the freshness rules in
     `RESEARCH-STANDARDS.md`: date-anchored queries, last-30-days
     preference, 1-2 targeted searches, cite outlet + date — and factor
     anything material into the view.
   - ETF: `fetch_etf_screener` (expense ratio, tracking, AUM, category peers).
   - Mutual fund: `get_mutualfund_details`.
   - F&O contract: `fno_mcx_contracts_search_tool`,
     `get_greeks_for_fno_contract` / `get_greeks_for_fno_symbol`,
     `get_open_interest_analysis`.

3. **Check fit against the user's existing portfolio.**
   - `get_equity_portfolio_holdings` / `get_mutualfund_details` — does the
     user already hold this or something highly correlated/overlapping
     (e.g. another fund with the same top holdings)?
   - Cross-check against `PORTFOLIO-PLAN.md` concentration limits
     (single-stock / sector caps) if it exists — would adding/adding-to this
     breach them?

4. **Form a view, with reasoning shown:**
   - Valuation (cheap/fair/expensive vs. peers and its own history)
   - Quality/growth trend (fundamentals improving or deteriorating)
   - Technical posture (trend, momentum, near support/resistance)
   - Portfolio fit (diversification value vs. concentration risk)
   - Net: buy / accumulate / hold / reduce / avoid, and at roughly what size
     would make sense given the portfolio (as a suggestion, never an order).
   - The view must satisfy the Recommendation completeness checklist in
     `RESEARCH-STANDARDS.md` — horizon, numeric basis, 2-3 specific key
     risks/invalidators, position disclosure, data as-of.

5. **Present:** one-paragraph verdict up top, then the supporting detail
   (fundamentals table, technical read, peer comparison, news findings,
   fit note). State the data's as-of time, and close with the
   `RESEARCH-STANDARDS.md` disclosure block. Optional: use the Research
   Note template in `REPORT-TEMPLATES.md` if the user wants a formal/
   saveable note (save under `reports/` per its naming convention) — a
   normal conversational answer is fine otherwise.
