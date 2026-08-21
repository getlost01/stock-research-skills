---
name: rate-watch
description: Track the interest-rate environment — RBI policy stance, repo rate path, G-Sec yield curve moves, inflation prints — and translate it into implications for the user's actual debt funds, bonds, and any bond-buying timing. Use when the user asks about RBI policy, rate cuts/hikes, yields, "is now a good time to lock rates", or how rates affect their debt holdings. Read-only.
---

# Rate Watch

Read-only macro-rates briefing, always tied back to what the user
actually holds — not a generic economics note. See root `CLAUDE.md` for
the hard read-only rule and `RESEARCH-STANDARDS.md` for news freshness
rules (RBI primary sources preferred) and the disclosure block (applies
when a positioning view is given).

## Steps

1. **Current rate picture.** `WebSearch`, date-anchored, preferring
   primary sources (RBI monetary policy statements/press releases, CPI
   prints) over commentary:
   - Repo rate, latest MPC decision + stance, and the next MPC date.
   - Current G-Sec yields at key tenors (1Y/5Y/10Y) and how the curve
     has moved since the last policy.
   - Latest CPI vs. RBI's band — the main driver of the path.
   - `get_budget_announcement` in budget season (borrowing numbers move
     yields). 2–3 searches total is usually enough — this is a briefing.

2. **User's rate exposure.**
   - Debt funds via `get_mutualfund_details` — duration category per
     fund (liquid/short/gilt/long); longer duration = bigger NAV swing
     per rate move. State the direction and rough sensitivity.
   - Direct bonds/FDs from `PORTFOLIO-PLAN.md`'s fixed-income inventory
     — locked rates vs. today's, and what's maturing into this
     environment (coordinate with `bond-ladder-planner`).

3. **Translate, don't just report:**
   - Falling-rate outlook → existing long-duration holdings gain, and
     locking today's yields on new money gets less attractive over time
     (and vice versa). Frame as scenario reasoning, never a rate
     prediction stated as fact — say what the market/RBI signals, cite
     it, and show both branches where genuinely uncertain.
   - Flag concrete timing questions the environment raises: "FD maturing
     next month into likely-lower rates", "gilt fund now carries most of
     the duration you'd want before cuts".

4. **Present:** a short "rate picture now" block (repo, stance, 10Y,
   CPI, next MPC date — all sourced/dated), then per-holding
   implications, then any positioning view meeting the Recommendation
   completeness checklist, with the disclosure block. Keep it briefing-
   length; deep instrument work goes to `bond-analysis` /
   `bond-ladder-planner`. Optional: Rate Watch template in
   `REPORT-TEMPLATES.md`, saved under `reports/` if wanted.
