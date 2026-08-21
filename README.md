# Stock Research Skills

**19 read-only skills that turn Groww's MCP server into a research desk
for Indian markets** — stocks, ETFs, mutual funds, bonds, and F&O.
For [Claude Code](https://claude.com/claude-code), Codex, and Cursor.

Not a trading bot. It never places, modifies, or cancels an order. It
reads your real portfolio, pulls live market data and current news, and
gives you structured analysis — you place any trade yourself in the Groww
app.

> ⚠️ **Not investment advice.** Not affiliated with Groww, and not a
> SEBI-registered Research Analyst or Investment Adviser.
> See [the disclaimer](docs/read-only.md#disclaimer).

## Install

You'll need Node.js (for `npx`) and a Groww account.

**Claude Code**

```text
/plugin marketplace add getlost01/stock-research-skills
/plugin install stock-research-skills@stock-research-skills
```

**Codex**

```bash
codex plugin marketplace add getlost01/stock-research-skills
codex plugin add stock-research-skills@stock-research-skills
```

**Cursor** 
```js
open Cursor Settings -> find the Plugins section -> add this repo
(getlost01/stock-research-skills) as a Marketplace -> then add the Stock Research plugin.
```

**Note:** Once plugin setup, then do three setup steps — authenticate Groww, create your portfolio plan file, and
add the read-only deny list in agents setting.

**[Plugin & MCP Setup guide →](docs/install.md)**

### Then just ask

Skills trigger on intent, so there's nothing to memorise:

```
review my portfolio
what do you think about HDFC Bank at current levels?
which of my holdings report results this month?
should I continue my small-cap SIP?
what does the rate outlook mean for my debt funds?
```

## The skills

| Group | Skills |
|---|---|
| **Planning** | `portfolio-plan-builder` |
| **Portfolio** | `portfolio-review` · `rebalancing-planner` · `tax-capital-gains` |
| **Research** | `stock-research` · `mutual-fund-analysis` · `bond-analysis` · `new-investment-screener` · `ipo-analysis` · `ipo-watch` |
| **Ongoing ownership** | `earnings-watch` · `corporate-actions` · `dividend-income` · `sip-review` · `fund-house-watch` |
| **Fixed income** | `bond-ladder-planner` · `rate-watch` |
| **Market & F&O** | `market-pulse` · `fno-analysis` |

**[What each one does →](docs/skills.md)**

## Why it exists

An LLM with a brokerage connection will happily give you a confident
verdict off two data points and stale training data. Every skill here is
forced through a shared rulebook that requires a real peer set,
multi-timeframe technicals, live date-anchored news, and a recommendation
stating its horizon, its numeric basis, its specific invalidators, and
whether you already hold the thing.

The other half is coverage most portfolio tools skip: not just "should I
buy this", but what to *watch* about what you already own — earnings
dates, corporate actions, SIP drift, fund-manager exits, rate moves
against your debt book.

**[How it works →](docs/how-it-works.md)**

## Read-only

Three layers: the policy every skill reads, the boundary each skill
restates for its asset class, and a deny list you paste into your own
settings. The third is the only one a machine enforces — Claude Code
ignores `permissions` in plugin-supplied settings, by design, so that
step is yours. This project won't claim to be technically incapable of
trading when what's true is that it's instructed not to, thoroughly, and
hands you the switch for the rest.

**[Read-only boundary →](docs/read-only.md)**

## Docs

- **[Setup](docs/install.md)** — the three setup steps, updating,
  troubleshooting
- **[The skills](docs/skills.md)** — what each does, and which to reach
  for
- **[How it works](docs/how-it-works.md)** — the shared rulebook, data
  sources, repo layout
- **[Read-only boundary](docs/read-only.md)** — the rule, what enforces
  it, and the full disclaimer
- **[Contributing](CONTRIBUTING.md)** — adding a skill, the quality bar,
  testing

## Contributing

New skills, sharper analysis, and bug reports are welcome — see
[CONTRIBUTING.md](CONTRIBUTING.md). Two hard rules: **never weaken the
read-only boundary**, and **never include real portfolio data** in
issues, PRs, or examples.

## License

[MIT](LICENSE) — provided "as is", without warranty. Research and
educational use only; see [the
disclaimer](docs/read-only.md#disclaimer).
