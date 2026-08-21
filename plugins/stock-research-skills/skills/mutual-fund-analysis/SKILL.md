---
name: mutual-fund-analysis
description: Deep dive or peer comparison on mutual funds — expense ratio, alpha vs. benchmark/category, manager consistency, overlap with the user's other holdings. Use when the user asks about a specific mutual fund, wants to compare funds, or asks if their fund is worth holding vs. alternatives. Never places any order (no SIP start/stop/modify either).
---

# Mutual Fund Analysis

Read-only. See `reference/READ-ONLY-POLICY.md` for the hard read-only rule and
`reference/RESEARCH-STANDARDS.md` for the peer-comparison framework and disclosure
block used here — apply both.

## Steps

1. **Pull the fund(s).** `get_mutualfund_details` for the named fund(s)
   and for the user's existing MF holdings (for overlap/fit checks).

2. **Classify** — category (large-cap/flexi-cap/debt/hybrid/index/sector
   etc.) and benchmark, since everything below is relative to that.

3. **Peer comparison** (see `reference/RESEARCH-STANDARDS.md` → Mutual funds/ETFs):
   expense ratio, rolling returns vs. benchmark and category average,
   AUM trend, manager tenure/consistency. For index funds/ETFs, tracking
   error matters more than raw returns — judge accordingly.
   - `fetch_etf_screener` if comparing against a passive alternative in the
     same space (e.g. "is this active large-cap fund beating a Nifty 50
     ETF after fees?" is often the real question).

4. **Overlap check.** Compare top holdings/sector weights against the
   user's other funds and direct stock holdings
   (`get_equity_portfolio_holdings`) — flag meaningful overlap (owning
   three flexi-cap funds that are all secretly 60% the same 15 stocks adds
   cost without adding diversification).

5. **News (optional, use judgment per RESEARCH-STANDARDS.md).** A
   fund-manager change, AMC-level issue, or category-wide regulatory
   change (e.g. SEBI circular affecting a fund category) is worth a quick
   `WebSearch` before a strong view — routine "should I hold this SIP"
   questions usually don't need it.

6. **Form a view**: worth holding / consider switching to [alternative] /
   redeem — with the fee-adjusted performance and overlap reasoning shown,
   never just a raw-returns comparison. Note this never starts, stops, or
   modifies a SIP — that's the user's action in the Groww app.

7. **Present:** fund summary (category, expense ratio, benchmark, AUM)
   first, then the peer/benchmark comparison, then overlap flags, then the
   view. Close with the `reference/RESEARCH-STANDARDS.md` disclosure block. Optional:
   use the Fund Note template in `reference/REPORT-TEMPLATES.md` for a formal/
   saveable version — save under `reports/` per its naming convention
   if the user wants it kept.
