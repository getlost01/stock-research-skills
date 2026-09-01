---
name: stock-research
description: Deep dive on one stock, ETF, or fund — fundamentals, technicals, peers, current news, and fit against the user's portfolio. Use when the user asks "should I buy <symbol>", wants research on a specific instrument, or a buy/hold/avoid view. Read-only.
---

# Single-Instrument Research

Read-only deep dive on one symbol. `reference/READ-ONLY-POLICY.md` (hard
rule) and `reference/RESEARCH-STANDARDS.md` (technical framework, peer
framework, news sourcing, completeness checklist, disclosure) apply —
follow those methods, don't freelance a different one.

This skill works over whichever broker(s) `BROKERS.md` names as active.
Steps below name **capabilities**, not tool names — look each one up in
`reference/BROKER-CAPABILITIES.md` for the concrete tool on your active
broker(s). Only Groww's rows are live-verified; treat any other
broker's tool as unconfirmed until it returns data.

## Steps

1. **Identify the instrument type** (stock / ETF / mutual fund / F&O
   contract) — the capabilities differ.

2. **Pull data.**
   - Price: LTP, quote + depth.
   - Stock fundamentals: single-name fundamentals, fundamentals screener
     — build a real peer set (same sector/market-cap band) and compare
     against it, not just against the stock's own history. Groww is the
     only broker here with a working fundamentals screener.
   - Technicals: technical indicators, candlestick patterns, historical
     candles — the full multi-timeframe / trend / momentum / volatility
     / volume framework over several horizons (1M/6M/1Y), not one
     indicator. Groww-only capability today — if it's not active, say
     technicals aren't available rather than eyeballing candles.
   - **News (mandatory):** `WebSearch` for results, management
     commentary, rating actions, corporate actions, regulatory issues.
     Date-anchored, last-30-days preference, 1–2 searches, cite outlet +
     date, and factor anything material into the view.
   - ETF: symbol search filtered to ETF + LTP, fees and tracking error
     from the web. Mutual fund: route to `mutual-fund-analysis` — check
     the capability map first, since INDmoney (if active) carries real
     fund details and Groww doesn't (see **Tool availability**).
   - F&O: contract search, greeks per contract, open interest analysis.

3. **Check fit against the user's portfolio.**
   - Equity holdings capability, plus the fund list in
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
   then the disclosure block. Formal version:
   `reference/templates/stock-research.md`.
