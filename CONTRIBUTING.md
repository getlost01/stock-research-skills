# Contributing to Stock Research Skills

Thanks for helping out. This project handles people's real money and real
brokerage accounts, so it holds two rules above everything else.

## The two hard rules

### 1. Never weaken the read-only boundary

No contribution may add, enable, or make it easier to reach any tool that
places, modifies, or cancels an order — equity, F&O, mutual fund, SIP, or
IPO application. This includes "just a dry-run mode", "only with
confirmation", or a helper that assembles order parameters for the user to
paste. A PR that touches this will be closed.

If you find a way to make the agent execute a trade, that is a security
issue — please report it privately (see [Reporting a
problem](#reporting-a-problem)) rather than opening a public PR.

The boundary lives in four places, and all four must stay in sync:
`plugins/stock-research-skills/reference/READ-ONLY-POLICY.md` (the rule),
each `SKILL.md` (restated per asset class), the deny list documented in
`plugins/stock-research-skills/README.md` (which users install
themselves, since a plugin cannot ship enforced permissions), and
`.claude/settings.json` (which protects contributors working in this
repo). If Groww ships a new order tool, adding its name to both deny
lists is a very welcome PR.

Never describe the plugin as technically unable to trade. It is
*instructed* not to, and the user installs the enforcement. Keep that
distinction honest in docs and in skill text.

### 2. Never include real financial data

Not in code, not in examples, not in issues, not in screenshots, not in
commit messages. That means no real holdings, quantities, P&L, account
IDs, or filled-in `PORTFOLIO-PLAN.md`. Use obviously fake numbers and
well-known tickers when illustrating something.

`reports/` and `PORTFOLIO-PLAN.md` are git-ignored for exactly this
reason. Don't un-ignore them, and check `git diff --staged` before you
push.

## Ways to contribute

- **New skill** — a job none of the 17 covers (see below).
- **Sharper analysis** — improvements to `RESEARCH-STANDARDS.md` are the
  highest-leverage change in the repo, since every skill inherits it.
- **Tool coverage** — Groww's MCP gains tools over time; wiring a genuinely
  useful new read-only tool into the right skill is valuable.
- **Bug reports** — especially a skill that gives a shallow, stale, or
  overconfident answer. Include the prompt and what you expected.

## Repo layout

Everything shipped to users lives in `plugins/stock-research-skills/` —
the single source of truth, with no second copy of the skills anywhere:

```
plugins/stock-research-skills/
  .claude-plugin/plugin.json    Claude Code manifest
  .codex-plugin/plugin.json     Codex manifest
  .cursor-plugin/plugin.json    Cursor manifest
  .mcp.json                     growwmcp server config
  skills/<name>/SKILL.md        the 17 skills
  reference/                    READ-ONLY-POLICY, RESEARCH-STANDARDS,
                                REPORT-TEMPLATES, PORTFOLIO-PLAN.example
```

Root `.claude-plugin/`, `.cursor-plugin/`, and `.agents/plugins/`
marketplace files make the repo installable by all three tools. Bump
`version` in all three plugin manifests together when releasing.

## Adding a skill

Skills live in `plugins/stock-research-skills/skills/<skill-name>/SKILL.md`.
Match the existing ones — read two or three first, they're short.

Reference sibling docs by plugin-relative path
(`reference/RESEARCH-STANDARDS.md`), but reference the user's plan as a
bare `PORTFOLIO-PLAN.md` — that file lives in the user's project, not in
the plugin.

**Frontmatter.** `name` (kebab-case, matching the directory) and a
`description` that says what the skill does, **when to use it** (the
trigger phrasing a user would actually type), and ends with the read-only
statement for that asset class. The description is how the skill gets
selected, so write it for routing, not for marketing.

**Body.** A short intro pointing at `CLAUDE.md` for the read-only rule and
`RESEARCH-STANDARDS.md` for the frameworks it uses, then numbered
`## Steps`. Conventions worth keeping:

1. **Pull real state first** — never let a skill assume holdings.
2. **Name the actual MCP tools** to call, in the order that makes sense.
3. **Be explicit about data efficiency** — batch lookups, precomputed
   indicators over raw candles, depth only on flagged names/finalists.
4. **Say where news is mandatory vs. optional**, and defer to the
   freshness rules rather than restating them.
5. **End with presentation guidance** — what leads, what's a table, and
   the disclosure block when a view is given.
6. **Point at sibling skills** for depth instead of duplicating them.

A new skill should also get: a row in the root README table (in the right
group), a mention in the plugin README's skill list, a `<skill-slug>`
entry and usually a template in `reference/REPORT-TEMPLATES.md`, and new
`reference/PORTFOLIO-PLAN.example.md` fields if it needs user context
Groww's MCP can't provide.

**Don't add a skill that** overlaps an existing one (extend that one
instead), needs write access to anything, or is really just a prompt
("summarize my portfolio" is `portfolio-review`).

## Analysis quality bar

Any skill producing a view must satisfy the **Recommendation
completeness** checklist in `RESEARCH-STANDARDS.md`: view **and horizon**,
numeric basis (real figures, not adjectives), 2–3 *specific*
invalidators, position disclosure, and data as-of. Generic risk filler
("markets are volatile") doesn't count.

Two things this project will not ship, on principle:

- **Implied certainty.** No price targets stated as fact, no "assured"
  or "guaranteed" returns, no rate/index predictions presented as
  knowledge. Scenario framing with both branches is the house style.
- **Fake credentials.** Nothing may claim or imply SEBI registration,
  professional advice, or a track record the agent doesn't have.

## Testing

There's no automated test suite — these are prompt artifacts, and the
data source is a live authenticated brokerage account, so behaviour can't
be asserted in CI. Verify by hand:

0. `claude plugin validate ./plugins/stock-research-skills` — catches
   manifest and structure errors before anything else.
1. Run `claude --plugin-dir ./plugins/stock-research-skills` and confirm
   your skill triggers on the phrasings in its `description` (and doesn't
   hijack another skill's).
2. Check it pulls live data rather than answering from memory, and that
   figures in the output match what the tools returned.
3. Confirm the tool calls are proportional — no per-symbol call where a
   batch works, no full-universe deep dive.
4. Confirm the disclosure block appears on views and is absent on plain
   lookups.
5. Try to make it misbehave: ask it to place a trade, and ask it
   something it has no data for. It should refuse the first and admit the
   second.

Say in your PR what you ran and what you saw. "Tested with these three
prompts, here's the shape of the output" is enough — redact any real
numbers.

## Pull requests

`main` is protected: it takes no direct pushes, and every change lands
through a pull request that needs an approving review, with review
threads resolved before merge. Force-pushes and branch deletion are
blocked outright.

- Branch off `main`, one logical change per PR.
- Keep prose wrapped to ~72–76 columns to match the existing files.
- Explain the *why* in the description, not just the what.
- Checklist before pushing: read-only boundary intact · no real financial
  data · docs updated (README / `CLAUDE.md` / templates / plan example) ·
  manual verification noted.

## Reporting a problem

Open an issue for bugs, weak analysis, or skill-routing problems —
include the prompt, the output shape, and what you expected, with real
numbers redacted.

For anything that could cause financial loss — a path to order
execution, a boundary bypass, or a skill that could leak account data —
report it privately via GitHub Security Advisories on this repo rather
than a public issue.

## License

Contributions are accepted under the [MIT License](LICENSE).
