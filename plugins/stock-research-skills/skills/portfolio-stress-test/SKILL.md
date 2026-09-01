---
name: portfolio-stress-test
description: Stress the portfolio against scenarios — market drawdown, sector crash, single-stock hit, rate shock, currency move, F&O margin call — measured against the plan's risk limits. Use when the user asks what happens if the market falls, how much they could lose, or about downside, drawdown, or scenario risk. Read-only.
---

# Portfolio Stress Test

Read-only across the user's live account(s). `reference/READ-ONLY-POLICY.md`
(hard rule) and `reference/RESEARCH-STANDARDS.md` (data efficiency,
completeness checklist, disclosure) apply. Steps name capabilities —
resolve each against `reference/BROKER-CAPABILITIES.md`.

This is **sensitivity analysis, not forecasting**: it answers "how much
would this portfolio lose if X happened", never "will X happen". No
broker exposes beta or correlation, so the inputs are derived or
web-sourced — state the method with the number, every time.

## Steps

1. **Load the plan's risk side.** `PORTFOLIO-PLAN.md` — **Risk limits**
   (especially max drawdown the user would hold through, single-stock and
   sector caps, F&O and leverage rules), **Target allocation**, horizon,
   the **fixed-income inventory** (no MCP can see direct bonds/FDs/SGBs,
   and they are the ballast the whole exercise turns on), the **SIP
   register**, and **Deployable capital** — dry powder and known upcoming
   outflows decide whether a drawdown is survivable or forced.
   Missing "max drawdown" → say so: without it there's nothing to judge
   the result against, only a number.

2. **Pull live state.** Equity holdings from every active broker; open
   F&O/intraday positions and available margin where any exist — leverage
   is where a scenario stops being academic. One batched LTP call for
   valuation.

3. **Map exposure before modelling anything** — weights by sector,
   market cap, asset class, and single name, plus overlap (two funds and
   a direct holding in the same top names is one exposure). The two or
   three largest concentrations will drive every scenario, so identify
   them and keep the depth there.

4. **Derive the sensitivity inputs honestly.** Beta and correlation are
   on no broker's tool list. For the top holdings only: derive beta from
   historical candles (name vs. index, daily, ~1Y) via the candles
   capability, or take a web-sourced beta and label it. Often more
   useful than beta — actual peak-to-trough drawdown in past shocks from
   the same candle data, which needs no model. Debt-sleeve duration comes
   from the plan inventory or `bond-analysis`. Name the method and its
   window alongside every figure, and don't derive a beta for a small
   position.

5. **Run the scenario set** — these, plus any the user names:
   - Broad equity **−10% / −20% / −30%**, beta-weighted.
   - The **largest sector −25%**.
   - The **largest single position −40%** (single-stock risk is usually
     the sharpest number in the report).
   - **Rates +100bps** — duration × Δyield across the debt sleeve, plus
     the second-order hit to rate-sensitive equity.
   - **INR −10%**, only where it actually bites: US holdings, importers,
     exporters, commodity-linked names.
   - **F&O stress** — margin adequacy against available margin, and gap
     risk on any short-option leg. Flag it as the fastest path to a
     realised loss the rest of the portfolio can't cushion.

6. **For each scenario report** the estimated ₹ and % portfolio hit, the
   allocation *after* the shock (a crash rebalances the portfolio without
   asking), and whether the drawdown exceeds the plan's stated tolerance.
   That breach — not the loss figure — is the actionable finding.

7. **Two checks the scenarios miss.**
   - **Exit liquidity**: quote + depth on the largest and thinnest
     positions. A position that can't be sold in size is a bigger risk
     than its beta says.
   - **Forced-selling risk**: known outflows and expenses against cash
     and the fixed-income sleeve — months covered without selling equity
     into the fall.

8. **Present.** Assumptions box first: single-factor shocks, correlations
   rise in a real crash so treat every figure as a **floor not a
   ceiling**, and no probability is being assigned to any scenario. Then
   the scenario table, then breaches of stated limits, then positioning
   notes — scenario-framed with both branches, never a market prediction.
   Data as-of near the top; disclosure block. Formal version: Stress Test
   in `reference/REPORT-TEMPLATES.md`.

9. **Hand off** for the fix: `rebalancing-planner` to size any reduction,
   `fno-analysis` for the derivatives leg, `bond-ladder-planner` for
   ballast and `rate-watch` for the rate leg, `tax-capital-gains` before
   anything is trimmed pre-emptively.
