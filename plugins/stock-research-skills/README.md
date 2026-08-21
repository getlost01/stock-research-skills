# Stock Research Skills

17 read-only skills that turn Groww's MCP server into a research desk for
Indian markets — stocks, ETFs, mutual funds, bonds, and F&O.

It reads your real portfolio, pulls live market data and current news, and
gives you structured analysis. **It never places, modifies, or cancels an
order** — you place any trade yourself in the Groww app.

## Setup after installing

**1. Authenticate Groww.** The plugin ships the `growwmcp` MCP server
config, so it appears automatically. On first use, Groww's OAuth flow
opens in your browser; the token is cached afterwards.

**2. Create your plan file** in the project where you'll use this:

```bash
cp "$CLAUDE_PLUGIN_ROOT/reference/PORTFOLIO-PLAN.example.md" ./PORTFOLIO-PLAN.md
echo "PORTFOLIO-PLAN.md" >> .gitignore
```

Fill in your target allocation, risk profile, and concentration limits.
The **fixed-income inventory** and **SIP register** tables matter most:
Groww's MCP cannot see direct bonds, FDs, SGBs, or live SIP amounts, so
those tables are the only source of truth for `bond-ladder-planner`,
`rate-watch`, and `sip-review`.

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
      "mcp__growwmcp__modify_sip"
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

**Portfolio** — `portfolio-review`, `rebalancing-planner`,
`tax-capital-gains`

**Research** — `stock-research`, `mutual-fund-analysis`, `bond-analysis`,
`new-investment-screener`, `ipo-watch`

**Ongoing ownership** — `earnings-watch`, `corporate-actions`,
`dividend-income`, `sip-review`, `fund-house-watch`

**Fixed income** — `bond-ladder-planner`, `rate-watch`

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

Not affiliated with Groww. **Not** a SEBI-registered Research Analyst or
Investment Adviser. Output comes from a language model working on data
that may be incomplete or delayed, and it can be confidently wrong. No
return is assured or guaranteed. Tax figures are rough estimates, not
filing-grade. Verify independently and consider consulting a
SEBI-registered adviser before acting.

Full terms: https://github.com/getlost01/stock-research-skills
