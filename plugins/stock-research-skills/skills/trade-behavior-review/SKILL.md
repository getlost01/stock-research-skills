---
name: trade-behavior-review
description: Review actual trade/order history — win rate, holding period, churn and cost, and whether real behavior matches the plan's stated horizon. Use when the user asks "how have I been trading", "am I overtrading", "what's my win rate", or wants a trade journal review. Read-only.
---

# Trade Behavior Review

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (data efficiency, delegated parsing,
disclosure) apply.

Kite is the only broker with working order/trade history (Groww's is
dead; Upstox's is doc-verified but unconfirmed live) — check
`reference/BROKER-CAPABILITIES.md`. With no such broker active, say so
rather than substituting current holdings: a holding's average price is
not a trade log.

## Steps

1. **Pull the history** via the order/trade-history capability, scoped
   to a lookback (default 6-12 months). Use the summary/history call,
   not a per-order loop. Large dumps go to a subagent per **Delegate
   bulk parsing** in `RESEARCH-STANDARDS.md`, with an output contract
   (one row per closed trade: symbol, buy date/price, sell date/price,
   qty, realized P&L) — judgement stays here.

2. **Reconstruct round trips** — match fills per symbol FIFO into
   closed trades with holding period and realized P&L. Still-open
   positions are excluded from win-rate math and noted separately.

3. **Metrics**: win rate; average win vs. average loss and the ratio;
   median *and* mean holding period (median matters more — a few
   long holds skew the mean); trade count and traded value as churn;
   best/worst names by realized P&L.

4. **Compare against stated intent.** Check median holding period
   against the horizon or style in `PORTFOLIO-PLAN.md` and name the gap
   in days/weeks. If the plan is silent, say so — don't assume one.

5. **Flag patterns with the number behind each**: repeat round trips in
   one name at a net loss; position *value* increasing after a loss;
   a few names driving most of the realized loss.

6. **A coaching view** ("median hold is 4 days against a stated 1-3 year
   horizon") takes the completeness checklist, with past-pattern-isn't-
   predictive as the honest invalidator, plus the disclosure block.
   Metrics-only sections carry no view and skip it.

7. **Present:** headline metrics table, then flagged patterns with their
   trades, then the plan comparison, then any coaching view. Formal
   version: `reference/templates/trade-behavior-review.md`. Never
   recommend placing, closing, or modifying anything.
