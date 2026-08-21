# Report Templates

Optional output scaffolds. Default to a normal conversational answer —
reach for the matching template only when the user wants something more
formal/structured: explicitly asks for "a report", wants to save/share/
export it, or the output has enough line items (long holdings table, long
shortlist) that structure genuinely helps readability.

Treat every `[bracket]` as a placeholder to fill in, and drop any section
that has nothing to say rather than leaving it templated-but-empty. Every
template ends with the `RESEARCH-STANDARDS.md` disclosure block where the
report contains a recommendation — that line is not optional. Any template
with a verdict must also satisfy the **Recommendation completeness**
checklist in `RESEARCH-STANDARDS.md` (view + horizon, basis with numbers,
key risks, position disclosure, data as-of).

Keep reports tight: numbers in tables, reasoning in short prose, no
restating table figures in sentences, no filler sections. A report's
value is per line, not its length.

## Where saved reports go

When a report is worth writing to disk (the user asked to save/export it,
or wants to refer back to it later), save it under `reports/` at the repo
root — that directory is git-ignored (contains real portfolio/financial
data, never commit it) and won't exist until the first report is written,
so create it if needed.

Naming convention: `reports/<skill-slug>-<subject-slug>-<YYYY-MM-DD>.md`

- `<skill-slug>`: `portfolio-review`, `stock-research`, `mutual-fund`,
  `bond`, `rebalancing`, `fno`, `tax-capital-gains`, `screener`, `ipo`,
  `market-pulse`, `earnings-watch`, `corporate-actions`,
  `dividend-income`, `bond-ladder`, `rate-watch`, `sip-review`,
  `fund-house`.
- `<subject-slug>`: the symbol/fund/topic in lowercase-kebab-case (e.g.
  `tcs`, `hdfc-flexicap`, `full-portfolio`), or omit for portfolio-wide
  reports.
- Date the report was generated, not the data's as-of time if different.

Examples: `reports/stock-research-tcs-2026-08-21.md`,
`reports/portfolio-review-full-portfolio-2026-08-21.md`,
`reports/tax-capital-gains-2026-08-21.md`.

If a same-named report already exists for today and the user asks to
regenerate it, overwrite it rather than creating a `-2`/`-v2` variant —
these are meant to reflect the latest data, not a version history.

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

## IPO Note — `ipo-watch`

```markdown
# IPO Note: [Company] — [date/time]

## Facts
Price band: [₹X-Y] | Lot size: [X] | Dates: [open-close] | Issue size: [₹X]

## Peer valuation
| Metric | [Company] (at IPO price) | Listed peer avg. |
|---|---|---|

## News
- [outlet, date] — [finding]

## View
[Subscribe (sizing + listing-gains vs. long-term intent) / Neutral / Avoid
— with reasoning]

## Key risks to this view
- [e.g. listing-day volatility, valuation stretch vs. peers, sector cycle]

[disclosure block]
```

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
