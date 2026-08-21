---
name: ipo-watch
description: Review upcoming or ongoing IPOs and give a subscribe/avoid view, using live Groww IPO data compared against listed peers. Use when the user asks about IPOs, an upcoming listing, or whether to apply for/subscribe to an IPO. Never places any application/order.
---

# IPO Watch

Read-only IPO research. See `reference/READ-ONLY-POLICY.md` for the hard read-only rule —
this never applies to/subscribes for an IPO on the user's behalf. See
`reference/RESEARCH-STANDARDS.md` for the peer-comparison framework, news-sourcing
rules, and disclosure block.

## Steps

1. **List what's live/upcoming.** `fetch_ipo_listings` for the current
   IPO calendar (or the specific one the user named).

2. **Detail.** `fetch_ipo_details` for the named IPO — price band, lot
   size, dates, issue size, and whatever business/financial detail is
   available.

3. **Benchmark against listed peers.** Use `fetch_fundamentals_screener` /
   `fetch_stocks_fundamental_data` on comparable listed companies in the
   same sector to sanity-check the valuation the IPO is asking for, per
   the peer framework in `reference/RESEARCH-STANDARDS.md`.

4. **News (mandatory).** `WebSearch` for recent coverage — subscription
   status/GMP chatter (label clearly as informal/unofficial sentiment, not
   fact), anchor investor detail, any red flags (litigation, auditor
   issues, promoter background) raised by business press ahead of listing.

5. **Check portfolio fit.** `get_equity_portfolio_holdings` —
   would this add useful diversification, or is the user already
   overweight this sector (cross-check `PORTFOLIO-PLAN.md` limits if set)?

6. **Form a view**: subscribe (and rough sizing rationale — e.g. "small,
   speculative allocation" vs. "core-sized") / neutral / avoid, with the
   valuation-vs-peers, news, and portfolio-fit reasoning shown. Note
   listing-day volatility risk explicitly if relevant.

7. **Present:** key IPO facts up top (dates, price band, lot size), then
   the peer-valuation comparison, then news findings, then the view — and
   remind the user they apply/subscribe themselves via Groww. Close with
   the `reference/RESEARCH-STANDARDS.md` disclosure block. Optional: use the IPO
   Note template in `reference/REPORT-TEMPLATES.md` for a formal/saveable version —
   save under `reports/` per its naming convention if the user wants it
   kept.
