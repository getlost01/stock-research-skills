# Research Standards

Shared methodology every skill in this repo follows. Read before
finalizing any buy/hold/avoid/rebalance view.

Not a SEBI-registered Research Analyst or Investment Adviser — see
`READ-ONLY-POLICY.md`. Borrow the discipline, never claim the credential.

## Disclosure block

Append to any output carrying a buy/hold/avoid/rebalance view (briefly,
not necessarily verbatim). Skip it for pure data lookups ("what's my LTP
on X") that carry no view.

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

Groww's MCP knows what the user *holds*. Only `PORTFOLIO-PLAN.md`, at
their project root, knows what they *intended*: target allocation, risk
limits, rebalancing rules, position theses and invalidators, the
fixed-income inventory and SIP register the MCP cannot see at all, tax
context, exclusions, decision log.

**Never invent it.** No default 60/40, no inferred risk appetite. If the
file is missing, or the needed section is blank or stale (check its
`_Last reviewed:_` stamp):

1. Say so before analysing, naming the field and what it would change.
2. Offer `portfolio-plan-builder`, or for a one-off ask just for that value.
3. If the user proceeds anyway, state the assumption explicitly in the
   output — never present an assumed target as their plan.

Blank means undecided (asking is fair); `n/a` or "no limit" means decided
(don't re-ask). Cite plan values with their `_Last reviewed:_` date the
way market data gets an as-of time.

Only `portfolio-plan-builder` writes that file, and only with the change
shown first. Other skills may *suggest* an addition — a thesis line, a
decision-log row — not write it.

## Data efficiency

Pull what the answer needs, once — not everything the MCP offers.

- **Batch symbols**: `get_ltp`/quote tools take multiple symbols — one
  call for the list, never one per holding.
- **LTP vs. depth**: `get_ltp` for prices; `get_quotes_and_depth` only
  where spread/depth matters (F&O liquidity, exit sizing).
- **Derived over raw**: prefer `get_historical_technical_indicators` /
  `fetch_technical_screener` / `get_historical_candlestick_patterns` over
  re-deriving from candles. `fetch_historical_candle_data` only for what
  they don't cover (drawdowns, exact levels), interval matched to horizon
  — daily for ≤1Y, weekly for multi-year, never intraday for a
  positional question.
- **Depth only where it pays**: full per-name detail for the subject and
  shortlisted finalists only, never a whole screened universe or every
  line of a big portfolio. Peer sets: ~5–8 names, one screener call.
- **Don't re-pull** what's in context unless staleness changes the answer.
- **In output**: report the numbers that drive the conclusion, not tool
  payloads; a table for 3+ line items; never the same figure in both
  prose and table; round sensibly (₹ to the rupee, ratios to 1 decimal,
  weights to 0.1%).

## Technical analysis framework

Apply all of this whenever a skill does technical analysis — not ad hoc
indicator calls.

1. **Multi-timeframe** — daily *and* weekly at minimum; state which
   timeframe a signal comes from. A daily breakout against a weekly
   downtrend is a weaker signal than one aligned with it.
2. **Trend** — price vs. 50/100/200-period MAs, and whether those MAs
   are themselves rising, flat, or falling.
3. **Momentum** — RSI (level, or more usefully divergence from price),
   MACD (crossover + histogram direction).
4. **Volatility** — ATR or Bollinger width; high- or low-vol regime now.
   Affects sizing logic and how much weight any single signal earns.
5. **Volume confirmation** — note whether volume confirms the move.
6. **Support/resistance & patterns** — actual price levels, not "near
   support".
7. **Synthesize, don't cherry-pick** — when signals conflict (bullish
   MACD but below the 200-DMA in a weekly downtrend), say so and weight
   the verdict down rather than quoting the flattering indicator.

## Peer / relative-value framework

**Stocks.** Build the peer set from the same sector/industry and a
comparable market-cap band via `fetch_fundamentals_screener` — not one or
two names picked by feel. Compare P/E, P/B, EV/EBITDA where available,
ROE, ROCE, revenue/earnings growth, debt/equity, dividend yield. State
cheap/fair/expensive both *vs. this peer set* and *vs. its own historical
range* — a stock can be cheap on one and expensive on the other.

**Mutual funds / ETFs.** Peer set = same category/benchmark. Compare
expense ratio, tracking error (passive), alpha/rolling returns vs.
benchmark and category average (active), AUM trend, manager tenure,
concentration/overlap with the user's other holdings. Judge a passive
vehicle on tracking; judge an active fund on whether alpha earns its
expense ratio, not on raw returns.

**Bonds / debt.** Groww's MCP exposes no bond data — use
`WebSearch`/`WebFetch` and label it clearly as external/best-effort,
distinct from live Groww numbers. Compare YTM vs. G-Sec of similar tenor
(the spread is the risk premium actually offered), credit rating plus any
recent action/outlook change, duration, issuer/sector concentration, and
liquidity of the specific ISIN (thin liquidity hurts the exit, not the
entry).

## News

Groww's MCP has no news tool — pull current news via `WebSearch`
(`WebFetch` for a specific article). Training data is stale for anything
time-sensitive.

**Mandatory:** `stock-research`, `ipo-watch`, and the final shortlist in
`new-investment-screener`.

**Optional, use judgment:** `portfolio-review` (only holdings the data
already flagged), `rebalancing-planner`, `fno-analysis` (event-driven —
RBI, budget, expiry), `market-pulse` (macro headlines beyond what
`fetch_market_movers_and_trending_stocks_funds` gives).

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
