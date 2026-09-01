---
name: us-stock-research
description: Deep dive on a US-listed stock (e.g. AAPL, NVDA) — fundamentals, technicals, news, currency and LRS context. Use when the user asks about a US ticker or whether to buy one. Read-only.
---

# US Stock Research

Read-only deep dive on one US-listed symbol.
`reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (completeness checklist, news
freshness, disclosure) apply.

US coverage here is genuinely thinner than Indian. INDmoney's US-stock
capability is the only broker source, and no broker has a US
fundamentals screener, technical-indicator tool, or peer-set builder —
so label every figure live-vs-web, per figure. For Indian-listed names
use `stock-research` instead.

## Steps

1. **Resolve ticker and exchange** (NASDAQ/NYSE) — disambiguate a
   ticker shared with an Indian listing before anything else.

2. **Price** via INDmoney's US-stock capability if active; otherwise a
   web-sourced price with its timestamp stated, never dressed up as live.

3. **Fundamentals — peer set built by hand.** No screener exists here:
   pull recent-quarter revenue/EPS/margins plus 2-4 sector and size
   matched peers via date-anchored `WebSearch`/`WebFetch` (10-Q/10-K,
   IR releases, or an aggregator), source and date cited per figure.
   Manual is not a reason to skip the peer anchor.

4. **Technicals** — web-sourced and labelled, or explicitly declared
   unavailable. Never eyeballed from memory.

5. **News (mandatory)** — earnings, guidance, rating actions, and
   regulatory action (SEC, and FDA/FTC/DOJ where relevant).

6. **Currency and access**, the two things Indian skills don't need:
   INR/USD trend as its own line, not folded into the stock's %change;
   and the LRS limit ($250,000/person/year) as a real sizing constraint
   when discussing how much to buy.

7. **Portfolio fit** — existing US exposure via INDmoney's holdings /
   net-worth capability if it surfaces US positions, else
   `PORTFOLIO-PLAN.md` if tracked manually. If neither, ask rather than
   assuming zero exposure.

8. **View + present:** verdict up top, then fundamentals/peer table,
   technicals (or their stated absence), news, currency/LRS, fit, then
   the disclosure block — noting it carries no US regulatory standing
   either. Formal version: US Research Note in
   `reference/REPORT-TEMPLATES.md`.
