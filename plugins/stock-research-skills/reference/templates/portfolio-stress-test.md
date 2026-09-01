# Stress Test — `portfolio-stress-test`

Fill every `[bracket]`; drop any section with nothing to say. Naming,
where saved, and the shared rules: `../REPORT-TEMPLATES.md`.

```markdown
# Stress Test — [date/time]

## Assumptions
Single-factor shocks, no probability assigned to any scenario.
Correlations rise in a real drawdown — treat every figure as a floor,
not a ceiling. Beta/drawdown method: [derived from candles, window / web-sourced,
source+date]. Max drawdown per PORTFOLIO-PLAN.md: [X%]

## Scenarios
| Scenario | Est. ₹ hit | % of portfolio | Post-shock allocation shift | Limit breached? |
|---|---|---|---|---|
| Equity −10 / −20 / −30% | | | | |
| Largest sector −25% | | | | |
| Largest position −40% | | | | |
| Rates +100bps | | | | |
| INR −10% | | | | |
| F&O stress | | | | |

## Breaches
[hard limits first, with the overshoot vs. the stated tolerance]

## Liquidity & forced-selling
Thinnest positions: [spread/depth] | Months of known outflows covered
without selling equity: [X]

## Positioning notes
[scenario-framed, both branches — never a market prediction]

[disclosure block]
```
