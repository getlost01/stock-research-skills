---
name: sip-review
description: Periodic health check across all the user's SIPs and mutual fund holdings as a set — is each fund still earning its expense ratio, category-rank drift, manager changes, overlap creep, and continue/pause/redirect verdicts per SIP. Use when the user asks to review their SIPs, whether to continue a SIP, or for an annual/periodic MF portfolio checkup. Never starts, stops, or modifies any SIP or order.
---

# SIP Review

Read-only review of the recurring commitments as a set — where
`mutual-fund-analysis` deep-dives one fund on demand, this sweeps all of
them on a cadence. See root `CLAUDE.md` for the hard read-only rule and
`RESEARCH-STANDARDS.md` for the MF peer framework, data-efficiency rules,
and disclosure block.

## Steps

1. **Assemble the set.** `get_mutualfund_details` for all MF holdings.
   Active SIP amounts/dates come from the **SIP register** in
   `PORTFOLIO-PLAN.md` (ask the user to fill/update it if empty — the
   MCP shows holdings, not necessarily which have live SIPs).

2. **Per-fund scorecard** (peer framework from `RESEARCH-STANDARDS.md`,
   applied lightly across the set — full depth only for funds that flag):
   - Returns vs. benchmark and category average (1Y/3Y where available)
     — is the expense ratio being earned with actual alpha? For index
     funds, tracking error instead.
   - Category-rank drift: a former top-quartile fund now middling for
     2+ years is a flag, one bad year isn't.
   - `WebSearch` (one query, date-anchored) only for flagged funds or on
     a full annual review: manager change, mandate/category change, AMC
     issue — coordinate with `fund-house-watch` for AMC-level flags.

3. **Set-level checks:**
   - Overlap creep: multiple funds converging on the same top holdings —
     paying active fees several times for one exposure.
   - Allocation drift: does the SIP flow still match the target mix in
     `PORTFOLIO-PLAN.md`, or has it skewed (e.g. all new money into
     small-cap after a hot run)?
   - Cost: blended expense ratio of the set vs. a passive equivalent.

4. **Verdict per SIP**: **continue / continue-but-watch / pause / 
   redirect to [bucket]** — each meeting the Recommendation completeness
   checklist. Redirect targets are buckets; specific fund picks route
   through `mutual-fund-analysis`. Note explicitly that any SIP change
   is the user's action in the Groww app — this skill never touches one.

5. **Present:** one summary table first (fund, SIP ₹, vs. benchmark, 
   rank trend, flag, verdict), then detail only for non-"continue"
   verdicts, then set-level flags, then the disclosure block. Optional:
   SIP Review template in `REPORT-TEMPLATES.md`, saved under `reports/`
   — this skill's output is naturally worth saving for the next review.
