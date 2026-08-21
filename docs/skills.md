# The skills

All 17 are read-only and trigger on intent — you don't invoke them by
name. Each one gives analysis and recommendations; you place any trade
yourself in the Groww app.

Every skill that produces a view inherits the recommendation-completeness
checklist in [How it works](how-it-works.md): a verdict must state its
horizon, its numeric basis, 2–3 specific invalidators, whether you
already hold the instrument, and the data's as-of time.

## Portfolio & allocation

| Skill | For |
|---|---|
| `portfolio-review` | Full health check — holdings, allocation, performance vs. benchmark, concentration and overlap risk flags |
| `rebalancing-planner` | Actual vs. target allocation from your plan, with concrete ₹-sized rebalancing moves |
| `tax-capital-gains` | LTCG/STCG estimates, loss-harvesting candidates, positions near the 12-month threshold |

*"review my portfolio" · "am I over-weight anywhere?" · "what are my
unrealized gains?"*

## Research & ideas

| Skill | For |
|---|---|
| `stock-research` | Deep dive on one stock/ETF — fundamentals vs. a real peer set, multi-timeframe technicals, live news, portfolio fit |
| `mutual-fund-analysis` | Fund deep dive or comparison — expense ratio vs. actual alpha, category rank, overlap with what you hold |
| `bond-analysis` | One bond/NCD/debt instrument — YTM vs. comparable G-Sec, credit rating and rationale, duration, liquidity |
| `new-investment-screener` | Screen for ideas by theme or criteria, filtered against your concentration limits and existing exposure |
| `ipo-watch` | Upcoming/ongoing IPOs priced against listed peers, with a subscribe/neutral/avoid view |

*"what do you think about HDFC Bank?" · "is my flexi-cap fund worth
holding?" · "find me some dividend ideas"*

## Ongoing ownership

What to watch about what you already own — the gap most portfolio tools
leave open.

| Skill | For |
|---|---|
| `earnings-watch` | Results calendar for your holdings, plus post-results checks: actual vs. expected, price reaction, does the thesis still hold |
| `corporate-actions` | Dividends, splits, bonuses, buybacks, rights issues — exact ex/record dates, cost-basis and tax mechanics, tender/subscribe decisions |
| `dividend-income` | Projected annual income, yield-on-cost, upcoming ex-dates, and flags where a high yield is really a falling price |
| `sip-review` | All your SIPs as a set — is each fund earning its fee, rank drift, overlap creep, continue/pause/redirect verdicts |
| `fund-house-watch` | AMC-level red flags: manager exits, SEBI action, scheme reclassification, unusual outflows |

*"who reports this month?" · "any corporate actions coming up?" ·
"should I continue my small-cap SIP?"*

## Fixed income

| Skill | For |
|---|---|
| `bond-ladder-planner` | The whole debt book — maturity ladder by year, reinvestment risk, credit mix vs. your floor, which tenor to fill next |
| `rate-watch` | RBI stance, repo path, G-Sec curve moves and CPI, translated into what they mean for your actual debt holdings |

*"what matures when?" · "is now a good time to lock rates?" · "how do
rate cuts hit my gilt fund?"*

Both read the fixed-income inventory in your `PORTFOLIO-PLAN.md` —
Groww's MCP can't see direct bonds, FDs, or SGBs.

## Market & derivatives

| Skill | For |
|---|---|
| `market-pulse` | Market briefing — movers, trending names/funds, macro and calendar context, tied back to your holdings |
| `fno-analysis` | Options/futures positions — greeks aggregated and per-position, open interest, payoff profiles, margin vs. headroom |

*"what's happening in the market?" · "what's my net delta?" · "what does
this spread pay off like?"*

## Which skill for which question

| If you're asking… | Start with |
|---|---|
| How is my portfolio doing overall? | `portfolio-review` |
| Should I buy/hold/sell this one name? | `stock-research` |
| Am I off my target allocation? | `rebalancing-planner` |
| What's coming up on things I own? | `earnings-watch`, `corporate-actions` |
| Are my recurring investments still good? | `sip-review`, `fund-house-watch` |
| What do I do with my debt allocation? | `bond-ladder-planner`, `rate-watch` |
| What will this cost me in tax? | `tax-capital-gains` |
| What should I buy that I don't own? | `new-investment-screener`, `ipo-watch` |

Skills cross-refer rather than duplicate: `portfolio-review` points at
`stock-research` for a single name, `rebalancing-planner` picks the
bucket while `stock-research` picks the instrument, and
`bond-ladder-planner` picks the tenor slot while `bond-analysis` judges
the bond.
