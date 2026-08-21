---
name: mutual-fund-analysis
description: Deep dive or peer comparison on mutual funds — expense ratio, alpha vs. benchmark and category, manager consistency, overlap with existing holdings. Use when the user asks about a specific fund, wants funds compared, or asks whether a fund is worth holding. Read-only — no SIP changes.
---

# Mutual Fund Analysis

Read-only, SIPs included — never start, stop, pause, or modify one.
`reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (peer framework, tool availability,
disclosure) apply.

Groww's MCP returns no mutual fund data — `get_mutualfund_details` is
dead and `fetch_etf_screener` errors. Every fund figure here is
web-sourced or user-supplied; say which is which, and never present an
external number as a live Groww one.

## Steps

1. **Establish the fund(s).** The named fund comes from the user. The
   *held* set — for overlap and fit — comes from `PORTFOLIO-PLAN.md`
   (holdings, **SIP register**); if that's missing or stale, ask for the
   list and offer `portfolio-plan-builder`, never assume one. Direct
   equity still comes live from `get_equity_portfolio_holdings`, and
   exchange-traded funds have live prices via
   `curate_symbols(entity_type='etf')` + one batched `get_ltp`.

2. **Classify** — category (large-cap/flexi-cap/debt/hybrid/index/sector)
   and benchmark; everything below is relative to that.

3. **Peer comparison** per the MF/ETF peer framework: expense ratio,
   rolling returns vs. benchmark and category average, AUM trend,
   manager tenure and consistency. For index funds and ETFs, tracking
   error matters more than raw returns. All of it via date-anchored
   `WebSearch` on AMFI / Value Research / Morningstar / the AMC
   factsheet — cite source and date per figure. Where the passive
   alternative is the real question ("is this active large-cap fund
   beating a Nifty 50 ETF after fees?" — it usually is), price the ETF
   leg live off `curate_symbols` + `get_ltp` and take its expense ratio
   and tracking error from the web.

4. **Overlap.** Compare top holdings and sector weights (from the
   factsheets pulled in step 3) against the user's other funds and
   direct stocks (`get_equity_portfolio_holdings`, live). Three
   flexi-cap funds that are all 60% the same fifteen stocks add cost,
   not diversification.

5. **News (optional).** A manager change, AMC-level issue, or
   category-wide SEBI circular is worth a quick `WebSearch` before a
   strong view; a routine "should I hold this SIP" usually isn't.

6. **View:** worth holding / switch to [alternative] / redeem — on
   fee-adjusted performance and overlap, never a raw-returns comparison.
   Any SIP change is the user's action in the Groww app.

7. **Present:** fund summary first (category, expense ratio, benchmark,
   AUM), then peer/benchmark comparison, then overlap flags, then the
   view with the disclosure block. One line up top on where the fund
   data came from, since none of it is live Groww data. Formal version:
   Fund Note in `reference/REPORT-TEMPLATES.md`.
