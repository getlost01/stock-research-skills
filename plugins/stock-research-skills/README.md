# Stock Research Skills

27 read-only skills that turn your broker's MCP server into a research
desk for Indian markets — stocks, ETFs, mutual funds, bonds, and F&O.
Works with Groww, Zerodha (Kite), INDmoney, and Upstox out of the box.

It reads your real portfolio, pulls live market data and current news, and
gives you structured analysis. **It never places, modifies, or cancels an
order**, on any broker — you place any trade yourself in your broker's
app.

## Setup after installing

**1. Pick and authenticate your broker(s).** The plugin ships MCP
configs for Groww (`growwmcp`), Zerodha Kite (`kite`), INDmoney
(`indmoney`), and Upstox (`upstox`), so they appear automatically. On
first use of each, that broker's OAuth flow opens in your browser; the
token is cached afterwards (Upstox re-authorizes daily by design). Then
say which to use — just ask *"which brokers should I set up?"* and
`BROKERS.md` gets written at your project root, git-ignored, listing
your broker(s) in preference order (most people list one). Which broker
covers which data is `reference/BROKER-CAPABILITIES.md`.

**2. Create your plan file** — ask *"help me set up my portfolio plan"*
and `portfolio-plan-builder` reads your real holdings, interviews you,
and writes `PORTFOLIO-PLAN.md` (showing you the change first).

Prefer doing either by hand? Copy the matching `.example.md` out of
`reference/` and git-ignore your copy.

Fill in your target allocation, risk limits, and rebalancing rules.
The **fixed-income inventory** and **SIP register** tables matter most:
no broker's MCP can see direct bonds, FDs, or SGBs, and only INDmoney
exposes live SIP data, so those tables are the source of truth for
`bond-ladder-planner`, `rate-watch`, and (unless INDmoney is active)
`sip-review`.

**3. Add the read-only deny list** to your project's
`.claude/settings.json`. A plugin cannot ship enforced permissions, so
this step is yours:

```json
{
  "permissions": {
    "deny": [
      "mcp__growwmcp__place_order",
      "mcp__growwmcp__modify_order",
      "mcp__growwmcp__cancel_order",
      "mcp__growwmcp__place_gtt_order",
      "mcp__growwmcp__execute_order",
      "mcp__growwmcp__create_order",
      "mcp__growwmcp__place_mutualfund_order",
      "mcp__growwmcp__start_sip",
      "mcp__growwmcp__cancel_sip",
      "mcp__growwmcp__modify_sip",
      "mcp__kite__place_order",
      "mcp__kite__modify_order",
      "mcp__kite__cancel_order",
      "mcp__kite__place_gtt_order",
      "mcp__kite__modify_gtt_order",
      "mcp__kite__delete_gtt_order"
    ]
  }
}
```

The skills are instructed never to trade, and that instruction is
absolute — but an instruction is not a sandbox. This deny list is the
part a machine enforces. Add it.

## Usage

Skills trigger on intent, so just ask:

```
review my portfolio
what do you think about HDFC Bank at current levels?
which of my holdings report results this month?
should I continue my small-cap SIP?
what does the rate outlook mean for my debt funds?
am I over-weight anywhere versus my plan?
```

## The skills

**Planning** — `portfolio-plan-builder`

**Portfolio** — `portfolio-review`, `rebalancing-planner`,
`portfolio-stress-test`, `tax-capital-gains`, `trade-behavior-review`

**Research** — `stock-research`, `us-stock-research`,
`mutual-fund-analysis`, `mf-nav-attribution`, `etf-tracking-quality`,
`bond-analysis`, `new-investment-screener`, `watchlist-monitor`,
`ipo-analysis`, `ipo-watch`

**Ongoing ownership** — `thesis-audit`, `earnings-watch`,
`corporate-actions`, `dividend-income`, `sip-review`,
`fund-house-watch`

**Fixed income & gold** — `bond-ladder-planner`, `rate-watch`,
`gold-and-commodity`

**Market & derivatives** — `market-pulse`, `fno-analysis`

## How the analysis stays honest

Every skill inherits `reference/RESEARCH-STANDARDS.md`, which requires:

- **Recommendation completeness** — a view must state its horizon, its
  numeric basis (real figures, not adjectives), 2–3 *specific*
  invalidators, whether you already hold the thing, and the data's
  as-of time.
- **Data efficiency** — batch symbol lookups, prefer precomputed
  indicators over raw candles, go deep only on the subject and
  shortlisted finalists.
- **Technical discipline** — multi-timeframe, trend + momentum +
  volatility + volume, and explicit acknowledgement when signals
  conflict instead of quoting the flattering one.
- **News freshness** — date-anchored searches, last-30-days preference,
  outlet and date cited, and never presenting training data as current.

`reference/REPORT-TEMPLATES.md` holds optional scaffolds for when you
want a saveable report instead of a chat answer.

## Not investment advice

Not affiliated with Groww, Zerodha, INDmoney, Upstox, or any other
broker. **Not** a SEBI-registered Research Analyst or
Investment Adviser. Output comes from a language model working on data
that may be incomplete or delayed, and it can be confidently wrong. No
return is assured or guaranteed. Tax figures are rough estimates, not
filing-grade. Verify independently and consider consulting a
SEBI-registered adviser before acting.

Full terms: https://github.com/getlost01/stock-research-skills
