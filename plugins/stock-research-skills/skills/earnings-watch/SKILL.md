---
name: earnings-watch
description: Track upcoming results dates for the user's holdings and review announced results — vs. expectations, price reaction, thesis check. Use when the user asks about upcoming earnings/results, "who reports this week", or how a holding's results were. Read-only.
---

# Earnings Watch

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (freshness, data efficiency,
disclosure) apply. Steps below name capabilities — resolve each against
`reference/BROKER-CAPABILITIES.md` for the broker(s) in `BROKERS.md`.

No broker's MCP configured here has an earnings calendar — dates and
results content come from `WebSearch` (NSE/BSE announcements, company IR,
business press), tied back to live price and technical data (technicals
are Groww-only today).

## Steps

1. **Scope to what's owned.** Equity holdings capability, from every
   active broker, plus any name the user adds, anything on the
   `PORTFOLIO-PLAN.md` watchlist, and (if INDmoney is active) its saved
   watchlist capability — unverified, so confirm before trusting
   that one. Don't build a market-wide calendar. Read the plan's
   **position theses** while you're there — their invalidators are what
   step 3 tests.

2. **Upcoming results.** Date-anchored `WebSearch` ("Q2 FY26 results date
   <company>"), batched sensibly: one sector/index results-calendar
   search for the larger names before per-name searches for the rest.
   Present as a dated calendar, nearest first.

3. **For results already announced:**
   - `WebSearch` the actual numbers — revenue/EBITDA/PAT **vs. analyst
     consensus where findable** (not just YoY/QoQ), management
     commentary, guidance changes. When consensus isn't findable, say so
     explicitly rather than silently comparing to YoY only — a beat on
     YoY that misses consensus is a miss, not a beat. Cite outlet + date.
   - Price reaction: batched LTP capability, and where the reaction
     matters to the verdict, the technical-indicators / historical-candles
     capabilities (daily) for the post-result move vs. the prior trend —
     a good result sold into is a different signal from a good result
     bought. Groww-only today.
   - **Thesis check:** test the result against the recorded invalidator
     in `PORTFOLIO-PLAN.md` — hit, approached, or clear? Quote the
     invalidator and the number that meets or misses it. Where none is
     recorded, say so and offer `portfolio-plan-builder`: "the thesis
     still holds" means nothing if it was never written down.

4. **Flag what needs action.** Only holdings where the result — or an
   imminent one plus stretched positioning — changes the picture get a
   view, per the completeness checklist. Routine in-line results are one
   line each.

5. **Present:** upcoming calendar first (date, name, portfolio weight),
   then reviewed results (one line each, detail only for flagged names),
   then views with the disclosure block. `stock-research` re-underwrites
   a single name. Formal version: `reference/templates/earnings-watch.md`.
