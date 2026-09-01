# Report Templates

Optional output scaffolds, one file per skill in `templates/`. **Read only
the one you need** — this index carries the rules that apply to all of
them, so there's never a reason to load the whole set.

Default to a normal conversational answer; reach for a template only when
the user asks for "a report", wants to save/share/export it, or the output
has enough line items that structure genuinely helps.

Fill every `[bracket]`; drop any section with nothing to say rather than
leaving it templated-but-empty. Where a report carries a recommendation it
ends with the disclosure block from `RESEARCH-STANDARDS.md` and satisfies
that file's **Recommendation completeness** checklist — neither is
optional. Keep reports tight: numbers in tables, reasoning in short
prose, no figure stated twice. A report's value is per line, not in
length.

## The templates

| Skill | Template |
|---|---|
| `portfolio-review` | [templates/portfolio-review.md](templates/portfolio-review.md) |
| `portfolio-stress-test` | [templates/portfolio-stress-test.md](templates/portfolio-stress-test.md) |
| `rebalancing-planner` | [templates/rebalancing-planner.md](templates/rebalancing-planner.md) |
| `tax-capital-gains` | [templates/tax-capital-gains.md](templates/tax-capital-gains.md) |
| `trade-behavior-review` | [templates/trade-behavior-review.md](templates/trade-behavior-review.md) |
| `thesis-audit` | [templates/thesis-audit.md](templates/thesis-audit.md) |
| `stock-research` | [templates/stock-research.md](templates/stock-research.md) |
| `us-stock-research` | [templates/us-stock-research.md](templates/us-stock-research.md) |
| `mutual-fund-analysis` | [templates/mutual-fund-analysis.md](templates/mutual-fund-analysis.md) |
| `mf-nav-attribution` | [templates/mf-nav-attribution.md](templates/mf-nav-attribution.md) |
| `etf-tracking-quality` | [templates/etf-tracking-quality.md](templates/etf-tracking-quality.md) |
| `bond-analysis` | [templates/bond-analysis.md](templates/bond-analysis.md) |
| `bond-ladder-planner` | [templates/bond-ladder-planner.md](templates/bond-ladder-planner.md) |
| `rate-watch` | [templates/rate-watch.md](templates/rate-watch.md) |
| `gold-and-commodity` | [templates/gold-and-commodity.md](templates/gold-and-commodity.md) |
| `new-investment-screener` | [templates/new-investment-screener.md](templates/new-investment-screener.md) |
| `watchlist-monitor` | [templates/watchlist-monitor.md](templates/watchlist-monitor.md) |
| `ipo-analysis` | [templates/ipo-analysis.md](templates/ipo-analysis.md) |
| `earnings-watch` | [templates/earnings-watch.md](templates/earnings-watch.md) |
| `corporate-actions` | [templates/corporate-actions.md](templates/corporate-actions.md) |
| `dividend-income` | [templates/dividend-income.md](templates/dividend-income.md) |
| `sip-review` | [templates/sip-review.md](templates/sip-review.md) |
| `fund-house-watch` | [templates/fund-house-watch.md](templates/fund-house-watch.md) |
| `fno-analysis` | [templates/fno-analysis.md](templates/fno-analysis.md) |
| `market-pulse` | [templates/market-pulse.md](templates/market-pulse.md) |
| `portfolio-plan-builder` | [templates/portfolio-plan-builder.md](templates/portfolio-plan-builder.md) — the plan-audit findings list, not the interview |

`ipo-watch` has no template: its output is the calendar table plus a
"worth a closer look" pointer, which doesn't need saving.

## Where saved reports go

`reports/<skill-slug>-<subject-slug>-<YYYY-MM-DD>.md` at the repo root —
git-ignored, holds real financial data, never commit it. Create the
directory on first use.

- `<skill-slug>`: the skill's own name, shortened where obvious
  (`mutual-fund`, `bond`, `rebalancing`, `fno`, `screener`, `ipo`,
  `bond-ladder`, `fund-house`, `plan-audit`, `thesis`, `watchlist`,
  `stress-test`, `gold`, `etf`).
- `<subject-slug>`: the symbol/fund/topic in kebab-case (`tcs`,
  `hdfc-flexicap`, `full-portfolio`); omit for portfolio-wide reports.
- Date generated, not the data's as-of time.

e.g. `reports/stock-research-tcs-2026-08-21.md`,
`reports/tax-capital-gains-2026-08-21.md`.

Regenerating the same report the same day overwrites it — these reflect
latest data, not a version history.

## Adding one

A new skill's template is a new file in `templates/`, a row above, and a
one-clause pointer in the skill itself ("Formal version:
`reference/templates/<skill>.md`"). Keep the rules in this index rather
than restating them per template.
