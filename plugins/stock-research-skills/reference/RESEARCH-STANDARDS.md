# Research Standards

Shared methodology every skill follows. Read before finalizing any
buy/hold/avoid/rebalance view.

Not a SEBI-registered Research Analyst or Investment Adviser — see
`READ-ONLY-POLICY.md`. Borrow the discipline, never claim the credential.

## Disclosure block

Append to any output carrying a buy/hold/avoid/rebalance view (briefly,
not necessarily verbatim). Skip it for pure data lookups that carry no
view.

> Analysis only, not investment advice — not from a SEBI-registered
> Research Analyst or Investment Adviser. Based on data pulled at the time
> noted above plus [fundamentals / technicals / news — name which].
> No return is assured or guaranteed. Verify independently and consider
> consulting a SEBI-registered adviser before acting.

## Recommendation completeness

Any buy/accumulate/hold/reduce/avoid/subscribe/switch view carries all
five. A verdict missing any of them is not done.

1. **View + horizon** — the verdict *and* its timeframe ("trading,
   weeks", "1–3 years", "hold to maturity"). A technical call and a
   fundamentals call rarely share one.
2. **Basis** — which pillar(s) drive it (valuation / fundamentals /
   technicals / news / portfolio fit), in numbers not adjectives:
   "P/E 18 vs. peer median 27", not "attractively valued".
3. **Key risks** — 2–3 *specific* invalidators ("close below ₹X breaks
   the setup", "margin thesis fails if crude stays above $Y", "verdict
   due [month]"). Never generic "markets carry risk" filler.
4. **Position disclosure** — whether the user already holds this or
   something materially overlapping, and how the view interacts with it.
5. **Data as-of** — timestamp, stated once near the top.

## `PORTFOLIO-PLAN.md` is the source of truth for intent

The active broker's MCP knows what the user *holds*. Only
`PORTFOLIO-PLAN.md` knows what they *intended*: target allocation, risk
limits, rebalancing rules, position theses and invalidators, the
fixed-income inventory and SIP register no MCP can see, tax context,
exclusions, decision log.

**Never invent it.** No default 60/40, no inferred risk appetite. If the
file is missing, or the needed section is blank or stale (check its
`_Last reviewed:_` stamp):

1. Say so before analysing, naming the field and what it would change.
2. Offer `portfolio-plan-builder`, or for a one-off ask just for that value.
3. If the user proceeds anyway, state the assumption explicitly — never
   present an assumed target as their plan.

Blank means undecided (asking is fair); `n/a` or "no limit" means decided
(don't re-ask). Cite plan values with their `_Last reviewed:_` date the
way market data gets an as-of time.

Only `portfolio-plan-builder` writes that file, and only with the change
shown first. Other skills may *suggest* an addition, not write it.

## Tools before the web

Where `BROKER-CAPABILITIES.md` maps a capability to a live tool, call
it. `WebSearch`/`WebFetch` covers what no broker exposes — news, bonds,
fund factsheets, US fundamentals — and is a fallback, never a substitute
for a working tool or a reason to skip one.

## Tool availability

`BROKER-CAPABILITIES.md` maps capabilities to tools per broker, with
verification status in `BROKER-RESEARCH.md`. The table below is Groww's
specifically — the most-used and best-tested broker here, so it gets its
own dead-tool log. Verified live **2026-08-21**; re-verify before
trusting it, and fix it here first when a tool's state changes.

| Tool | State | Use instead |
|---|---|---|
| `get_mutualfund_details` | Returns `"GROWWMCP doesn't support mutual fund details"` — no fund data at all | Holdings/units/cost basis/SIP amounts: the user, or `PORTFOLIO-PLAN.md`. Fund facts (expense ratio, benchmark, category returns, manager tenure, top holdings): `WebSearch` on AMFI / Value Research / Morningstar / the AMC factsheet |
| `fetch_etf_screener` | Errors on every request, including an empty filter | `curate_symbols(entity_type='etf')` to resolve, one batched `get_ltp` for price, `get_quotes_and_depth` for liquidity, `fetch_stocks_fundamental_data` where it resolves. Expense ratio/tracking error/AUM: `WebSearch` |
| `fetch_technical_screener` | Server error on every request | Build the candidate list first (`fetch_fundamentals_screener`, `fetch_market_movers_and_trending_stocks_funds`, or holdings), then read it with `get_historical_technical_indicators` — up to 10 names per call |
| `fetch_ipo_details` | Returns `{}` for every issue | `fetch_ipo_listings` already carries dates, price band, lot size, issue size. Anything deeper — RHP financials, objects of the issue, promoter/anchor detail, subscription splits — via `WebSearch`/`WebFetch` |
| `get_greeks_for_fno_symbol` | Returns `[]` for every symbol and expiry | `get_greeks_for_fno_contract`, strikes resolved via `fno_mcx_contracts_search_tool` — up to 20 contracts per call |
| `get_order_details` | Returns an un-awaited coroutine string, never order data | Nothing equivalent on Groww. Say order-level history is unavailable and work from `get_equity_portfolio_holdings` average prices and `get_my_trading_positions_today` — or use Kite's order/trade history if active |
| `calculator` | Arithmetic and `**` work; math constants (`e`, `pi`) error. `log()` is **natural** log — `log(100)` returns 4.605, not 2 | Substitute literal values for constants; use `log(x)/log(10)` for base-10 |
| `fetch_ipo_listings` | Works, but `view='all'` returns ~127K characters and overflows context | Always pass `open`, `upcoming`, or `closed` |

Everything not listed was verified working.

**When a gap touches the answer, name it.** Say which numbers came from
a broker live and which came from the web, with source and date. Never
fill a gap from training data, never present an external figure as a
live broker number, and never quietly drop a section because its tool
was broken — an unavailable input is a finding.

## Data efficiency

Pull what the answer needs, once — not everything the MCP offers.

- **Batch symbols**: LTP/quote tools take multiple symbols — one call
  for the list, never one per holding.
- **LTP vs. depth**: LTP for prices; quote+depth only where spread or
  depth matters (F&O liquidity, exit sizing).
- **Derived over raw**: prefer indicator/pattern tools (batch up to 10
  names) over re-deriving from candles. Raw candles only for what they
  don't cover (drawdowns, exact levels), interval matched to horizon —
  daily for ≤1Y, weekly for multi-year, never intraday for a positional
  question.
- **Depth only where it pays**: full detail for the subject and
  shortlisted finalists only, never a whole screened universe or every
  line of a big portfolio. Peer sets: ~5–8 names, one screener call.
- **Don't re-pull** what's in context unless staleness changes the answer.
- **In output**: report the numbers that drive the conclusion, not tool
  payloads; a table for 3+ line items; never the same figure in both
  prose and table; round sensibly (₹ to the rupee, ratios to 1 decimal,
  weights to 0.1%).

### Delegate bulk parsing to a subagent

Large mechanical inputs go to a subagent — a payload that overflows
context, a tool result written to a file, a user's `.xlsx`/`.csv`/PDF
statement, or one narrow field across a dozen funds.

- **Cheap model, run in parallel.** Extraction is transcription, not
  judgement; Haiku is usually enough. Independent extractions go in one
  message.
- **Give an output contract** — name the fields, units and row shape
  ("one row per fund: scheme, plan, folio, units, value in ₹; no
  commentary"), and cap it. Without one, prose comes back and nothing
  is saved.
- **Never delegate the verdict.** Subagents extract, count, parse. The
  buy/hold/avoid call, the completeness checklist and the disclosure
  stay with the main agent — the only one that has read
  `PORTFOLIO-PLAN.md`.
- **Read-only travels with them** — brief each subagent on
  `READ-ONLY-POLICY.md` explicitly rather than assuming.
- **A partial read is a finding** — say which portion was parsed.

## Technical analysis framework

Apply all of this whenever a skill does technical analysis — not ad hoc
indicator calls.

1. **Multi-timeframe** — daily *and* weekly at minimum; state which
   timeframe a signal comes from. A daily breakout against a weekly
   downtrend is weaker than one aligned with it.
2. **Trend** — price vs. 50/100/200-period MAs, and whether those MAs
   are themselves rising, flat, or falling.
3. **Momentum** — RSI (level, or more usefully divergence from price),
   MACD (crossover + histogram direction).
4. **Volatility** — ATR or Bollinger width; high- or low-vol regime now.
   Affects sizing logic and how much weight any single signal earns.
5. **Volume confirmation** — note whether volume confirms the move.
6. **Support/resistance & patterns** — actual price levels, not "near
   support".
7. **Synthesize, don't cherry-pick** — when signals conflict, say so and
   weight the verdict down rather than quoting the flattering indicator.

## Peer / relative-value framework

**Stocks.** Build the peer set from the same sector/industry and a
comparable market-cap band via the fundamentals-screener capability —
not one or two names picked by feel. Compare P/E, P/B, EV/EBITDA where
available, ROE, ROCE, revenue/earnings growth, debt/equity, dividend
yield. State cheap/fair/expensive both *vs. the peer set* and *vs. its
own historical range* — a stock can be cheap on one and expensive on
the other.

**Mutual funds / ETFs.** Peer set = same category/benchmark. Compare
expense ratio, tracking error (passive), alpha and rolling returns vs.
benchmark and category average (active), AUM trend, manager tenure,
overlap with the user's other holdings. Judge a passive vehicle on
tracking; judge an active fund on whether alpha earns its expense
ratio, not on raw returns. INDmoney (if active) carries real fund data;
otherwise these figures are web-sourced — label and date them, and get
the held set from the user or `PORTFOLIO-PLAN.md`.

**Bonds / debt.** No broker exposes bond data — `WebSearch`/`WebFetch`,
labelled external/best-effort. Compare YTM vs. G-Sec of similar tenor
(that spread is the risk premium actually offered), credit rating plus
any recent action or outlook change, duration, issuer/sector
concentration, and liquidity of the specific ISIN (thin liquidity hurts
the exit, not the entry).

## News

No broker MCP has a news tool — pull current news via `WebSearch`
(`WebFetch` for a specific article). Training data is stale for anything
time-sensitive.

**Mandatory:** `stock-research`, `us-stock-research`, `ipo-analysis`,
`ipo-watch`, and the final shortlist in `new-investment-screener`.
`ipo-analysis` leans hardest on the web, since `fetch_ipo_details` is
dead — its prospectus and subscription figures are *all* external.

**Optional, use judgment:** `portfolio-review` (only holdings already
flagged), `rebalancing-planner`, `fno-analysis` (event-driven — RBI,
budget, expiry), `market-pulse` (macro beyond the movers tool).

**Freshness:**
- Date-anchor every query (current month/year, "latest", "Q1 FY26") — a
  dateless query surfaces stale pages.
- Prefer the last 30 days; the last few days during results season. "No
  material news in the last N days" is a finding, not a gap.
- Never present training-data knowledge as current news.
- 1–2 targeted searches per name, not five.
- Cite outlet + date. Don't state a single-source claim as settled fact.

### Preferred sources (India; not exhaustive)

| Use | Sources |
|---|---|
| Market/company news | Economic Times Markets, LiveMint, Business Standard, Moneycontrol, Reuters India |
| Filings/announcements | NSE/BSE corporate announcements, company IR page |
| Fundamental cross-check | Screener.in, Tickertape, Trendlyne |
| Regulatory/policy | SEBI and RBI press releases, MPC statements |
| Credit ratings | CRISIL, ICRA, CARE rating rationales |
| Mutual fund data | AMFI, Value Research, Morningstar India |

Prefer primary sources (exchange filings, SEBI/RBI, the company, rating
rationale) over aggregator commentary where they disagree.
