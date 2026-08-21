# Nivesh Desk

**17 read-only [Claude Code](https://claude.com/claude-code) skills that
turn Groww's MCP server into a research desk for Indian markets** —
stocks, ETFs, mutual funds, bonds, and F&O.

Not a trading bot. It never places, modifies, or cancels an order. It
reads your real portfolio, pulls live market data and current news, and
gives you structured analysis and recommendations — you place any trade
yourself in the Groww app.

> ⚠️ **Not investment advice.** This project is not affiliated with
> Groww, and is not a SEBI-registered Research Analyst or Investment
> Adviser. See [Disclaimer](#disclaimer).

## Why this exists

An LLM with a brokerage connection will happily give you a confident
verdict off two data points and stale training data. These skills exist
to stop that. Every skill is forced through a shared rulebook that
requires a real peer set, multi-timeframe technicals, live date-anchored
news, and a recommendation that states its horizon, its numeric basis,
its specific invalidators, and whether you already hold the thing.

## Read-only by design

Three independent layers, because one convention is not a guarantee:

1. **Instruction** — `CLAUDE.md` states the hard rule: no order-placing,
   modifying, or cancelling tool call, ever, regardless of how a request
   is phrased.
2. **Per-skill** — every `SKILL.md` restates the boundary for its own
   asset class (including "no SIP start/stop/modify" and "no IPO
   application").
3. **Enforcement** — `.claude/settings.json` denies the order tools
   outright, as a backstop rather than a promise.

Requests that imply execution ("buy 10 TCS") return analysis plus a note
that you need to place it yourself.

## Install

**Prerequisites:** [Claude Code](https://claude.com/claude-code), Node.js
(for `npx`), and a Groww account.

```bash
git clone https://github.com/getlost01/nivesh-desk.git
cd nivesh-desk
cp PORTFOLIO-PLAN.example.md PORTFOLIO-PLAN.md   # your plan; git-ignored
claude
```

On first run, Claude Code starts the `growwmcp` MCP server from
`.mcp.json` and Groww's OAuth flow opens in your browser. The token is
cached afterwards, so this is a one-time step.

Then just ask — the right skill triggers on intent:

```
review my portfolio
what do you think about HDFC Bank at current levels?
which of my holdings report results this month?
should I continue my small-cap SIP?
what does the rate outlook mean for my debt funds?
```

**Want them in your own project instead?** Copy any
`.claude/skills/<name>/` directory into your project's `.claude/skills/`,
or into `~/.claude/skills/` to use it everywhere. Bring
`RESEARCH-STANDARDS.md` along — the skills reference it by name.

## The skills

**Portfolio & allocation**

| Skill | For |
|---|---|
| `portfolio-review` | Full health check — holdings, allocation, performance, risk flags |
| `rebalancing-planner` | Actual vs. target allocation, concrete rebalancing moves |
| `tax-capital-gains` | LTCG/STCG estimates, loss/gain harvesting ideas |

**Research & ideas**

| Skill | For |
|---|---|
| `stock-research` | Deep dive on one stock/ETF — fundamentals, technicals, peers, news |
| `mutual-fund-analysis` | Fund deep dive, peer comparison, overlap check |
| `bond-analysis` | Bond/NCD/debt instrument — yield vs. G-Sec, rating, duration |
| `new-investment-screener` | Screen for new ideas by theme/criteria, checked for portfolio fit |
| `ipo-watch` | Upcoming IPOs vs. listed peers, subscribe/avoid view |

**Ongoing ownership** — what to watch about what you already own

| Skill | For |
|---|---|
| `earnings-watch` | Results calendar for your holdings + post-results thesis checks |
| `corporate-actions` | Dividends, splits, bonuses, buybacks, rights — dates and decisions |
| `dividend-income` | Projected income, yield-on-cost, payout sustainability flags |
| `sip-review` | All your SIPs as a set — continue/pause/redirect verdicts |
| `fund-house-watch` | AMC-level red flags: manager exits, SEBI action, AUM anomalies |

**Fixed income**

| Skill | For |
|---|---|
| `bond-ladder-planner` | The whole debt book — maturity ladder, reinvestment risk, credit mix |
| `rate-watch` | RBI policy and yield-curve moves, tied to your actual rate exposure |

**Market & derivatives**

| Skill | For |
|---|---|
| `market-pulse` | Market briefing, movers/trending, tied back to your holdings |
| `fno-analysis` | Options/futures positions — greeks, OI, payoff, margin |

## How it works

Four shared files do the heavy lifting, so the skills stay short and
consistent rather than each inventing its own method:

- **`RESEARCH-STANDARDS.md`** — the rulebook. A recommendation-
  completeness checklist (view **and horizon**, numeric basis, specific
  key risks, position disclosure, data as-of), data-efficiency rules
  (batch symbol lookups, prefer precomputed indicators over raw candles,
  go deep only on finalists), a structured technical-analysis framework
  (multi-timeframe, trend/momentum/volatility/volume, no cherry-picking),
  peer-comparison method per asset class, news freshness rules, and the
  standard disclosure block.
- **`PORTFOLIO-PLAN.example.md`** — your target allocation, risk profile,
  concentration limits, **fixed-income inventory**, and **SIP register**.
  The last two matter: Groww's MCP can't see direct bonds/FDs/SGBs or
  live SIP amounts, so those tables are the source of truth for
  `bond-ladder-planner`, `rate-watch`, and `sip-review`.
- **`REPORT-TEMPLATES.md`** — one optional markdown scaffold per skill,
  for when you want a formal saveable report instead of a chat answer.
- **`CLAUDE.md`** — the operating rules the agent reads on every run.

Saved reports land in `reports/` (git-ignored). Your filled-in
`PORTFOLIO-PLAN.md` is git-ignored too — only the example is tracked, so
your real financial data cannot leave your machine through this repo.

## Data sources

Live account and market data comes from **`growwmcp`**, Groww's official
MCP server at `https://mcp.groww.in/mcp`, bridged via `mcp-remote` (pinned
in `.mcp.json` — the OAuth token cache is version-scoped, so bumping the
version forces a re-auth).

Gaps are filled by web search against Indian primary sources: NSE/BSE
announcements, SEBI and RBI releases, rating-agency rationales, AMFI, and
the business press. Groww's MCP has no earnings calendar, corporate-
actions feed, bond data, or news tool, so those skills are search-driven
by design and label their sources and dates.

## Contributing

New skills, better analysis frameworks, and bug reports are welcome — see
[CONTRIBUTING.md](CONTRIBUTING.md). Two hard rules: **never weaken the
read-only boundary**, and **never include real portfolio data** in
issues, PRs, or examples.

## Disclaimer

This software is for research and educational purposes only. It is
**not** investment, financial, legal, or tax advice.

- Not affiliated with, endorsed by, or connected to Groww or any of its
  entities. "Groww" is used only to name the API this connects to.
- **Not** a SEBI-registered Research Analyst or Investment Adviser. This
  project borrows the *discipline* of that world (show your basis,
  disclose what you don't know, never imply assured returns) — not the
  credential, and it must never be represented as one.
- Output is generated by a language model from data that may be
  incomplete, delayed, or wrong. It can be confidently mistaken. Verify
  every number independently before acting on it.
- No return is assured or guaranteed. Securities investments carry market
  risk, including loss of principal.
- Tax figures are rough estimates, not filing-grade. Use Groww's official
  Capital Gains Statement and consult a qualified CA.
- You are solely responsible for your own trades. Consider consulting a
  SEBI-registered investment adviser before acting on anything here.

Provided "as is", without warranty of any kind. See [LICENSE](LICENSE).

## License

[MIT](LICENSE)
