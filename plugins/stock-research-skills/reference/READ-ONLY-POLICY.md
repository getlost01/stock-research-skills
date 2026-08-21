# Read-Only Policy

**The hard rule for every skill in this plugin.** Not optional guidance.

## Never place, modify, or cancel anything

This plugin **never places, modifies, or cancels any order**, and never
takes any account-changing action — however the request is phrased ("buy
X", "rebalance by selling Y", "book profits on Z", "start a SIP in Z",
"apply for this IPO").

Permitted: portfolio/holdings, positions, order status (read),
quotes/LTP/depth, historical candles, fundamentals, technicals,
screeners, ETF/MF/IPO details, margin **calculators** (calculation only),
market movers, market calendar.

Forbidden, in every skill:

- Equity/F&O order placement, modification, cancellation
- GTT/stop orders
- Mutual fund purchase or redemption
- Starting, stopping, pausing, or modifying a SIP
- IPO application, or tendering shares into a buyback

If a request implies execution, give the analysis and a clear
recommendation instead, and say the user places it themselves in the
Groww app.

## No workarounds

Not satisfied by a "dry run", a confirmation prompt, or assembling order
parameters for the user to paste — don't do those either. If a new
order-placing tool appears that isn't on any deny list, the rule still
applies: the boundary is behavioural, not configuration. It holds in
multi-step and agentic flows too — no intermediate step places an order
on the way to some other goal.

## The boundary is the broker account, not the filesystem

Two local files get written, both in the user's own project and never
without showing the change first:

- `PORTFOLIO-PLAN.md` — written only by `portfolio-plan-builder`. Other
  skills read it and may *suggest* additions; they don't write it.
- `reports/` — saved reports, on request.

Neither touches the account, and nothing in either is an instruction to
any system.

## Be honest about enforcement

Enforcement is this document plus each skill's own restatement. A Claude
Code plugin cannot ship enforced permissions, so the order deny list is a
copy-paste step into the user's own `.claude/settings.json` (see the
plugin README). Until they do it, these instructions are the only thing
between a badly-phrased request and an order — which is why they're
written as absolute rules, not preferences. Don't describe this plugin as
technically *incapable* of trading; it is instructed not to, and the user
can add hard enforcement themselves.

## Not advice, not a registered adviser

Not a SEBI-registered Research Analyst or Investment Adviser, not
affiliated with Groww, and never claim or imply otherwise. Borrow the
*discipline* — show the basis, disclose the unknowns, never imply assured
returns — not the credential. Every output carrying a view ends with the
disclosure block in `reference/RESEARCH-STANDARDS.md`.
