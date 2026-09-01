---
name: thesis-audit
description: Re-test every position thesis in PORTFOLIO-PLAN.md against current data — is the stated reason for owning it still true, and has any invalidator tripped. Use when the user asks why they own something, whether a thesis still holds, or wants a conviction/thesis check across holdings. Read-only.
---

# Thesis Audit

Read-only across the user's live account(s). `reference/READ-ONLY-POLICY.md`
(hard rule) and `reference/RESEARCH-STANDARDS.md` (peer and technical
frameworks, data efficiency, completeness checklist, news freshness,
disclosure) apply. Steps name capabilities — resolve each against
`reference/BROKER-CAPABILITIES.md` for the active broker.

This is the deep, whole-book version of one lens: `portfolio-review`
tests theses as one section among five, and `portfolio-plan-builder`'s
audit mode only reports which invalidators have been hit. Here the
re-test *is* the work — every thesis row, measured against a number.

## Steps

1. **Load the theses.** `PORTFOLIO-PLAN.md` — **Position theses** (thesis,
   invalidator, target weight, per-row `Reviewed` date), plus the
   **Decision log** (a thesis the user already re-affirmed shouldn't be
   re-litigated) and **Risk limits** for the horizon each thesis is
   written against. No theses section → say so and offer
   `portfolio-plan-builder`; never reconstruct a thesis from the fact
   that a position exists.

2. **Reconcile against live holdings** — equity holdings capability from
   every active broker, plus the MF sleeve (**SIP register**, or
   INDmoney's fund holdings if active). Three reconciliation findings,
   all worth reporting even though no market data is involved:
   - a thesis row for something no longer held (stale row),
   - a material holding with no thesis (why is this owned?),
   - actual weight vs. the row's target weight.

3. **Classify each invalidator by what would test it**, before pulling
   anything: a **price level**, a **fundamental number** (margin,
   revenue, debt, payout), an **event or date** (results, verdict,
   approval, lock-in), or **untestable** ("if the story changes"). An
   untestable invalidator is itself the finding — flag it and propose a
   checkable rewrite.

4. **Pull only what the invalidators need**, per the data-efficiency
   rules: one batched LTP call covering every name; technical indicators
   batched (up to 10) only for price-level invalidators; fundamentals
   (single-name or screener) only where the invalidator is fundamental;
   historical candles only for a level or drawdown the indicators don't
   give. A thesis whose invalidator is nowhere near tripping doesn't earn
   a deep dive.

5. **News only for event-shaped invalidators** — date-anchored, 1–2
   searches for that name, per the freshness rules. "Nothing in the last
   30 days on the thing that would break this" is a valid result.

6. **Classify each thesis** and show the number that decided it:
   - **Intact** — invalidator measured, comfortably untripped.
   - **Weakening** — moving toward the invalidator; say how far.
   - **Tripped** — the stated condition is met. The loudest finding in
     the report, whether or not the price agrees.
   - **Untestable** — no measurable invalidator.
   - **Stale** — `Reviewed` older than ~6 months, or written for
     conditions that no longer exist.

   A rising price does not repair a broken thesis, and a falling price
   does not break an intact one — separate the two explicitly.

7. **Present.** Lead with the counts and the tripped rows. Then one table:
   holding, weight, thesis (abbreviated), invalidator, status, measured
   value, suggested action. Detail prose only for tripped/weakening rows,
   each meeting the completeness checklist; data as-of near the top;
   disclosure block, since a "thesis broken → consider reducing" is a
   view. Formal version: Thesis Audit in `reference/REPORT-TEMPLATES.md`.

8. **Propose plan edits, don't write them** — rewritten invalidators,
   refreshed `Reviewed` dates, a thesis row for an unexplained holding.
   Only `portfolio-plan-builder` writes that file. For a tripped thesis,
   hand off to `stock-research` to re-underwrite the name from scratch,
   `rebalancing-planner` to size any change, and `tax-capital-gains`
   before anything gets sold.
