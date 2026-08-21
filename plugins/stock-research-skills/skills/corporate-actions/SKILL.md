---
name: corporate-actions
description: Track corporate actions — dividends, splits, bonuses, buybacks, rights issues, demergers — affecting the user's holdings, with record/ex-dates and what each means for cost basis, taxes, and the hold/tender decision. Use when the user asks about a dividend/split/bonus/buyback/rights issue on a holding, record or ex-dates, or "any corporate actions coming up". Never places any order or tenders any shares.
---

# Corporate Actions Tracker

Read-only. See `reference/READ-ONLY-POLICY.md` for the hard read-only rule and
`reference/RESEARCH-STANDARDS.md` for news freshness rules and the disclosure
block (applies when a view — e.g. tender/don't-tender — is given).

Groww's MCP has no corporate-actions feed — this is `WebSearch`-driven
(NSE/BSE corporate announcements pages are the primary source; business
press secondary), anchored to the user's real holdings.

## Steps

1. **Scope to holdings.** `get_equity_portfolio_holdings` (and the ETF/
   MF list via `get_mutualfund_details` for fund-level actions like
   mergers). Only search for actions on names actually held unless the
   user names something else.

2. **Search announced/upcoming actions.** `WebSearch` per the freshness
   rules — date-anchored queries against NSE/BSE announcements. For each
   action found, capture: type, ratio/amount, **ex-date and record date**
   (the dates that determine eligibility — state them exactly, sourced),
   and payment/credit date where known.

3. **Explain the mechanics per action**, tied to the user's actual
   quantity and cost basis:
   - **Dividend**: amount × qty held, taxed as income at slab.
   - **Split/bonus**: new qty and adjusted per-share cost; note that
     bonus shares reset the holding-period clock for STCG/LTCG on the
     bonus portion — flag interplay with `tax-capital-gains`.
   - **Buyback**: tender price vs. LTP (`get_ltp`, batched), acceptance-
     ratio reality for small shareholders, tax treatment — and an
     explicit tender/don't-tender view meeting the Recommendation
     completeness checklist.
   - **Rights issue**: price vs. market, dilution if not subscribed, and
     that rights entitlements are separately tradeable.
   - **Demerger/merger**: entitlement ratio and cost-basis split rules.

4. **Present:** a dated table first (action, name, type, key dates,
   per-holding impact in ₹), then detail only where a decision exists
   (buyback tender, rights subscribe). Views carry the disclosure block;
   any application/tender the user does themselves in Groww/their RTA.
   Optional: Corporate Actions template in `reference/REPORT-TEMPLATES.md`, saved
   under `reports/` if wanted.
