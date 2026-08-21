---
name: rate-watch
description: Track the rate environment — RBI stance, repo path, G-Sec curve, inflation — and translate it into implications for the user's debt holdings. Use when the user asks about RBI policy, rate cuts/hikes, yields, locking rates, or how rates affect their debt. Read-only.
---

# Rate Watch

Read-only, and always tied back to what the user holds — not a generic
economics note. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (freshness, RBI primary sources,
disclosure when a positioning view is given) apply.

## Steps

1. **Rate picture now.** Date-anchored `WebSearch`, preferring primary
   sources (RBI policy statements, CPI prints) over commentary — repo
   rate, latest MPC decision and stance, next MPC date; G-Sec yields at
   1Y/5Y/10Y and the curve's move since last policy; latest CPI vs.
   RBI's band. `get_budget_announcement` in budget season, since
   borrowing numbers move yields. Two or three searches is enough.

2. **The user's exposure.** Debt funds via `get_mutualfund_details`, with
   duration category per fund (liquid/short/gilt/long) — longer duration,
   bigger NAV swing per rate move; state direction and rough sensitivity.
   Direct bonds and FDs from `PORTFOLIO-PLAN.md`'s fixed-income
   inventory — locked rates vs. today's, and what matures into this
   environment (coordinate with `bond-ladder-planner`).

3. **Translate, don't just report.** A falling-rate outlook means
   existing long-duration holdings gain while locking today's yields on
   new money gets less attractive over time, and vice versa. Frame as
   scenario reasoning, never a rate prediction stated as fact: say what
   the market and RBI signal, cite it, show both branches where
   genuinely uncertain. Flag the concrete timing questions it raises —
   "FD maturing next month into likely-lower rates", "the gilt fund now
   carries most of the duration you'd want before cuts".

4. **Present:** a short "rate picture now" block (repo, stance, 10Y, CPI,
   next MPC — sourced and dated), then per-holding implications, then any
   positioning view with the disclosure block. Briefing-length; deep
   instrument work goes to `bond-analysis` / `bond-ladder-planner`.
   Formal version: Rate Watch in `reference/REPORT-TEMPLATES.md`.
