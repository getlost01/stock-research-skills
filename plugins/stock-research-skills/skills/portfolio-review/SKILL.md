---
name: portfolio-review
description: Review the user's live brokerage portfolio — allocation, risk, per-holding buy/hold/avoid views, rebalancing. Use when the user asks to review their portfolio, check allocation or diversification, get stock/ETF/MF suggestions, or rebalance. Read-only.
---

# Portfolio Review & Recommendations

Read-only research over the user's live account(s), across whichever
broker(s) `BROKERS.md` names as active. `reference/READ-ONLY-POLICY.md`
(hard rule) and `reference/RESEARCH-STANDARDS.md` (technical and peer
frameworks for flagged holdings, data efficiency, completeness checklist,
disclosure) apply. Steps below name capabilities — resolve each against
`reference/BROKER-CAPABILITIES.md` for your active broker's tool.

## Steps

1. **Load the user's plan first.** Read `PORTFOLIO-PLAN.md` — targets and
   bands, risk limits, position theses and invalidators, exclusions, and
   the decision log, so a suggestion the user already declined isn't
   re-pitched. It's what makes this a review against *their* plan rather
   than a generic one. Missing or stale → say so and offer
   `portfolio-plan-builder`; don't assume a target.

2. **Pull current state, always — never estimate holdings.** Equity
   holdings capability, from every active broker (a user split across
   Groww and Zerodha holds different things in each — don't read only
   the first one); open F&O/intraday positions capability where
   relevant. The MF sleeve comes from `PORTFOLIO-PLAN.md` unless
   INDmoney is active (it has real MF holdings — check
   `BROKER-CAPABILITIES.md`); if neither, review the equity book and say
   plainly that the fund side is out of view.

3. **Enrich with market data**, per the data-efficiency rules: batch all
   held symbols into **one** LTP call (check the holdings payload first —
   it often carries LTP already), and pull per-name detail only for
   holdings the analysis actually flags.
   - Quote + depth only where spread/depth matters.
   - Fundamentals (single-name / screener) for valuation, growth,
     quality — flagged and major holdings first. Groww-only today.
   - Technical indicators (batch up to 10) / candlestick patterns for
     flagged names. Groww-only today.
   - Market movers for context. ETF alternatives: symbol search filtered
     to ETF + LTP, with fees and tracking error from the web — Groww's
     ETF screener and technical screener are both down (see **Tool
     availability**), and no other configured broker replaces them.
   - Historical candles for trend/drawdown checks, interval matched to
     horizon.
   - Market calendar/timing capability where timing matters.

4. **Analyze.**
   - **Allocation** — sector, market-cap, and asset-class concentration;
     single-stock risk; overlap between holdings (two funds holding the
     same top names).
   - **Performance** — P&L per holding and portfolio-wide, vs. a
     relevant benchmark (Nifty 50/500) where data allows.
   - **Quality/valuation** — flag holdings that look expensive,
     fundamentally deteriorating, or technically broken, and ones that
     look attractively valued.
   - **Thesis check** — for holdings with a thesis in the plan, test the
     invalidator against what the data now shows. A position whose reason
     for existing has lapsed is a finding price alone won't surface.
   - **Rebalancing** — current vs. the plan's targets and bands, stating
     concretely what's over/under-weight and by how much. Hand off to
     `rebalancing-planner` for sized moves.

5. **News — only for flagged holdings.** Where one stands out (large
   weight, big move, red flag), a `WebSearch` before the verdict is worth
   it. Don't news-check every line of a large portfolio; that's
   `stock-research`'s job on a specific name.

6. **Recommend, don't execute.** A clear buy/hold/reduce/avoid per
   holding with the reasoning, each meeting the completeness checklist.
   Rebalancing framed as "consider trimming X by ~N%, adding to Y" — the
   user places any trade themselves. If an ask implies placing a trade,
   give the analysis and say plainly you're not executing it.

7. **Present.** Short summary first (portfolio value, overall skew, top
   2–3 findings), then detail; tables for holdings and allocation; data
   as-of noted since markets move; close with the disclosure block.
   Formal version: `reference/templates/portfolio-review.md` — worth it
   when the holdings table is long. Point to the dedicated skill for
   depth: `stock-research` for one name,
   `mutual-fund-analysis`/`bond-analysis` per instrument type,
   `rebalancing-planner` for allocation moves, `fno-analysis` for
   derivatives, `tax-capital-gains` for gains and harvesting.
