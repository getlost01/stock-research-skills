---
name: sip-review
description: Health check across all the user's SIPs and funds as a set — expense ratio earned, category-rank drift, manager changes, overlap creep, continue/pause/redirect per SIP. Use when the user asks to review their SIPs, whether to continue one, or for a periodic MF checkup. Read-only — never changes a SIP.
---

# SIP Review

Read-only — never start, stop, pause, or modify a SIP.
`reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (MF peer framework, tool
availability, data efficiency, disclosure) apply. Where
`mutual-fund-analysis` deep-dives one fund on demand, this sweeps the
whole set on a cadence.

Groww's MCP returns no mutual fund data at all, so this skill runs
entirely on `PORTFOLIO-PLAN.md` plus the web. With no SIP register and no
fund list, there is nothing to review — say so and offer
`portfolio-plan-builder` rather than producing a hollow sweep.

## Steps

1. **Assemble the set.** Funds held, units and cost basis, plus SIP
   amounts and dates, all come from `PORTFOLIO-PLAN.md`'s holdings and
   **SIP register** — the MCP exposes neither. Empty or stale → offer
   `portfolio-plan-builder` rather than guessing. Flag any fund listed
   with no SIP row, and any SIP row with no holding. Note the register's
   `_Last reviewed:_` date as the as-of for every ₹ figure that follows,
   since none of it is live.

2. **Per-fund scorecard**, peer framework applied lightly across the set
   — full depth only for funds that flag:
   - Returns vs. benchmark and category average (1Y/3Y) — is the expense
     ratio earned with actual alpha? Tracking error instead, for index
     funds. Web-sourced (AMFI / Value Research / factsheet), one
     date-anchored search per fund at most, cited.
   - Category-rank drift: a former top-quartile fund now middling for 2+
     years is a flag; one bad year isn't.
   - One date-anchored `WebSearch` for flagged funds or on a full annual
     review — manager change, mandate change, AMC issue.
     `fund-house-watch` covers AMC-level flags.

3. **Set-level checks:** overlap creep (funds converging on the same top
   holdings — active fees paid repeatedly for one exposure); allocation
   drift (does SIP flow still match the plan's target mix, or has it
   skewed, e.g. all new money into small-cap after a hot run); blended
   expense ratio vs. a passive equivalent.

4. **Verdict per SIP:** continue / continue-but-watch / pause / redirect
   to [bucket], each per the completeness checklist. Redirect targets are
   buckets; specific fund picks route through `mutual-fund-analysis`. Any
   change is the user's action in Groww.

5. **Present:** one summary table first (fund, SIP ₹, vs. benchmark, rank
   trend, flag, verdict), then detail only for non-continue verdicts,
   then set-level flags, then the disclosure block. Formal version: SIP
   Review in `reference/REPORT-TEMPLATES.md` — worth saving for the next
   review.
