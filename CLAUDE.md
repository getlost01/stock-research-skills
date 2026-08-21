# Stock Research Skills — repo instructions

This repository **is** a plugin: 18 read-only research skills for Indian
markets (stocks, ETFs, mutual funds, bonds, F&O) over Groww's `growwmcp`
MCP server, distributed for Claude Code, Codex, and Cursor.

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
  .mcp.json                     growwmcp server config
  skills/<name>/SKILL.md        the 18 skills
  reference/
    READ-ONLY-POLICY.md         the hard rule
    RESEARCH-STANDARDS.md       shared analysis discipline
    REPORT-TEMPLATES.md         optional output scaffolds
    PORTFOLIO-PLAN.example.md   template users copy
```

Root `.claude-plugin/marketplace.json`, `.cursor-plugin/marketplace.json`,
and `.agents/plugins/marketplace.json` make this repo its own
marketplace for the three tools. Root `README.md`, `CONTRIBUTING.md`, and
`LICENSE` are project docs.

Skills reference their siblings by plugin-relative path
(`reference/RESEARCH-STANDARDS.md`), and reference the user's own
`PORTFOLIO-PLAN.md` by bare name, since that file lives in the user's
project rather than in the plugin.

## Working on this repo

1. Read `reference/RESEARCH-STANDARDS.md` before changing any analysis
   behaviour — every skill inherits it, so a change there propagates to
   all 18. It carries the recommendation-completeness checklist (view +
   horizon, numeric basis, specific invalidators, position disclosure,
   data as-of), the data-efficiency rules, the technical framework, the
   peer-comparison method, and the news freshness rules.
2. Start any portfolio question from real data via `growwmcp` — never
   guess at holdings.
3. Never invent the user's intent either. Targets, limits, theses, the
   fixed-income inventory and SIP register live in `PORTFOLIO-PLAN.md`;
   when a needed section is missing or stale, say so and offer
   `portfolio-plan-builder` (the only skill that writes that file, and
   only with the diff shown first) — never a quietly assumed default.
4. State data as of "now" and cite actual numbers pulled, not vibes.
5. Be data-efficient: batch symbol lookups, prefer precomputed
   screener/indicator tools over raw candles, go deep only on the
   subject and flagged names, and report driving numbers rather than
   tool payloads.
6. Adding or changing a skill? `CONTRIBUTING.md` has the conventions and
   the list of files that must be updated alongside it.
7. Never commit real financial data. `reports/` and `PORTFOLIO-PLAN.md`
   are git-ignored; only the `.example.md` template is tracked.

## Not a registered adviser

This project is not affiliated with Groww and is **not** a
SEBI-registered Research Analyst or Investment Adviser. It borrows the
discipline of that world (show the basis, disclose the unknowns, never
imply assured returns), never the credential. Every output carrying a
view ends with the disclosure block in `RESEARCH-STANDARDS.md`.

## MCP setup note

`growwmcp` connects via `mcp-remote@0.1.37`, pinned in both `.mcp.json`
files. The OAuth token cache in `~/.mcp-auth/mcp-remote-<version>/` is
version-scoped, so bumping the version breaks the cached login and
forces re-auth. If auth breaks, check `~/.mcp-auth/` for the token
version folder before assuming the account needs re-authorizing.
