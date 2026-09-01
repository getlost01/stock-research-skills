---
name: watchlist-monitor
description: Sweep the watchlist in PORTFOLIO-PLAN.md — which triggers are hit, which are near, and what changed on the names now in range. Use when the user asks about their watchlist, whether anything hit their price, or "anything worth looking at today". Read-only.
---

# Watchlist Monitor

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (data efficiency, news freshness,
completeness checklist, disclosure) apply. Steps name capabilities —
resolve each against `reference/BROKER-CAPABILITIES.md`.

Built to be run often, so it must stay cheap: one batched price call for
the whole list, and depth only on what's actually in range.
`market-pulse` covers what the *market* did; this covers what the user
said they were waiting for.

## Steps

1. **Read the watchlist.** `PORTFOLIO-PLAN.md` — **Watchlist / themes**
   (name, why interested, trigger), plus **Exclusions**, **Risk limits**
   and **Deployable capital** (a hit with no cash behind it is a
   different conversation), and the **Decision log** so a name the user
   passed on isn't re-pitched as news. Empty watchlist → say so and offer
   `portfolio-plan-builder`. Never substitute a generic idea list for
   names the user actually chose.

2. **Resolve each trigger into something testable**: a **price level**, a
   **valuation level** (P/E, yield, market cap), an **event** ("after Q2
   results", "once the lock-in expires"), or a **theme criterion** ("any
   index ETF under 0.5% TER"). Anything vaguer is a finding — say it
   can't be checked and propose a concrete rewrite.

3. **Price the list in one call.** Symbol-search capability once for
   unresolved identifiers, then a single batched LTP call for every
   price-level name. The core output is **distance to trigger in %** —
   compute it for all of them, sort by it, and stop there for anything
   far away. Valuation triggers: fundamentals capability for those names
   only. Theme triggers aren't screened here — hand them to
   `new-investment-screener`, which is built for it.

4. **Go deeper only on hit or near (~5%).** Technical indicators (batched)
   and 1–2 date-anchored `WebSearch` calls per name. The question worth
   answering is *why* the level arrived: a de-rating on real news is not
   the same opportunity as ordinary drift toward a number the user picked
   months ago. Say which it looks like.

5. **Check a hit against the rest of the plan before treating it as
   actionable** — pull equity holdings and test for existing exposure or
   overlap (an "opportunity" the user already owns twice over isn't one),
   single-stock and sector headroom under **Risk limits**, exclusions,
   and cash under **Deployable capital**.

6. **Present.** One table sorted by distance: name, trigger, current
   value, distance, status (hit / near / far), one-line note. Prose only
   for hit and near names. A hit that carries a view needs the
   completeness checklist — including that the trigger firing is not
   itself a thesis — and the disclosure block; a sweep where nothing hit
   carries no view and needs no disclosure. Data as-of near the top.
   Formal version: Watchlist Sweep in `reference/REPORT-TEMPLATES.md`.

7. **Hand off.** `stock-research` to actually underwrite a name that hit,
   `new-investment-screener` for theme rows, `earnings-watch` for
   results-dependent triggers, `etf-tracking-quality` when the candidate
   is an index vehicle. Propose watchlist edits (a fired trigger to
   retire, a stale row) for `portfolio-plan-builder` to write.
