# How it works

An LLM with a brokerage connection will happily give you a confident
verdict off two data points and stale training data. Preventing that is
most of what this project is. The mechanism is a shared rulebook every
skill inherits, so discipline doesn't depend on each skill remembering
to be careful.

## The four shared files

All four live in `plugins/stock-research-skills/reference/`, and the
skills reference them by that relative path.

### `RESEARCH-STANDARDS.md` — the rulebook

The highest-leverage file in the repo: change it and all 19 skills
change. It carries:

**Recommendation completeness.** Any buy/hold/avoid/switch view must
state all five of: the view **and its horizon** (a "buy" without a
timeframe is meaningless, and a technical call rarely shares a horizon
with a fundamentals call); the **numeric basis** ("P/E 18 vs. peer
median 27", never "attractively valued"); **2–3 specific invalidators**
(a price level, an event date, a thesis-breaker — not "markets carry
risk"); **position disclosure** (do you already hold this or something
overlapping — the analogue of a registered analyst's conflict
disclosure); and the **data's as-of time**.

**Tool availability.** Groww's MCP is a live third-party server and not
all of its tools work — as of 21 Aug 2026 it returns no mutual fund data
at all, and its ETF screener, technical screener, IPO-details and
order-details tools are broken. The table names each one, the state it's
in, and the substitute (usually `PORTFOLIO-PLAN.md` for what the user
holds, plus labelled web sources for fund and prospectus facts). The rule
around it matters more than the list: an unavailable input is a finding
to report, never a gap to fill from training data, and a web-sourced
figure never gets presented as a live Groww number.

**Delegated parsing.** Bulk mechanical work — a payload that overflows
context, a broker statement, one field pulled from a dozen factsheets —
goes to a subagent on a cost-efficient model with an explicit output
contract, so the context stays free for the analysis. Subagents extract;
they never produce the verdict, and they inherit the read-only rule in
full.

**Data efficiency.** Batch symbol lookups into one call rather than one
per holding. Prefer precomputed indicator/screener tools over pulling
raw candles and re-deriving. Match candle interval to horizon — daily
for ≤1Y questions, weekly for multi-year, never intraday for a
positional question. Go deep only on the subject and shortlisted
finalists, never the whole screened universe or every line of a large
portfolio. Don't re-pull what's already in context.

**Technical analysis.** Multi-timeframe (daily *and* weekly at minimum —
a daily breakout against a weekly downtrend is a weaker signal, and the
output must say which timeframe a signal came from), trend vs.
50/100/200-period averages, momentum (RSI divergence, MACD), volatility
regime, volume confirmation, and actual support/resistance levels rather
than "near support". Conflicting signals get stated as conflicting and
weighted — not resolved by quoting the flattering indicator.

**Peer comparison**, specialised per asset class: sector and market-cap
matched peer sets for stocks (pulled via a screener, not picked by feel);
category and benchmark for funds, judging active funds on whether they
earn their expense ratio in actual alpha and index funds on tracking
error; spread over comparable-tenor G-Sec for bonds, plus rating
rationale and duration.

**News freshness.** Date-anchored queries, preference for the last 30
days, outlet and date cited on every claim, and a hard bar on presenting
training-data knowledge as current news. "No material news in the last N
days" is a finding, not a gap to paper over.

### `PORTFOLIO-PLAN.example.md` — your context

The MCP knows what you hold. This file is the only thing that knows what
you *intended*: target allocation and drift bands, risk limits,
rebalancing rules, per-holding theses with checkable invalidators, tax
context, exclusions, deployable capital, and a decision log so a skill
doesn't re-pitch something you already declined. It opens with a table
mapping each section to the skills that read it and what breaks when it's
blank.

Two tables carry more weight than they look like they should: the
**fixed-income inventory** and the **SIP register**. Groww's MCP cannot
see direct bonds, FDs, SGBs, or live SIP amounts, so those tables are the
*only* source of truth for `bond-ladder-planner`, `rate-watch`, and
`sip-review`.

Nothing here is ever guessed at. A missing section gets named out loud,
with an offer of `portfolio-plan-builder` — which interviews you against
your real holdings and writes the file — not a default 60/40 quietly
assumed. Each section carries a `_Last reviewed:_` stamp so skills can
flag a stale target the same way they flag stale news.

Copy it to `PORTFOLIO-PLAN.md` in your own project and git-ignore it, or
just ask for a plan and let `portfolio-plan-builder` do both.

### `REPORT-TEMPLATES.md` — optional structure

One markdown scaffold per skill, for when you want a saveable report
instead of a chat answer. A conversational answer is the default
everywhere; templates are for when you asked for a report or the output
has enough line items that structure genuinely helps. Saved reports go
to `reports/` (git-ignored).

### `READ-ONLY-POLICY.md` — the hard rule

Read by every skill. See [Read-only boundary](read-only.md).

## Data sources

Live account and market data comes from **`growwmcp`**, Groww's official
MCP server at `https://mcp.groww.in/mcp`, bridged via `mcp-remote`
(pinned — the OAuth token cache is version-scoped, so bumping the version
forces a re-auth).

Groww's MCP has **no** earnings calendar, corporate-actions feed, bond
data, or news tool. Those gaps are filled by web search against Indian
primary sources — NSE/BSE announcements, SEBI and RBI releases,
rating-agency rationales, AMFI — with the business press secondary. The
skills that lean on search say so and label every finding with its
source and date, so you can tell live Groww numbers from external
research.

A dedicated search MCP isn't needed for this: what's pulled from the web
is news-shaped, which built-in web search handles well once queries are
date-anchored. The upgrade that would actually matter is *structured*
Indian-market data (an NSE, Screener.in, or Tickertape MCP), which would
replace fuzzy search results with exact feeds.

## Repo layout

```
plugins/stock-research-skills/     the plugin — single source of truth
  .claude-plugin/plugin.json       Claude Code manifest
  .codex-plugin/plugin.json        Codex manifest
  .cursor-plugin/plugin.json       Cursor manifest
  .mcp.json                        growwmcp server config
  skills/<name>/SKILL.md           the 19 skills
  reference/                       the four shared files above
.claude-plugin/marketplace.json    makes this repo its own marketplace
.cursor-plugin/marketplace.json
.agents/plugins/marketplace.json
docs/                              this documentation
```

The skills exist in exactly one place. There's no second copy for
project-local use — running from source uses `--plugin-dir` against the
same directory that gets published.

## Your data

Nothing financial is stored in or transmitted through this repo.
`reports/` and `PORTFOLIO-PLAN.md` are git-ignored (only the
`.example.md` template is tracked), Groww auth happens in your own
browser with the token cached locally in `~/.mcp-auth/`, and the plugin
itself is markdown files — there's no server, no telemetry, and nothing
that phones home.
