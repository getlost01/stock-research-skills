---
name: dividend-income
description: Project the annual dividend income from the user's holdings — per-stock payout, yield-on-cost, upcoming ex-dates, and flags on dividend cuts or unsustainable payouts. Use when the user asks about dividend income, yield on their portfolio, upcoming dividends, or building/reviewing an income stream. Never places any order.
---

# Dividend Income Review

Read-only. See `reference/READ-ONLY-POLICY.md` for the hard read-only rule and
`reference/RESEARCH-STANDARDS.md` for data-efficiency and news freshness rules and
the disclosure block (applies when suggesting income-oriented changes).

## Steps

1. **Holdings + prices.** `get_equity_portfolio_holdings`, then one
   batched `get_ltp` if the holdings payload lacks current prices.

2. **Dividend data per holding.** `fetch_stocks_fundamental_data` /
   `fetch_fundamentals_screener` for dividend yield and payout metrics
   where available; `WebSearch` (date-anchored) fills gaps — trailing
   12-month dividend per share, declared-but-unpaid dividends and their
   ex-dates. Don't deep-search obvious non-payers; note them as such.

3. **Compute, per holding and in total:**
   - Projected annual income: trailing DPS × qty (label it as trailing-
     based, not guaranteed — never imply an assured payout).
   - Yield on current value and **yield-on-cost** (DPS ÷ avg buy price).
   - Upcoming ex-dates within the next quarter, as a dated list.

4. **Sustainability flags** — where income is a meaningful goal, check
   the payers that dominate the projected income: payout ratio vs.
   earnings, dividend cut/skip history, deteriorating fundamentals
   funding an unsustainable payout. A high yield from a falling price is
   a warning, not a feature — say so when the data shows it.

5. **Fit against the plan.** If `PORTFOLIO-PLAN.md` states an income
   goal, compare projected income against it and note the gap. Any
   suggestion to add income-oriented names routes through
   `new-investment-screener`/`stock-research` — don't shortcut it here.

6. **Present:** total projected annual income and portfolio yield first,
   then the per-holding table (qty, DPS, projected ₹, yield, yield-on-
   cost, next ex-date), then sustainability flags, then any view with
   the disclosure block. Optional: Dividend Income template in
   `reference/REPORT-TEMPLATES.md`, saved under `reports/` if wanted.
