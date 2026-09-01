---
name: dividend-income
description: Project annual dividend income from the user's holdings — per-stock payout, yield-on-cost, upcoming ex-dates, sustainability flags. Use when the user asks about dividend income, portfolio yield, upcoming dividends, or building an income stream. Read-only.
---

# Dividend Income Review

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (data efficiency, freshness,
disclosure) apply. Works over whichever broker(s) `BROKERS.md` names
active — resolve each capability below against
`reference/BROKER-CAPABILITIES.md`.

## Steps

1. **Holdings + prices.** Equity holdings capability, then one batched
   LTP call if that payload lacks current prices.

2. **Dividend data.** Fundamentals (single-name / screener) capability
   for yield and payout metrics — Groww-only among the wired brokers
   today; date-anchored `WebSearch` fills gaps — trailing-12-month DPS,
   declared-but-unpaid dividends and their ex-dates. Don't deep-search
   obvious non-payers; note them as such.

3. **Compute**, per holding and in total: projected annual income
   (trailing DPS × qty, labeled trailing-based, never an assured
   payout); yield on current value *and* yield-on-cost (DPS ÷ avg buy
   price); upcoming ex-dates in the next quarter as a dated list.

4. **Sustainability flags** on the payers that dominate projected
   income: payout ratio vs. earnings *and* vs. free cash flow — a
   dividend covered by earnings but not FCF is funded by working-capital
   or debt tricks, not real cash generation; cut/skip history;
   deteriorating fundamentals funding the payout. A high yield produced
   by a falling price is a warning — say so when the data shows it.

5. **Fit against the plan.** Where `PORTFOLIO-PLAN.md` states an income
   goal, compare and note the gap, including coupon/interest from the
   fixed-income inventory — dividends alone understate the income the
   goal is measured against. New income names route through
   `new-investment-screener`/`stock-research`.

6. **Present:** total projected income and portfolio yield first, then
   the per-holding table (qty, DPS, projected ₹, yield, yield-on-cost,
   next ex-date), then flags, then any view with the disclosure block.
   Formal version: `reference/templates/dividend-income.md`.
