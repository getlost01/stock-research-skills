---
name: mutual-fund-analysis
description: Deep dive or peer comparison on mutual funds — expense ratio, alpha vs. benchmark and category, manager consistency, overlap with existing holdings. Use when the user asks about a specific fund, wants funds compared, or asks whether a fund is worth holding. Read-only — no SIP changes.
---

# Mutual Fund Analysis

Read-only, SIPs included — never start, stop, pause, or modify one.
`reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (peer framework, tool availability,
disclosure) apply. Works over whichever broker(s) `BROKERS.md` names
active; resolve each capability below against
`reference/BROKER-CAPABILITIES.md`.

Groww's MCP returns no mutual fund data — `get_mutualfund_details` is
dead and `fetch_etf_screener` errors. If INDmoney is active, it's the
one broker here with real fund data (details, category screens, SIP
status — `get_mf_funds_details`, `get_mf_by_category`, `mf_sips`; still
**unverified**, check the capability map's status table before trusting
it live). Otherwise every fund figure here is web-sourced or
user-supplied; say which is which, and never present an external number
as a live broker one.

## Steps

1. **Establish the fund(s).** The named fund comes from the user. The
   *held* set — for overlap and fit — comes from `PORTFOLIO-PLAN.md`
   (holdings, **SIP register**), or from INDmoney's holdings/SIP
   capability if active; if neither, ask for the list and offer
   `portfolio-plan-builder`, never assume one. Direct equity still comes
   live from the equity holdings capability, and exchange-traded funds
   have live prices via ETF symbol search + one batched LTP call.

2. **Classify** — category (large-cap/flexi-cap/debt/hybrid/index/sector)
   and benchmark; everything below is relative to that.

3. **Peer comparison** per the MF/ETF peer framework: expense ratio,
   **rolling** returns (not point-to-point) vs. benchmark and category
   median, AUM trend, manager tenure and consistency. For an active
   fund, report alpha net of expense ratio — a fund beating its
   benchmark by less than it charges isn't earning its fee even with
   "positive" alpha. For index funds and ETFs, tracking error matters
   more than raw returns. Prefer INDmoney's fund-details/category-screen
   capability where active; otherwise date-anchored `WebSearch` on AMFI
   / Value Research / Morningstar / the AMC factsheet — cite source and
   date per figure either way. Where the passive alternative is the real
   question ("is this active large-cap fund beating a Nifty 50 ETF after
   fees?" — it usually is), price the ETF leg live off symbol search +
   LTP and take its expense ratio and tracking error from INDmoney or
   the web.

4. **Overlap.** Compare top holdings and sector weights (from the
   factsheets/screens pulled in step 3) against the user's other funds
   and direct stocks (equity holdings capability, live). Three
   flexi-cap funds that are all 60% the same fifteen stocks add cost,
   not diversification.

5. **News (optional).** A manager change, AMC-level issue, or
   category-wide SEBI circular is worth a quick `WebSearch` before a
   strong view; a routine "should I hold this SIP" usually isn't.

6. **View:** worth holding / switch to [alternative] / redeem — on
   fee-adjusted performance and overlap, never a raw-returns comparison.
   Any SIP change is the user's action in their broker's app.

7. **Present:** fund summary first (category, expense ratio, benchmark,
   AUM), then peer/benchmark comparison, then overlap flags, then the
   view with the disclosure block. One line up top on where the fund
   data came from — live via INDmoney, or web-sourced — since it's
   rarely live Groww/Kite/Upstox data. Formal version:
   `reference/templates/mutual-fund-analysis.md`.
