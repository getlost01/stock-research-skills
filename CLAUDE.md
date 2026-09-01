# Stock Research Skills — repo instructions

This repository **is** a plugin: 22 read-only research skills for Indian
markets (stocks, ETFs, mutual funds, bonds, F&O), distributed for Claude
Code, Codex, and Cursor. It's broker-agnostic by design: skills call
capabilities (LTP, holdings, greeks, …), not one broker's tool names, and
`.mcp.json` currently wires four broker MCP servers — Groww
(`growwmcp`), Zerodha (`kite`), INDmoney (`indmoney`), and Upstox
(`upstox`) — all official/hosted and read-only or read-only-restricted.
Angel One was researched and deliberately **not** wired: it has no
official MCP, only third-party community servers, several of which place
orders using a self-supplied API key — see
`reference/BROKER-RESEARCH.md`'s **Verification status** before
adding one.
Which broker(s) are actually active for a given user lives in their own
`BROKERS.md`; see `reference/BROKER-CAPABILITIES.md` for the
capability→tool map and how to add another broker.

All 22 skills use the capability-based pattern — a skill names the
capability and resolves it via `BROKER-CAPABILITIES.md`, citing a raw
tool name only for broker-specific caveats (usually Groww's dead tools).
Keep new and edited skills to that pattern.

## Hard rule: read-only, no execution

Never place, modify, or cancel any order, and never take any
account-changing action, regardless of how a request is phrased ("buy X",
"rebalance by selling Y", "start a SIP in Z", "apply for this IPO").
Only read tools are permitted; margin **calculators** are fine
(calculation, not execution).

The authoritative statement of this rule — including the no-workarounds
clause and what to say when a request implies execution — lives in
`plugins/stock-research-skills/reference/READ-ONLY-POLICY.md`. Read it
before touching any skill. `.claude/settings.json` denies the known
order tools while working in this repo, as a backstop.

## Layout

Everything shipped to users lives in `plugins/stock-research-skills/`,
which is the single source of truth — there is no second copy of the
skills anywhere:

```
plugins/stock-research-skills/
  .claude-plugin/plugin.json    Claude Code manifest
  .codex-plugin/plugin.json     Codex manifest
  .cursor-plugin/plugin.json    Cursor manifest
  .mcp.json                     broker MCP configs (groww/kite/indmoney/upstox)
  skills/<name>/SKILL.md        the 22 skills
  reference/
    READ-ONLY-POLICY.md         the hard rule
    RESEARCH-STANDARDS.md       shared analysis discipline
    BROKER-CAPABILITIES.md      runtime: capability→tool map, write-tool deny list
    BROKER-RESEARCH.md          contributor-only: verification status, per-broker
                                setup docs, rejected brokers, how to add one
    REPORT-TEMPLATES.md         optional output scaffolds
    PORTFOLIO-PLAN.example.md   template users copy
    BROKERS.example.md          template users copy — which broker(s) are active
```

Root `.claude-plugin/marketplace.json`, `.cursor-plugin/marketplace.json`,
and `.agents/plugins/marketplace.json` make this repo its own
marketplace for the three tools. Root `README.md`, `CONTRIBUTING.md`, and
`LICENSE` are project docs.

Skills reference their siblings by plugin-relative path
(`reference/RESEARCH-STANDARDS.md`), and reference the user's own
`PORTFOLIO-PLAN.md` and `BROKERS.md` by bare name, since those files live
in the user's project rather than in the plugin.

## Working on this repo

1. Read `reference/RESEARCH-STANDARDS.md` before changing any analysis
   behaviour — every skill inherits it, so a change there propagates to
   all 22. It carries the recommendation-completeness checklist (view +
   horizon, numeric basis, specific invalidators, position disclosure,
   data as-of), the data-efficiency rules, the technical framework, the
   peer-comparison method, and the news freshness rules.
2. Start any portfolio question from real data via the user's active
   broker MCP (see `BROKERS.md` / `reference/BROKER-CAPABILITIES.md`) —
   never guess at holdings.
3. Never invent the user's intent either. Targets, limits, theses, the
   fixed-income inventory and SIP register live in `PORTFOLIO-PLAN.md`;
   when a needed section is missing or stale, say so and offer
   `portfolio-plan-builder` (the only skill that writes that file, and
   only with the diff shown first) — never a quietly assumed default.
4. State data as of "now" and cite actual numbers pulled, not vibes.
5. Be data-efficient: batch symbol lookups, prefer precomputed
   screener/indicator tools over raw candles, go deep only on the
   subject and flagged names, and report driving numbers rather than
   tool payloads. For bulk mechanical work — an oversized payload, a
   statement export, one field across a dozen funds — delegate the
   parsing to a subagent on a cost-efficient model with an explicit
   output contract, and keep the judgement in the main agent. See
   **Delegate bulk parsing to a subagent** in `RESEARCH-STANDARDS.md`.
6. Not every tool works, on any broker. Groww's dead/broken tools are in
   the **Tool availability** table in `reference/RESEARCH-STANDARDS.md`
   (as of 2026-08-21: no mutual fund data at all, no ETF or technical
   screener, no IPO details, no order details, no per-symbol greeks).
   Zerodha's and INDmoney's tool lists in
   `reference/BROKER-CAPABILITIES.md` are **unverified** — transcribed
   from public docs, not exercised against a live account by this repo.
   Before wiring any tool into a skill, or trusting an unverified row,
   call it once yourself and confirm it returns data — and when a tool's
   state changes, fix the relevant table first, then the skills that
   cite it.
7. Adding or changing a skill? `CONTRIBUTING.md` has the conventions and
   the list of files that must be updated alongside it.
8. Never commit real financial data. `reports/`, `PORTFOLIO-PLAN.md` and
   `.portfolio/` are git-ignored; only the `.example.md` template is
   tracked. A user may keep the plan and its structured data in
   `.portfolio/`, with a pointer file at the root — follow the pointer
   rather than assuming the plan is missing.

## Not a registered adviser

This project is not affiliated with Groww and is **not** a
SEBI-registered Research Analyst or Investment Adviser. It borrows the
discipline of that world (show the basis, disclose the unknowns, never
imply assured returns), never the credential. Every output carrying a
view ends with the disclosure block in `RESEARCH-STANDARDS.md`.

## MCP setup note

`growwmcp` and `upstox` connect via `mcp-remote@0.1.38` (pinned) on
dedicated local ports (52155/52158); `kite` uses the plain
`npx mcp-remote <url>` form from Zerodha's own docs (unpinned — note the
cache caveat below applies on every version bump); `indmoney` connects
directly as `streamable-http`, no bridge. The OAuth token cache in
`~/.mcp-auth/mcp-remote-<version>/` is version-scoped, so a changed
`mcp-remote` version breaks that server's cached login and forces
re-auth. If auth breaks, check `~/.mcp-auth/` for the token
version folder before assuming an account needs re-authorizing. Note
Upstox's own quirk: its docs say the account link must be re-authorized
daily.

A user who only trades through one broker still gets a connect/auth
prompt for the others by default, since `.mcp.json` ships all four.
`BROKERS.md` (which broker to *use*) doesn't suppress that — point users
at removing the unused entries from their local `.mcp.json` if it
bothers them (see `reference/BROKERS.example.md`).
