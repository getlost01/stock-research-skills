---
name: mutual-fund-analysis
description: Deep dive or peer comparison on mutual funds — expense ratio, alpha vs. benchmark and category, manager consistency, overlap with existing holdings. Use when the user asks about a specific fund, wants funds compared, or asks whether a fund is worth holding. Read-only — no SIP changes.
---

# Mutual Fund Analysis

Read-only, SIPs included — never start, stop, pause, or modify one.
`reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (peer framework, disclosure) apply.

## Steps

1. **Pull the fund(s).** `get_mutualfund_details` for the named fund(s)
   and for existing MF holdings, for the overlap and fit checks.

2. **Classify** — category (large-cap/flexi-cap/debt/hybrid/index/sector)
   and benchmark; everything below is relative to that.

3. **Peer comparison** per the MF/ETF peer framework: expense ratio,
   rolling returns vs. benchmark and category average, AUM trend,
   manager tenure and consistency. For index funds and ETFs, tracking
   error matters more than raw returns. `fetch_etf_screener` when the
   real question is the passive alternative — "is this active large-cap
   fund beating a Nifty 50 ETF after fees?" usually is.

4. **Overlap.** Compare top holdings and sector weights against the
   user's other funds and direct stocks
   (`get_equity_portfolio_holdings`). Three flexi-cap funds that are all
   60% the same fifteen stocks add cost, not diversification.

5. **News (optional).** A manager change, AMC-level issue, or
   category-wide SEBI circular is worth a quick `WebSearch` before a
   strong view; a routine "should I hold this SIP" usually isn't.

6. **View:** worth holding / switch to [alternative] / redeem — on
   fee-adjusted performance and overlap, never a raw-returns comparison.
   Any SIP change is the user's action in the Groww app.

7. **Present:** fund summary first (category, expense ratio, benchmark,
   AUM), then peer/benchmark comparison, then overlap flags, then the
   view with the disclosure block. Formal version: Fund Note in
   `reference/REPORT-TEMPLATES.md`.
