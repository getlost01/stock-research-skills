---
name: fno-analysis
description: Analyze the user's F&O positions and candidate trades — greeks, open interest, payoff, straddle levels, margin. Use when the user asks about options/futures positions, greeks, open interest, payoff, straddle, or F&O margin. Read-only.
---

# F&O Position & Strategy Analysis

Read-only, F&O included — no order placement, modification, or
cancellation. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (disclosure) apply. Works over
whichever broker(s) `BROKERS.md` names active — resolve each capability
below against `reference/BROKER-CAPABILITIES.md`; Zerodha's positions
capability (`kite`) is the main non-Groww source here.

## Steps

1. **Current exposure.** Open intraday/F&O positions capability, across
   every active broker with F&O exposure. Resting-order status: Groww's
   is broken (`get_order_details`, see **Tool availability**); Kite's
   order/trade-history capability works if active — otherwise say status
   is unavailable and point the user at their broker's order book rather
   than inferring it.

2. **Contract/market data.** F&O contract search to resolve strike/expiry
   when the user names one loosely; Groww's curated liquid F&O list;
   quote + depth / LTP for live pricing; market calendar/timing
   capability for expiry/settlement.

3. **Risk/greeks.** Greeks-per-contract capability — delta, theta, gamma,
   vega per position and aggregated, up to 20 contracts per call on
   Groww, strikes resolved via contract search. Groww's per-symbol greeks
   tool is dead — don't reach for it; INDmoney's greeks-history
   capability (`get_indian_stocks_greeks_history`, if active) is a
   time-series alternative, not a like-for-like substitute. Open interest
   analysis and ATM straddle chart are Groww-only.

4. **Strategy shape.** Payoff chart capability (Groww-only) for an
   existing or hypothetical position — max profit/loss, breakevens, and
   behaviour under a hedge or adjustment the user is weighing.

5. **Capital.** F&O margin calculator (Groww-only) for the requirement;
   available-margin capability (Groww or Kite) for headroom against it.

6. **Event check (optional).** Worth a `WebSearch` for positions exposed
   to a known near-term event (RBI policy, budget, earnings before
   expiry); skip for routine reviews with nothing in the window.

7. **Present:** exposure summary first (net delta/theta, notional), then
   per-position detail, then any candidate strategy with its margin
   impact. Flag anything expiring soon or carrying outsized directional
   or theta risk relative to the account. Disclosure block when a view or
   strategy suggestion is given, not for a plain position lookup. Formal
   version: F&O Position Review in `reference/REPORT-TEMPLATES.md`.
