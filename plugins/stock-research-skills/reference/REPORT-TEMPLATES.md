# Report Templates

Optional output scaffolds. Default to a normal conversational answer;
reach for a template only when the user asks for "a report", wants to
save/share/export it, or the output has enough line items that structure
genuinely helps.

Fill every `[bracket]`; drop any section with nothing to say rather than
leaving it templated-but-empty. Where a report carries a recommendation it
ends with the disclosure block from `RESEARCH-STANDARDS.md` and satisfies
that file's **Recommendation completeness** checklist — neither is
optional. Keep reports tight: numbers in tables, reasoning in short
prose, no figure stated twice. A report's value is per line, not in
length.

## Where saved reports go

`reports/<skill-slug>-<subject-slug>-<YYYY-MM-DD>.md` at the repo root —
git-ignored, holds real financial data, never commit it. Create the
directory on first use.

- `<skill-slug>`: the skill's own name, shortened where obvious
  (`mutual-fund`, `bond`, `rebalancing`, `fno`, `screener`, `ipo`,
  `bond-ladder`, `fund-house`, `plan-audit`).
- `<subject-slug>`: the symbol/fund/topic in kebab-case (`tcs`,
  `hdfc-flexicap`, `full-portfolio`); omit for portfolio-wide reports.
- Date generated, not the data's as-of time.

e.g. `reports/stock-research-tcs-2026-08-21.md`,
`reports/tax-capital-gains-2026-08-21.md`.

Regenerating the same report the same day overwrites it — these reflect
latest data, not a version history.

---

## Portfolio Review — `portfolio-review`

```markdown
# Portfolio Review — [date/time]

## Summary
- Total value: [₹X] across [N] holdings ([stocks/ETFs/MFs/F&O breakdown])
- Overall skew: [e.g. "heavily large-cap, tech-sector concentrated"]
- Top findings: [1-3 bullets — the things that actually matter]

## Holdings
| Holding | Qty | Avg. Price | LTP | Value | P&L | Weight % |
|---|---|---|---|---|---|---|

## Allocation
[by asset class / sector / market-cap — table or bullets]

## Risk flags
- [concentration, correlation/overlap, anything technically or
  fundamentally deteriorating]

## Recommendations
- [buy/hold/reduce/avoid per flagged holding, with reasoning]

## See also
[stock-research / rebalancing-planner / tax-capital-gains as relevant]

[disclosure block]
```

---

## Stock / ETF Research — `stock-research`

```markdown
# Research Note: [Symbol] — [date/time]

## Verdict
[Buy / Accumulate / Hold / Reduce / Avoid] — [one-line why] — Horizon: [X]

## Snapshot
LTP: [₹X] (as of [time]) | [day/1M/1Y change] | Market cap: [X] | Sector: [X]

## Fundamentals vs. peers
| Metric | [Symbol] | Peer avg. | Sector range |
|---|---|---|---|
| P/E | | | |
| P/B | | | |
| ROE | | | |
| Debt/Equity | | | |
| Revenue growth | | | |

## Technical read
- Trend (daily/weekly): [above/below 50/100/200-DMA, direction]
- Momentum: [RSI/MACD read]
- Volatility: [regime]
- Key levels: [support/resistance]
- Net technical posture: [aligned/conflicting signals, weighted how]

## News
- [outlet, date] — [headline/finding]
- [outlet, date] — [headline/finding]

## Portfolio fit & position disclosure
[does the user already hold this or an overlapping exposure; concentration
impact, diversification value]

## Key risks to this view
- [2-3 specific invalidators — levels, events, thesis-breakers]

[disclosure block]
```

---

## Mutual Fund Analysis — `mutual-fund-analysis`

```markdown
# Fund Note: [Fund name] — [date/time]

## Snapshot
Category: [X] | Benchmark: [X] | Expense ratio: [X%] | AUM: [X] | Manager tenure: [X]

## Performance vs. benchmark/category
| Period | Fund | Benchmark | Category avg. |
|---|---|---|---|
| 1Y | | | |
| 3Y | | | |
| 5Y | | | |

## Overlap with existing holdings
[funds/stocks with meaningful overlap, and %]

## View
[worth holding / switch to X / redeem — with reasoning and horizon]

## Key risks to this view
- [e.g. manager change, category regulation, style rotation]

[disclosure block]
```

---

## Bond / Debt Instrument — `bond-analysis`

```markdown
# Bond Note: [Issuer/Instrument] — [date/time]
Source: [Groww / external — label each figure below accordingly]

## Instrument facts
Issuer: [X] | Rating: [X, agency, outlook] | Coupon: [X%] | Maturity: [X] | Type: [Gov/PSU/Corporate, secured/unsecured]

## Yield & risk
- YTM: [X%] vs. comparable G-Sec: [X%] → spread: [X bps]
- Duration: [X years] — rate sensitivity: [note]
- Issuer/sector concentration impact on fixed-income allocation: [X%]
- Liquidity: [note, incl. if unclear from available data]

## View
[attractive / fair / avoid at current yield, with reasoning, horizon
(hold-to-maturity vs. tradeable), and sizing note]

## Key risks to this view
- [e.g. rating downgrade trigger, rate path, exit liquidity]

[disclosure block, plus explicit note on external-data freshness]
```

---

## Rebalancing Plan — `rebalancing-planner`

```markdown
# Rebalancing Plan — [date/time]

## Target vs. actual
| Bucket | Target % | Actual % | Diff | ₹ Amount |
|---|---|---|---|---|

## Concentration/overlap flags
- [single-stock/sector limits breached, or fund overlap]

## Suggested moves
1. [Trim/add — instrument or bucket, ~₹X / ~X%, why]
2. ...

[disclosure block]
```

---

## F&O Position Review — `fno-analysis`

```markdown
# F&O Position Review — [date/time]

## Net exposure
Net delta: [X] | Net theta: [X] | Notional: [₹X] | Margin used/available: [₹X / ₹X]

## Positions
| Underlying | Contract | Qty | Greeks (Δ/Θ/Γ/V) | P&L | Expiry |
|---|---|---|---|---|---|

## Flags
- [expiring soon, outsized directional/theta risk, event exposure]

## [If a candidate strategy was analyzed]
Payoff: [max profit/loss, breakevens] | Margin impact: [₹X]

[disclosure block, if a view/suggestion was given]
```

---

## Tax / Capital Gains — `tax-capital-gains`

```markdown
# Capital Gains Snapshot — [date/time]
Estimates only — verify against Groww's official Capital Gains Statement.

## Holdings
| Holding | Qty | Cost basis | Current value | Unrealized G/L | LTCG/STCG |
|---|---|---|---|---|---|

## Harvesting observations
- [loss-harvesting candidates, LTCG/STCG threshold timing notes]

[disclosure block + CA-consultation caveat]
```

---

## Investment Screener Shortlist — `new-investment-screener`

```markdown
# Screener Shortlist: [brief/theme] — [date/time]

| Name | Type | Key metric(s) | Rationale | Recent news |
|---|---|---|---|---|

## Why not others
[brief note on notable names screened out and why, if useful]

[disclosure block]
```

---

## IPO Note — `ipo-analysis`

```markdown
# IPO Note: [Company] — [date/time]

## Facts
Price band: [₹X-Y] | Lot size: [X] | Min outlay: [₹X] | Dates: [open-close]
Issue size: [₹X] | Fresh issue / OFS: [X% / Y%]

## Financials (from RHP — [source, date])
| ₹ Cr | FY[n-2] | FY[n-1] | FY[n] |
|---|---|---|---|
| Revenue / EBITDA margin / PAT / Debt / OCF | | | |

## Implied valuation vs. listed peers
| Metric | [Company] at [band low–high] | Peer median | Peers |
|---|---|---|---|
| P/E, P/B, EV/EBITDA, RoE | | | |

## Issue structure flags
- [objects of the issue, post-issue promoter holding, pledge, lock-in expiry]

## Demand & news
- [outlet, date] — [subscription by category, anchors, flags]
- GMP: [X] — informal, unregulated sentiment; predicts nothing

## View
[Subscribe (core-sized) / Subscribe small, speculative / Neutral / Avoid]
Listing horizon: [...] · Long-term horizon: [...] — stated separately
Portfolio fit / position disclosure: [...]

## Key risks to this view
- [valuation premium vs. peers, weak QIB book, lock-in supply, named
  business risk]

[disclosure block]
```

`ipo-watch` produces no note — its output is the calendar table plus a
"worth a closer look" pointer, which doesn't need saving.

---

## Earnings Watch — `earnings-watch`

```markdown
# Earnings Watch — [date/time]

## Upcoming results (holdings)
| Date | Company | Portfolio weight | Notes (expectations/events) |
|---|---|---|---|

## Results reviewed
| Company | Result vs. expectation | Price reaction | Thesis status |
|---|---|---|---|

## Flagged — views
[per flagged holding: view + horizon, basis, key risks]

[disclosure block if any view given]
```

---

## Corporate Actions — `corporate-actions`

```markdown
# Corporate Actions — [date/time]

| Company | Action | Ratio/Amount | Ex-date | Record date | Impact on holding |
|---|---|---|---|---|---|

## Decisions needed
[buyback tender / rights subscribe — view + horizon, basis, key risks each]

## Tax/cost-basis notes
[bonus holding-period resets, dividend income at slab, etc.]

[disclosure block if any view given]
```

---

## Dividend Income — `dividend-income`

```markdown
# Dividend Income — [date/time]

Projected annual income: [₹X] (trailing-DPS based, not assured) | Portfolio yield: [X%]

| Holding | Qty | DPS (TTM) | Projected ₹ | Yield | Yield-on-cost | Next ex-date |
|---|---|---|---|---|---|---|

## Sustainability flags
- [payout ratio / cut history / deteriorating fundamentals]

[disclosure block if any view given]
```

---

## Bond Ladder — `bond-ladder-planner`

```markdown
# Bond Ladder — [date/time]
Direct-bond inventory from PORTFOLIO-PLAN.md as of [date]; curve data: [source, date]

## Ladder
| Maturity year | ₹ maturing | Instruments | Locked yield |
|---|---|---|---|
(Debt funds by duration bucket below the table)

## vs. plan
Fixed-income actual [X%] vs. target [X%] | Credit floor breaches: [none/list]

## Flags
- [gaps/lumps, reinvestment risk ₹ estimate, duration fit]

## Suggested moves
1. [tenor bucket to fill / action at each maturity — with basis and key risks]

[disclosure block + external-data freshness note]
```

---

## Rate Watch — `rate-watch`

```markdown
# Rate Watch — [date/time]

## Rate picture
Repo: [X%] ([stance], MPC [date]) | 10Y G-Sec: [X%] ([move since last policy]) | CPI: [X%] | Next MPC: [date]
Sources: [RBI/…, dated]

## Your exposure
| Holding | Type/duration | Sensitivity | Implication |
|---|---|---|---|

## Positioning notes
[scenario-framed, both branches where uncertain — never a rate prediction as fact]

[disclosure block if any view given]
```

---

## SIP Review — `sip-review`

```markdown
# SIP Review — [date/time]

| Fund | SIP ₹/mo | vs. benchmark (1Y/3Y) | Rank trend | Flag | Verdict |
|---|---|---|---|---|---|

## Detail (non-continue verdicts only)
[per fund: basis, key risks, redirect bucket if any]

## Set-level
- Overlap: [finding] | Allocation drift: [finding] | Blended expense vs. passive: [X% vs. X%]

[disclosure block]
```

---

## Fund House Watch — `fund-house-watch`

```markdown
# Fund House Watch — [date/time]

| AMC | Your exposure | Funds held | Flag | Finding (source, date) |
|---|---|---|---|---|

## Red/amber detail
[what happened, which held fund it touches, recommended follow-up]

[disclosure block if any view given]
```

---

## Plan Audit — `portfolio-plan-builder` (maintenance mode)

For the periodic "is my plan still the plan" pass. The interview itself
doesn't need a report — this is the findings list from auditing an
existing plan against reality.

```markdown
# Plan Audit — [date]

Plan last reviewed: [per-section dates] · Holdings as of [date/time]

## Plan vs. reality
| Item | Plan | Actual | Drift | Action |
|---|---|---|---|---|

## Breached limits
[hard limits first, with the ₹/% overshoot]

## Theses to re-test
[holdings whose invalidator has been hit or can no longer be checked]

## Stale sections
[section — last reviewed — what depends on it]

## Triggers fired
[watchlist triggers hit, decision-log rows due to revisit,
fixed-income rows matured, SIPs changed]

## Proposed edits
[the exact rows to change, for the user to confirm]

[disclosure block if any positioning view is given]
```

---

## Market Pulse Briefing — `market-pulse`

```markdown
# Market Pulse — [date/time]

## What matters for you today
[anything touching the user's actual holdings]

## Movers & trending
[top gainers/losers, trending names/funds]

## Macro
[budget/RBI/global note if relevant]
```
