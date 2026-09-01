# The skills

All 27 are read-only and trigger on intent — you don't invoke them by
name. Each one gives analysis and recommendations; you place any trade
yourself in your broker's app.

Every skill that produces a view inherits the recommendation-completeness
checklist in [How it works](how-it-works.md): a verdict must state its
horizon, its numeric basis, 2–3 specific invalidators, whether you
already hold the instrument, and the data's as-of time.

## Planning

| Skill | For |
|---|---|
| `portfolio-plan-builder` | Interviews you — grounded in your real holdings — and writes `PORTFOLIO-PLAN.md`: targets, risk limits, rebalancing rules, position theses, fixed-income inventory, SIP register, tax context |

*"help me set up my plan" · "what should my target allocation be?" ·
"my plan is out of date"*

That file is the source of truth for everything the market data can't
tell a skill — what you *intended*. Skills read it instead of assuming;
when a section it needs is blank, they say so and offer this skill rather
than inventing a target. It's the first thing to run.

## Portfolio & allocation

| Skill | For |
|---|---|
| `portfolio-review` | Full health check — holdings, allocation, performance vs. benchmark, concentration and overlap risk flags |
| `rebalancing-planner` | Actual vs. target allocation from your plan, with concrete ₹-sized rebalancing moves |
| `tax-capital-gains` | LTCG/STCG estimates, loss-harvesting candidates, positions near the 12-month threshold |
| `portfolio-stress-test` | What a drawdown, sector crash, rate shock, currency move or margin call would actually cost you — measured against the max drawdown your plan says you'd hold through |
| `trade-behavior-review` | Trade/order history as a behavior review — win rate, holding period, churn and cost, actual behavior vs. the plan's stated horizon |

*"review my portfolio" · "am I over-weight anywhere?" · "what are my
unrealized gains?" · "what happens if the market drops 20%?"*

## Research & ideas

| Skill | For |
|---|---|
| `stock-research` | Deep dive on one stock/ETF — fundamentals vs. a real peer set, multi-timeframe technicals, live news, portfolio fit |
| `us-stock-research` | Deep dive on a US-listed stock — mostly web-sourced fundamentals/technicals (no US screener on any broker), plus currency and LRS-limit context |
| `mutual-fund-analysis` | Fund deep dive or comparison — expense ratio vs. actual alpha, category rank, overlap with what you hold |
| `mf-nav-attribution` | Estimate today's NAV move before it's published — last-disclosed holdings weighted by today's live constituent price changes |
| `etf-tracking-quality` | Which index ETF to actually hold — tracking difference vs. the index, TER, premium/discount to iNAV, spread and depth at your order size |
| `bond-analysis` | One bond/NCD/debt instrument — YTM vs. comparable G-Sec, credit rating and rationale, duration, liquidity |
| `new-investment-screener` | Screen for ideas by theme or criteria, filtered against your concentration limits and existing exposure |
| `watchlist-monitor` | Your watchlist swept against live prices — what hit its trigger, what's near, and whether the level arrived on news or on drift |
| `ipo-analysis` | One IPO researched properly — RHP financials, implied valuation vs. listed peers, issue structure, subscribe/avoid with sizing |
| `ipo-watch` | The IPO calendar — what's open, closing, or awaiting listing, and which issues merit a closer look |

*"what do you think about HDFC Bank?" · "is my flexi-cap fund worth
holding?" · "find me some dividend ideas" · "anything hit my watchlist
price?"*

## Ongoing ownership

What to watch about what you already own — the gap most portfolio tools
leave open.

| Skill | For |
|---|---|
| `thesis-audit` | Every position thesis in your plan re-tested against live data — intact, weakening, tripped, or written so vaguely it can't be checked |
| `earnings-watch` | Results calendar for your holdings, plus post-results checks: actual vs. expected, price reaction, does the thesis still hold |
| `corporate-actions` | Dividends, splits, bonuses, buybacks, rights issues — exact ex/record dates, cost-basis and tax mechanics, tender/subscribe decisions |
| `dividend-income` | Projected annual income, yield-on-cost, upcoming ex-dates, and flags where a high yield is really a falling price |
| `sip-review` | All your SIPs as a set — is each fund earning its fee, rank drift, overlap creep, continue/pause/redirect verdicts |
| `fund-house-watch` | AMC-level red flags: manager exits, SEBI action, scheme reclassification, unusual outflows |

*"who reports this month?" · "any corporate actions coming up?" ·
"should I continue my small-cap SIP?" · "why do I own this again?"*

## Fixed income & gold

| Skill | For |
|---|---|
| `bond-ladder-planner` | The whole debt book — maturity ladder by year, reinvestment risk, credit mix vs. your floor, which tenor to fill next |
| `rate-watch` | RBI stance, repo path, G-Sec curve moves and CPI, translated into what they mean for your actual debt holdings |
| `gold-and-commodity` | The gold/silver sleeve — SGB premium or discount to gold value and tenor left, ETF tracking and liquidity, sleeve size vs. target |

*"what matures when?" · "is now a good time to lock rates?" · "how do
rate cuts hit my gilt fund?" · "should I hold my SGBs to maturity?"*

Both read the fixed-income inventory in your `PORTFOLIO-PLAN.md` —
no broker's MCP can see direct bonds, FDs, or SGBs.

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
| I'm setting this up / my plan is stale | `portfolio-plan-builder` |
| How is my portfolio doing overall? | `portfolio-review` |
| Should I buy/hold/sell this one name? | `stock-research` |
| Is the reason I bought this still true? | `thesis-audit` |
| How much could I lose if things go wrong? | `portfolio-stress-test` |
| Am I off my target allocation? | `rebalancing-planner` |
| What's coming up on things I own? | `earnings-watch`, `corporate-actions` |
| Are my recurring investments still good? | `sip-review`, `fund-house-watch` |
| What do I do with my debt allocation? | `bond-ladder-planner`, `rate-watch` |
| What will this cost me in tax? | `tax-capital-gains` |
| What should I buy that I don't own? | `new-investment-screener`, `ipo-watch` |
| Did anything I'm waiting on hit my price? | `watchlist-monitor` |
| Which index ETF should I hold? | `etf-tracking-quality` |
| What about my gold / SGBs? | `gold-and-commodity` |
| Should I apply for this IPO? | `ipo-analysis` |

Skills cross-refer rather than duplicate: `portfolio-review` points at
`stock-research` for a single name, `rebalancing-planner` picks the
bucket while `stock-research` picks the instrument, and
`bond-ladder-planner` picks the tenor slot while `bond-analysis` judges
the bond. Two pairs worth knowing apart: `portfolio-plan-builder`'s audit
mode asks whether your *plan* is current, while `thesis-audit` re-tests
each thesis against the *market*; and `new-investment-screener` finds new
names, while `watchlist-monitor` only watches the ones you already chose.
