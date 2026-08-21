---
name: market-pulse
description: Market briefing — movers, trending stocks and funds, macro/budget notes, calendar context — with anything touching the user's holdings called out. Use when the user asks what's happening in the market, for a briefing, or about market movers. Read-only.
---

# Market Pulse / Briefing

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (news sourcing, preferred outlets)
apply.

## Steps

1. **Timing first.** `resolve_market_time_and_calendar` — open or
   closed, and near-term holidays/expiries/settlement worth flagging.

2. **What's moving.** `fetch_market_movers_and_trending_stocks_funds` for
   gainers/losers and trending stocks and funds; `curate_symbols` /
   `fetch_curated_fno` to narrow to a theme or F&O on request.

3. **Macro.** `get_budget_announcement` in budget season or when the
   user asks about policy impact.

4. **Headline macro news (optional).** `WebSearch` for RBI/Fed policy,
   budget, or major global-market news the tools above wouldn't surface
   — worth it on request or around known event windows, not for a
   routine "what's moving" check.

5. **Connect to the user's book.** Cross-check movers against
   `get_equity_portfolio_holdings` (and the fund list in
   `PORTFOLIO-PLAN.md`, since the MCP returns no fund data) so the
   briefing flags what actually affects them, not generic noise. Check
   `PORTFOLIO-PLAN.md`'s **watchlist** too: a name whose trigger has
   been hit today is the single most useful line a briefing carries —
   state the trigger and the current level.

6. **Present:** "what matters today" first (especially anything touching
   holdings), then broader movers, then any macro note. Keep it
   scannable — a briefing, not a deep dive; point to `stock-research`
   for any specific name. Briefings are usually ephemeral, so a chat
   answer is the right default; the Market Pulse template in
   `reference/REPORT-TEMPLATES.md` is there if the user wants one saved.
