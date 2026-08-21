---
name: corporate-actions
description: Track dividends, splits, bonuses, buybacks, rights issues, and demergers on the user's holdings — key dates, cost-basis and tax impact, hold/tender decisions. Use when the user asks about any of those, record or ex-dates, or "any corporate actions coming up". Read-only.
---

# Corporate Actions Tracker

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (freshness, disclosure when a
tender/don't-tender view is given) apply.

Groww's MCP has no corporate-actions feed, so this is `WebSearch`-driven
— NSE/BSE announcement pages primary, business press secondary —
anchored to real holdings.

## Steps

1. **Scope to holdings.** `get_equity_portfolio_holdings`, plus
   `get_mutualfund_details` for fund-level actions like mergers. Only
   search names actually held unless the user names another.

2. **Search announced and upcoming actions.** Date-anchored `WebSearch`
   against NSE/BSE announcements. Per action: type, ratio/amount,
   **ex-date and record date** (exact and sourced — they determine
   eligibility), and payment/credit date where known.

3. **Explain the mechanics**, tied to the user's actual quantity and
   cost basis:
   - **Dividend** — amount × qty, taxed as income at slab.
   - **Split/bonus** — new qty and adjusted per-share cost. Bonus shares
     reset the holding-period clock on the bonus portion; flag the
     interplay with `tax-capital-gains`.
   - **Buyback** — tender price vs. LTP (batched `get_ltp`), realistic
     acceptance ratio for small shareholders, tax treatment, and an
     explicit tender/don't-tender view per the completeness checklist.
     Check the plan's tax context and selling constraints first: a tender
     is a sale, so a lock-in or LTCG-threshold hold changes the answer.
   - **Rights issue** — price vs. market, dilution if not subscribed, and
     that entitlements trade separately.
   - **Demerger/merger** — entitlement ratio and cost-basis split.

4. **Present:** a dated table first (action, name, type, key dates,
   per-holding ₹ impact), then detail only where a decision exists
   (tender, rights). Views carry the disclosure block; the user applies
   or tenders themselves. Formal version: Corporate Actions in
   `reference/REPORT-TEMPLATES.md`.
