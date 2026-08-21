---
name: market-pulse
description: Give a market briefing — top movers, trending stocks/funds, relevant macro/budget announcements, and market calendar/timing context — and note anything relevant to the user's own holdings. Use when the user asks "what's happening in the market", for a market update/briefing, or about market movers/trending stocks. Read-only.
---

# Market Pulse / Briefing

Read-only market context. See `reference/READ-ONLY-POLICY.md` for the hard read-only
rule and `reference/RESEARCH-STANDARDS.md` for news-sourcing rules/preferred outlets.

## Steps

1. **Timing context first.** `resolve_market_time_and_calendar` — is the
   market open, and any near-term holidays/expiries/settlement dates worth
   flagging.

2. **What's moving.**
   - `fetch_market_movers_and_trending_stocks_funds` — top gainers/losers,
     trending stocks and funds.
   - `curate_symbols` / `fetch_curated_fno` for a curated view if the user
     wants it narrowed to a theme or F&O specifically.

3. **Macro context.** `get_budget_announcement` when relevant (budget
   season, or the user asks about policy/announcement impact).

4. **Headline macro news (optional, use judgment).** `WebSearch` for
   anything RBI/Fed policy, budget, or major global-markets news the
   Groww tools above wouldn't surface directly — worth doing on request or
   around known event windows, not needed for a routine "what's moving"
   check.

5. **Connect to the user's own portfolio** — cross-check movers/trending
   names against `get_equity_portfolio_holdings` / `get_mutualfund_details`
   so the briefing calls out anything that actually affects the user,
   not just generic market noise.

6. **Present:** a short "what matters today" summary first (especially
   anything touching the user's holdings), then the broader movers/trending
   list, then any macro note. Keep it scannable — this is a briefing, not a
   deep dive (point to `stock-research` for that on any specific name).
   Optional: use the Market Pulse template in `reference/REPORT-TEMPLATES.md` only
   if the user wants a saveable daily-briefing format; a normal chat
   answer is the better default here since briefings are usually
   ephemeral. If saved, use `reports/` per that file's naming convention.
