---
name: ipo-watch
description: Sweep the IPO calendar — what's open, closing, upcoming, or awaiting listing — and flag which merit a closer look. Use when the user asks what IPOs are open or coming up, or about allotment and listing dates. Read-only — never applies or subscribes.
---

# IPO Watch

Read-only — never apply, bid, or subscribe on the user's behalf.
`reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (tool availability, freshness,
disclosure where a view results) apply.

This is the calendar sweep: what's live, what's next, what needs a
decision this week. A single named issue the user is actually considering
goes to `ipo-analysis` — don't half-research one here.

## Steps

1. **Pull the calendar.** `fetch_ipo_listings` with `view='open'` and
   `view='upcoming'` — never `view='all'`, which returns ~127K characters
   and overflows context. Use `view='closed'` only when the question is
   about a recent issue's allotment or listing. The payload carries dates,
   price band, lot size and issue size; `fetch_ipo_details` is dead, so
   don't reach for per-issue depth here.

2. **Date it.** `resolve_market_time_and_calendar` — days left to bid on
   each open issue, and which allotment or listing dates land next. What's
   closing in the next 48 hours leads the output; everything else is
   context.

3. **Triage, don't research.** For each open or imminent issue: sector,
   issue size, and whether it's worth the user's attention at all. Screen
   out anything on `PORTFOLIO-PLAN.md`'s **exclusions** list in one line,
   plus anything in a sector already at its plan limit given
   `get_equity_portfolio_holdings`. SME issues get flagged as such —
   thinner liquidity and a different risk profile from a mainboard issue.

4. **News, lightly.** One date-anchored `WebSearch` across the current
   crop for subscription status and anything the business press has
   flagged — not one search per issue. Grey-market chatter, if mentioned
   at all, is labelled informal sentiment.

5. **Present:** one table (issue, open–close dates, band, lot size,
   minimum outlay, SME/mainboard, status) with what's closing soonest
   first, then a short "worth a closer look" line naming the one or two
   issues that merit `ipo-analysis` and why, then anything awaiting
   allotment or listing. No subscribe/avoid verdicts here — triage
   pointers only, so no disclosure block unless a view slipped in. If the
   user wants a call on one of them, run `ipo-analysis`.
