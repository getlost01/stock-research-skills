# Portfolio Plan

**This is the template.** Copy it before filling anything in:

```bash
cp PORTFOLIO-PLAN.example.md PORTFOLIO-PLAN.md
```

`PORTFOLIO-PLAN.md` is git-ignored, so your real allocation, holdings,
bonds, and SIP amounts stay on your machine. Never put real figures in
this example file.

This file is the shared memory the other 18 skills read from. Groww's MCP
knows what you hold; only this file knows what you *intended* — targets,
limits, theses, what you've already decided. Leave a section empty and
the skills that depend on it will ask you for the same thing every
session.

Filling it in by hand is fine. Faster: ask for **`portfolio-plan-builder`**
("help me set up my portfolio plan") — it reads your actual holdings
first, interviews you against them, pushes back where your answers don't
add up, and writes this file, showing the change before saving. It also
runs in **audit mode** on an existing plan: what's stale, which limits
are breached today, which theses have hit their invalidator, which
watchlist triggers fired.

Other skills only *propose* additions here (a thesis line after a deep
dive, a decision-log row when you decline a suggestion), never without
showing you the change. If you'd rather nothing ever touch this file, say
so in [Output preferences](#output-preferences).

---

## How the skills use this file

| Section | Read by | If left empty |
|---|---|---|
| [Target allocation](#target-allocation) | `rebalancing-planner`, `portfolio-review`, `new-investment-screener`, `sip-review` | No target to diff against — no over/under-weight analysis at all |
| [Risk limits](#risk-limits) | `rebalancing-planner`, `portfolio-review`, `stock-research`, `new-investment-screener`, `ipo-watch`, `fno-analysis` | Concentration breaches go unflagged; new ideas aren't filtered |
| [Rebalancing rules](#rebalancing-rules) | `rebalancing-planner`, `tax-capital-gains`, `sip-review` | Every small drift looks actionable; tax-aware sequencing impossible |
| [Position theses](#position-theses) | `portfolio-review`, `earnings-watch`, `stock-research`, `corporate-actions` | "Why do I own this?" can't be answered; nothing to invalidate |
| [Exclusions](#exclusions--constraints) | `new-investment-screener`, `ipo-watch`, `mutual-fund-analysis` | Screens keep surfacing names you've already ruled out |
| [Fixed income](#fixed-income) + [inventory](#fixed-income-inventory) | `bond-ladder-planner`, `rate-watch`, `bond-analysis` | **Hard blocker** — MCP can't see direct bonds/FDs/SGBs at all |
| [SIP register](#sip-register) | `sip-review`, `fund-house-watch`, `mutual-fund-analysis`, `portfolio-review`, `rebalancing-planner`, `dividend-income` | The MCP returns **no** fund data — without this your entire mutual fund sleeve is invisible |
| [Tax context](#tax-context) | `tax-capital-gains`, `rebalancing-planner`, `corporate-actions` | Harvesting advice is generic; can't net against your actual FY |
| [Income goal](#income-goal) | `dividend-income`, `bond-ladder-planner` | Projected income has nothing to be measured against |
| [Deployable capital](#deployable-capital) | `new-investment-screener`, `rebalancing-planner`, `ipo-watch`, `ipo-analysis`, `bond-ladder-planner` | Suggestions get sized by guesswork |
| [IPO participation](#ipo-participation) | `ipo-analysis`, `ipo-watch` | Every issue gets researched at the same depth and sized by guesswork |
| [Watchlist](#watchlist--themes) | `new-investment-screener`, `market-pulse`, `earnings-watch`, `ipo-watch` | Briefings can't tell you what you actually care about |
| [Decision log](#decision-log) | all skills | Same suggestion re-made every session after you've declined it |
| [Output preferences](#output-preferences) | all skills | Skills default to conversational answers, no saved reports |

**Fill-in order if you're short on time:** SIP register (if you hold any
funds — nothing else can see them) → Target allocation → Risk limits →
Fixed-income inventory (if you hold any direct bonds/FDs) → Rebalancing
rules → everything else as it comes up.

## Conventions (so every skill reads this the same way)

- **Money**: plain rupees — `250000`, or `2.5L` / `1.2Cr`. Label any
  other currency.
- **Dates**: `YYYY-MM-DD`. Relative dates ("next month") go stale and
  skills treat them as unreliable.
- **Symbols**: the exchange ticker as Groww shows it (`TCS`, `NIFTYBEES`);
  full scheme name for mutual funds.
- **Percentages**: of total portfolio current market value unless a row
  says otherwise.
- **Blanks vs. `n/a`**: blank means undecided (a skill may ask); `n/a` or
  `no limit` means decided (a skill stops asking).
- **Staleness**: each section carries `_Last reviewed:_`. Skills cite it
  and flag anything older than ~6 months — a quarter for tax and SIP.

---

## Target allocation

_Last reviewed: YYYY-MM-DD_

`Band` is the drift you'll tolerate before acting — see
[Rebalancing rules](#rebalancing-rules). Targets should sum to 100%.

| Bucket | Target % | Band (±) | Notes |
|---|---|---|---|
| Large-cap equity | | | |
| Mid/small-cap equity | | | |
| ETFs (index/sector/gold etc.) | | | |
| Mutual funds | | | |
| Fixed income (bonds/FDs/debt funds) | | | |
| F&O / derivatives | | | |
| Cash / liquid | | | |

<!-- Example: | Large-cap equity | 35 | 5 | core, mostly direct stocks | -->

Where a holding could sit in two buckets (an equity MF that's really
large-cap exposure), say which one you count it in — otherwise every
skill's arithmetic differs from yours.

## Risk limits

_Last reviewed: YYYY-MM-DD_

- Risk appetite: `<conservative / moderate / aggressive>`
- Investment horizon: `<e.g. 7+ years core, 1-2 years for the tactical sleeve>`
- Max single-stock weight: `<e.g. 8%>`
- Max sector weight: `<e.g. 25%>`
- Max single mutual fund / AMC weight: `<e.g. 15% per fund, 30% per AMC>`
- Max drawdown you'd hold through without changing plan: `<e.g. 30%>`
- F&O: `<e.g. hedging only, max 5% of portfolio as premium at risk, no naked shorts>`
- Leverage / margin: `<e.g. none>`

Mark a limit `(hard)` to have a breach called out as a problem rather
than a note.

## Rebalancing rules

_Last reviewed: YYYY-MM-DD_

What turns "you're 6% overweight" into an actionable recommendation, and
what keeps `rebalancing-planner` and `tax-capital-gains` from
contradicting each other.

- Review cadence: `<e.g. quarterly, plus any time a band breaks>`
- Act only when: `<e.g. a bucket is outside its band by >2%, or >₹50,000>`
- Preferred correction method (in order): `<e.g. 1) redirect new SIP money,
  2) direct fresh cash, 3) trim only if still out of band after 2 quarters>`
- Selling constraints: `<e.g. don't sell anything held <12 months unless
  the thesis broke — LTCG threshold; keep realized STCG under ₹X this FY>`
- Never touch: `<e.g. ELSS until lock-in ends 2027-04, the SGB tranche
  held for the tax-free maturity>`
- Minimum trade size worth the friction: `<e.g. ₹10,000>`

## Position theses

_Last reviewed: YYYY-MM-DD_

One line per meaningful holding: why you own it, and what would make you
stop. `portfolio-review` and `earnings-watch` check results and news
against the **invalidator** column — without it they can only tell you
the price moved. Top holdings first; no row needed for every ₹5,000
position.

| Holding | Bucket | Why held (thesis) | Invalidator | Target weight | Reviewed |
|---|---|---|---|---|---|
| | | | | | |

<!-- Example:
| TCS | Large-cap equity | steady cash return, buyback support | 2 straight quarters of falling constant-currency revenue | 6% | 2026-08-21 |
| NIFTYBEES | ETFs | core index exposure, low cost | expense ratio above a cheaper equivalent | 15% | 2026-08-21 |
-->

An invalidator must be **checkable**: a number, an event, or a date —
not "if the story changes".

## Exclusions / constraints

_Last reviewed: YYYY-MM-DD_

What screens and IPO reviews should filter out. Give the reason — it lets
a skill tell you when it no longer applies.

| Excluded | Scope | Reason |
|---|---|---|
| | | |

<!-- Example:
| tobacco, gambling | all screens | personal |
| <name> | new buys only | already 9% via two funds — overlap |
| SME IPOs | `ipo-watch`, `ipo-analysis` | liquidity |
-->

- Employer / insider-restricted names: `<list, or "none">`
- Other constraints: `<e.g. direct plans only, no closed-ended schemes,
  demat-only for bonds>`

## Fixed income

_Last reviewed: YYYY-MM-DD_

- Fixed-income target: `<% of total portfolio — should match the bucket above>`
- Purpose of this sleeve: `<e.g. 18 months of expenses + ballast>`
- Tenor preference: `<e.g. ladder 1-5Y, nothing beyond 7Y>`
- Credit-quality floor: `<e.g. AA and above only; G-Sec/SGB unlimited>`
- Liquidity floor: `<e.g. at least ₹3L reachable within 48h>`
- Reinvestment default: `<e.g. roll maturities back into the longest rung
  unless rate-watch flags otherwise>`

### Fixed-income inventory

_Last reviewed: YYYY-MM-DD_

Groww's MCP **cannot see** direct bonds, NCDs, FDs, or SGBs — this table
is the only source of truth for them, and `bond-ladder-planner` /
`rate-watch` are blind without it. Debt mutual funds come live from the
MCP; don't duplicate them here.

| Instrument | Issuer | Amount (₹) | Coupon/Rate | Payout | Maturity | Rating | Where held |
|---|---|---|---|---|---|---|---|
| | | | | | | | |

<!-- Example:
| SGB 2.5% 2031 | GoI | 200000 | 2.5 | semi-annual | 2031-07-12 | sovereign | demat |
| Bank FD | <bank> | 300000 | 7.1 | cumulative | 2027-03-04 | n/a | <bank> |
-->

`Payout` (cumulative vs. periodic) drives both the ladder view and the
income projection.

## SIP register

_Last reviewed: YYYY-MM-DD_

**Groww's MCP returns no mutual fund data at all** — not holdings, not
NAVs, not SIPs (see **Tool availability** in `RESEARCH-STANDARDS.md`).
This table is therefore the *only* record of your fund sleeve. Without
it, `sip-review` and `fund-house-watch` have nothing to work from, and
`portfolio-review` / `rebalancing-planner` can only see your direct
equity — half a portfolio reported as the whole one.

Keep units and cost so gains and weights can be computed; refresh the
value column when you review, since nothing updates it automatically.

| Fund | Units | Avg cost ₹ | Value ₹ (as of) | SIP ₹/month | SIP date | Started | Bucket | Purpose | Step-up |
|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | |

<!-- Example: | <fund name> | 412.55 | 121.40 | 62000 (2026-08-21) | 15000 | 5 | 2024-06-05 | Mutual funds | core equity | 10%/yr -->

List every fund you hold, including lump-sum-only ones (leave the SIP
columns blank) — a fund missing here is invisible to every skill. Note
paused SIPs with the date paused, otherwise a paused SIP reads as active.
Full scheme names, so a skill can search the right factsheet.

## Tax context

_Last reviewed: YYYY-MM-DD (refresh at least quarterly)_

`tax-capital-gains` produces estimates, not filing-grade numbers, and its
harvesting suggestions are only as good as this section.

- Financial year: `<e.g. FY26>`
- Tax regime / slab: `<new/old, slab>`
- Realized gains so far this FY — STCG: `<₹ or unknown>`, LTCG: `<₹ or unknown>`
- Carried-forward losses available: `<₹, and the FY they arose>`
- LTCG exemption already used this FY: `<₹ of the annual allowance>`
- Harvesting goals: `<e.g. offset a large STCG booked outside this account>`
- Lots/statement source: `<e.g. Groww capital gains statement, downloaded YYYY-MM-DD>`

Anything you know that the MCP doesn't carry (bonus/split-adjusted basis,
gifted or inherited holdings) is what makes the estimate usable.

## Income goal

_Last reviewed: YYYY-MM-DD_

- Target annual dividend + interest income: `<₹X, or "not a goal">`
- When it needs to be real: `<e.g. by 2032, not needed as cash before then>`
- Reinvest or draw: `<e.g. reinvest until 2030>`

## Deployable capital

_Last reviewed: YYYY-MM-DD_

Sizing suggestions is guesswork without this.

- Cash available to deploy now: `<₹>`
- Expected monthly surplus beyond SIPs: `<₹>`
- Emergency fund held separately: `<₹ — excluded from this portfolio? yes/no>`
- Known upcoming outflows: `<₹ and date — e.g. 400000 by 2027-01 for a down payment>`
- Preferred deployment style: `<e.g. tranche into 3 over 6 weeks, not lump sum>`

## IPO participation

_Last reviewed: YYYY-MM-DD_

How you actually play IPOs — `ipo-analysis` sizes its verdict from this,
and `ipo-watch` uses it to triage. Neither ever applies or bids.

- Do you apply to IPOs at all: `<regularly / selectively / no>`
- Sizing per issue: `<e.g. one lot always; or up to ₹X on conviction>`
- Listing day: `<flip most / flip all / hold long-term / decide per issue>`
- Which sleeve an allotment enters: `<e.g. tactical by default, core only on an explicit hold decision>`
- SME issues: `<in / excluded — and why>`
- Funded from: `<e.g. the month's surplus, never the dip reserve>`

Sizing matters more than it looks: "one lot always" and "max the retail
limit on ones I like" are different strategies, and a skill that assumes
the wrong one either under-sizes a conviction call or proposes a single
unlisted company at a fifth of the portfolio.

## Watchlist / themes

_Last reviewed: YYYY-MM-DD_

Themes you want ideas in, and names you're waiting on. The `Trigger`
column is what lets `market-pulse` and `earnings-watch` tell you when one
is actually hit.

| Name / theme | Why interested | Trigger to look again |
|---|---|---|
| | | |

<!-- Example:
| <symbol> | quality, want it cheaper | P/E under 25 or price under ₹X |
| EV / renewables | structural, underweight | any ETF with expense ratio <0.5% |
-->

## Decision log

_Append-only; newest at the bottom._

Decisions already made, including suggestions you considered and
declined. Skills read this so they don't re-pitch something you rejected,
and so a later review knows what you expected to happen.

| Date | Decision | Reasoning | Revisit |
|---|---|---|---|
| | | | |

<!-- Example:
| 2026-08-21 | declined trimming <symbol> despite 11% weight | conviction, and it's ~₹40k of LTCG | 2027-01 |
-->

## Output preferences

- Default answer style: `<conversational / formal report>`
- Save reports to `reports/` automatically: `<yes / only when I ask>`
- May skills propose edits to this file: `<yes, show me the diff / no>`
- Detail level: `<brief — verdict plus key numbers / full workings>`
- Anything to always include: `<e.g. always show the tax impact of a trim suggestion>`
