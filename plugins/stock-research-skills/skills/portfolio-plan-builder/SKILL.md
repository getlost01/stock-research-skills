---
name: portfolio-plan-builder
description: Build or update the user's PORTFOLIO-PLAN.md by interviewing them against their real holdings — targets, risk limits, rebalancing rules, theses, fixed-income inventory, SIP register, tax context. Use when that file is missing, stale, or lacks a section another skill needs, or when the user asks to set up or change their plan. Writes only that file.
---

# Portfolio Plan Builder

Interview-driven setup and maintenance of `PORTFOLIO-PLAN.md` — the file
every other skill reads for the user's *intent* that Groww's MCP can't
know. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (frameworks, disclosure) apply.

**Write scope.** The one skill that writes a file the user owns:
`PORTFOLIO-PLAN.md` at their project root, from
`reference/PORTFOLIO-PLAN.example.md`. Nothing else — never a broker
action, never another file, never an edit the user hasn't seen.

**A conversation, not a form.** The plan's value is the thinking it
forces, so probe and push back — a plan recording whatever the user said
first is worth less than one that survived a few good questions.
Interview in **rounds of 2–4 questions**, grounded in what they actually
hold. Never dump the template as a questionnaire.

## When this runs

- **File doesn't exist** → offer this *before* doing the analysis the
  user asked for. Don't proceed on guessed targets, and don't build it
  unasked either: say what's missing, what it unlocks, and offer (a)
  build it now, 5–10 minutes, (b) answer just the minimum for this one
  question, (c) proceed without it, stating the assumptions used.
- **A needed section is blank or stale** → fill *that section*, then hand
  back. Don't turn a rebalancing question into a full plan interview.
- **The user asks to create, review, refresh, or change the plan** →
  full or section-scoped pass, as they prefer.

## Steps

1. **Locate or create.** Check the project root. If absent, copy the
   template and confirm `PORTFOLIO-PLAN.md` is git-ignored — it will hold
   real financial data. If present, inventory what's filled, blank,
   marked `n/a` (a decision — don't re-ask), and stale per its
   `_Last reviewed:_` stamps.

2. **Ground the interview in real data before asking anything.** One
   batched pass — noting that this covers equity and ETFs only, since the
   MCP has no mutual fund data: `get_equity_portfolio_holdings`,
   `get_my_trading_positions_today`,
   `get_available_margin_details`, one batched `get_ltp`, and
   `fetch_stocks_fundamental_data` / screener data for sector and
   market-cap classification. Compute the current bucket split, top-10
   weights, and sector weights.

   This turns abstract questions concrete: not "what's your mid/small-cap
   target?" but "you're at 41% mid/small-cap today, mostly in three names
   — is that where you meant to be?" Users answer the second honestly and
   guess at the first.

3. **Interview in this order** — the template documents each field; what
   matters here is the sequence and what to press on:

   1. **Goals & horizon** — what the money is for, when it's needed, what
      an unacceptable outcome looks like. Everything else follows, so
      start here even though the template starts elsewhere.
   2. **Risk limits** — appetite, then anchored against their book: "a 30%
      drawdown on today's holdings is about ₹X — hold, add, or sell?"
      Single-stock, sector, and per-fund/AMC caps, each marked hard or soft.
   3. **Target allocation** — propose a split derived from their answers
      *and* their current book, then let them correct it. Proposing beats
      asking cold, as long as the reasoning shows and it's clearly a
      draft. Set drift bands per bucket.
   4. **Rebalancing rules** — cadence, threshold, correction order, selling
      constraints, untouchables.
   5. **Fixed income + inventory** — targets, then the inventory table.
      Say plainly that direct bonds/NCDs/FDs/SGBs are invisible to the
      MCP, so this table is the only source of truth for
      `bond-ladder-planner` and `rate-watch`.
   6. **SIP register** — this is the whole mutual fund inventory, not
      just SIP amounts: Groww's MCP returns no fund data whatsoever, so
      ask for every fund held with units, average cost, and current
      value, then the SIP rows on top. Say plainly that without this
      table their fund sleeve is invisible to every skill and
      `portfolio-review` will be reporting on the equity book alone. Flag
      lump-sum-only funds and paused SIPs explicitly.
   7. **Tax context** — note where lot-level data isn't available rather
      than assuming.
   8. **Position theses** — for the top holdings, why held plus a
      *checkable* invalidator (a number, event, or date). Highest-leverage
      section: without one, `earnings-watch` and `portfolio-review` can
      only report that the price moved. If they can't name one for a large
      position, that's the finding — record it as an open question rather
      than inventing a thesis.
   9. **Exclusions, deployable capital, income goal, watchlist, output
      preferences** — quick round, mostly preference.

4. **Grill — the part that earns the file.** Reflect contradictions back
   with the numbers, once each, as a question with both branches rather
   than a verdict on their judgment:
   - Targets that don't sum to 100%, or a fixed-income target that
     contradicts the bucket table.
   - Stated appetite vs. the actual book (self-described conservative,
     60% small-caps), or vs. behaviour they describe.
   - Horizon vs. known outflows — money needed in 18 months sitting in a
     seven-year bucket.
   - Limits already breached today: is the limit the intent, or is the
     position? One has to move.
   - Bands so tight they'd trigger constant trading, or so wide the plan
     never binds.
   - Overlap they may not see — funds converging on the same top
     holdings, an ETF duplicating direct holdings.
   - An income goal the book can't plausibly produce; show the projected
     figure against it.

   Accept "yes, deliberately" and record it as such, ideally in the
   decision log, so no skill re-litigates it later.

5. **Write it.** Fill the template, keeping its exact section headings and
   anchors — other skills key off them.
   - Only what the user actually said. No inferred numbers, no
     placeholder targets. Undecided → blank; decided there's no
     constraint → `n/a`.
   - Follow the template's conventions block (dates `YYYY-MM-DD`, rupees,
     percentages of current market value, exchange tickers).
   - Stamp every section touched with today's `_Last reviewed:_`.
   - **Show the diff and get confirmation before writing**, and honour
     the file's own "may skills propose edits" preference on later runs.
   - Never commit it, and never echo real figures into any example or
     tracked file.

6. **Close the loop.** State which sections are now filled, which are
   still blank and what that blocks (see the template's "How the skills
   use this file" table), and the natural next step — usually
   `rebalancing-planner` against the fresh targets, or back to the
   original question. `stock-research` is the way to test any shaky
   thesis that came out of the interview.

## Maintenance mode

For an existing plan, audit rather than re-interview:

- Sections stale past ~6 months (a quarter for tax and SIP), by their own
  stamps.
- Plan vs. reality now: limits breached, buckets outside their bands,
  theses that have hit an invalidator, watchlist triggers fired,
  decision-log rows due to revisit.
- Fixed-income rows that have matured, and SIPs that changed.

Present that as a short findings list, then offer to update only the rows
that moved. A yearly full pass is worth it; a monthly one isn't.

## Presentation

Lead with what the interview covers and roughly how long. Keep rounds
short — questions as a numbered list, one line each, with the grounding
number attached where there is one. Close with the filled/blank summary
table and the next step. No disclosure block for the interview itself; if
step 4 ends up giving a positioning opinion, it applies to that part.
