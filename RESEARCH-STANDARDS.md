# Research Standards

Shared methodology and disclosure rules every skill in this repo follows.
Referenced by each `SKILL.md` rather than repeated in full everywhere —
read this before finalizing any buy/hold/avoid/rebalance view.

## Positioning (read this first)

This agent is **not** a SEBI-registered Research Analyst or Investment
Adviser, and must never claim or imply that it is. What it *does* borrow
from that world is the **discipline**: show the analysis basis, disclose
what you don't know, never promise or imply assured/guaranteed returns
(SEBI explicitly bars registered entities from this — hold the same bar
here), and separate "here's the data and reasoning" from "here's what I'd
consider doing." Every output that gives a view on a specific instrument
ends with the disclosure block below.

### Standard disclosure block

Append this (briefly, not necessarily verbatim) to any output containing a
buy/hold/avoid/rebalance view:

> Analysis only, not investment advice — not from a SEBI-registered
> Research Analyst or Investment Adviser. Based on data pulled at the time
> noted above plus [fundamentals / technicals / news — name which].
> No return is assured or guaranteed. Verify independently and consider
> consulting a SEBI-registered adviser before acting.

Skip the block for pure data lookups (e.g. "what's my current LTP on
X") that carry no view — it's for recommendations, not every response.

## Recommendation completeness (RA-grade, every view)

Any buy/accumulate/hold/reduce/avoid/subscribe/switch view must carry all
five of these — a verdict missing any of them is not done:

1. **View + horizon** — the verdict *and* the timeframe it applies to
   (e.g. "trading, weeks", "1–3 years", "hold to maturity"). A "buy"
   without a horizon is meaningless; a technical call and a fundamentals
   call rarely share one.
2. **Basis** — which pillar(s) drive the call (valuation / fundamentals /
   technicals / news / portfolio fit), with the actual numbers, not
   adjectives ("P/E 18 vs. peer median 27", not "attractively valued").
3. **Key risks to the view** — 2–3 *specific* things that would invalidate
   it ("close below ₹X breaks the setup", "margin thesis fails if crude
   stays above $Y", "regulatory case verdict due [month]"). Never generic
   "markets carry risk" filler.
4. **Position disclosure** — state whether the user already holds the
   instrument or something materially overlapping, and how the view
   interacts with that position. This is the analogue of a registered
   analyst's conflict-of-interest disclosure.
5. **Data as-of** — timestamp of the pulled data, stated once near the top,
   not repeated per section.

## Data efficiency (keep tool calls and output proportional)

Pull what the answer needs, once — not everything the MCP offers.

- **Batch symbols**: `get_ltp`/quote tools take multiple symbols — one
  call for the whole list, never one call per holding.
- **LTP vs. depth**: `get_ltp` for prices. `get_quotes_and_depth` only
  when depth/spread genuinely matters (F&O liquidity, exit sizing).
- **Derived over raw**: prefer `get_historical_technical_indicators` /
  `fetch_technical_screener` / `get_historical_candlestick_patterns`
  (pre-computed) over pulling raw candles and re-deriving. Use
  `fetch_historical_candle_data` only for what those don't cover
  (drawdowns, exact levels), and match interval to horizon — daily
  candles for ≤1Y questions, weekly for multi-year; never intraday
  candles for a positional/long-horizon question.
- **Depth only where it pays**: full per-name detail (fundamentals,
  indicators, news) for the subject and shortlisted finalists only —
  never for a whole screened universe or every line of a big portfolio.
  Peer sets: ~5–8 names, pulled via one screener call.
- **Don't re-pull** what's already in context this session unless
  staleness actually changes the answer.
- **In the output**: report the numbers that drive the conclusion, not
  tool payloads; a table for anything with 3+ line items; never state the
  same figure in both prose and table; round sensibly (₹ to the rupee,
  ratios to 1 decimal, weights to 0.1%).

## Technical analysis framework (use this, not ad hoc calls)

Apply consistently whenever a skill does technical analysis, via
`fetch_technical_screener`, `get_historical_technical_indicators`,
`get_historical_candlestick_patterns`, `fetch_historical_candle_data`:

1. **Multi-timeframe** — check daily *and* weekly (at least). A daily
   breakout against a weekly downtrend is a different (weaker) signal than
   one aligned with it. State which timeframe(s) a signal comes from.
2. **Trend** — price vs. 50/100/200-period moving averages; is it above/
   below and are the averages themselves rising, flat, or falling.
3. **Momentum** — RSI (overbought/oversold, or more usefully divergence
   from price), MACD (crossover + histogram direction).
4. **Volatility** — ATR or Bollinger Band width; is the instrument in a
   high- or low-volatility regime right now (affects position sizing logic
   and how much weight to put on any single signal).
5. **Volume confirmation** — a move without volume support is weaker;
   note when volume data is available and whether it confirms.
6. **Support/resistance & patterns** — from historical candles/pattern
   detection; state actual price levels, not just "near support."
7. **Synthesize, don't cherry-pick** — if signals conflict (e.g. bullish
   MACD crossover but below the 200-DMA in a weekly downtrend), say so
   explicitly and weight the verdict accordingly rather than quoting only
   the flattering indicator.

## Peer / relative-value framework

### Stocks
Build the peer set from the same sector/industry and a comparable
market-cap band (use `fetch_fundamentals_screener` to pull the set, not
one or two names picked by feel). Compare: P/E, P/B, EV/EBITDA where
available, ROE, ROCE, revenue/earnings growth (trailing and, if available,
forward), debt/equity, dividend yield. State whether the subject is
cheap/fair/expensive *relative to this peer set* and relative to its own
historical range — both matter, a stock can be cheap vs. history but
expensive vs. peers or vice versa.

### Mutual funds / ETFs
Peer set = same category/benchmark. Compare: expense ratio, tracking
error (index funds/ETFs), alpha/rolling returns vs. benchmark and vs.
category average (active funds), AUM trend, fund manager tenure and
consistency of process where available, portfolio concentration/overlap
with the user's other holdings. A fund that merely tracks its benchmark
closely is doing its job for a passive vehicle; judge active funds against
whether they earn their expense ratio via actual alpha, not just against
raw returns.

### Bonds / debt instruments
Groww's MCP doesn't expose bond-specific data — use `WebSearch`/`WebFetch`
for issuer/credit detail (see sources below), and treat this as clearly
external/best-effort research, distinct from the live Groww-sourced
numbers elsewhere. Compare: yield-to-maturity vs. prevailing G-Sec yield
of similar tenor (the spread is the actual risk premium being offered),
credit rating and any recent rating action/outlook change, duration
(interest-rate sensitivity), issuer sector concentration risk, and
liquidity (how actively the specific ISIN trades — thin liquidity matters
for exit, not just entry).

## News integration

Groww's MCP has no news tool — pull current news via `WebSearch`
(and `WebFetch` for a specific article) rather than relying on training
data, which will be stale for anything time-sensitive (results,
management commentary, rating actions, regulatory action, macro events).

**When to pull news (mandatory):** `stock-research`, `ipo-watch`, and the
final shortlist in `new-investment-screener` — a view on a specific name
without checking for recent news (earnings surprise, downgrade, corporate
action, regulatory issue) is incomplete.

**When it's optional (use judgment):** `portfolio-review` (only pull news
for holdings already flagged by the data as a concern, not the whole
portfolio every time), `rebalancing-planner`, `fno-analysis` (macro/
event-driven — RBI policy, budget, expiry-specific news), `market-pulse`
(headline-level macro context beyond what `fetch_market_movers_and_
trending_stocks_funds` already gives).

**Freshness rules:**
- Anchor queries in time — include the current month/year (or "latest",
  "Q1 FY26" etc.) in the search; a dateless query surfaces stale pages.
- Prefer items from the last 30 days; for results season / live events,
  the last few days. If nothing recent exists, say "no material news in
  the last N days" — that's a finding, not a gap to paper over.
- Never present training-data knowledge as current news — anything
  time-sensitive (results, ratings, prices, policy) must come from a
  live search or a live tool, or be labeled as possibly outdated.
- 1–2 targeted searches per name is usually enough (e.g. one on results/
  news, one on any specific red flag found) — don't fan out five queries
  per finalist.

**Always:** cite the outlet and date for anything news-derived, note if a
result looks stale, and don't state a news-derived claim as settled fact
if only one source carries it — say what you found and where.

### Preferred sources (India-focused; not exhaustive)

| Use | Sources |
|---|---|
| General market/company news | Economic Times Markets, LiveMint, Business Standard, Moneycontrol, Reuters India |
| Company filings/announcements | NSE/BSE corporate announcements, the company's own investor-relations page |
| Fundamental cross-check | Screener.in, Tickertape, Trendlyne |
| Regulatory/policy | SEBI press releases, RBI press releases/monetary policy statements |
| Credit ratings (bonds) | CRISIL, ICRA, CARE Ratings rating rationales |
| Mutual fund data cross-check | AMFI, Value Research, Morningstar India |

Prefer primary sources (exchange filings, SEBI/RBI, the company itself,
rating agency rationale) over aggregator commentary when the two disagree.
