---
name: fund-house-watch
description: Watch for AMC-level red flags behind the user's funds — manager exits, SEBI action, AUM swings, scheme mergers. Use when the user asks about an AMC, a fund-manager change, or wants an "anything wrong at my fund houses" check. Read-only.
---

# Fund House Watch

Read-only AMC surveillance. `reference/READ-ONLY-POLICY.md` (hard rule)
and `reference/RESEARCH-STANDARDS.md` (freshness, SEBI/AMFI primary
sources, disclosure where a fund-level view results) apply.

## Steps

1. **Build the AMC list from holdings.** `get_mutualfund_details` — the
   distinct fund houses, which of the user's funds each carries, and how
   much ₹. Watch only these unless the user names another.

2. **Search per AMC.** Date-anchored `WebSearch`, one query per AMC
   ("<AMC> news fund manager SEBI <month year>"), a second only if
   something surfaces. Looking for manager exits (especially on the
   user's schemes — note the replacement's tenure); regulatory action
   (SEBI orders, show-cause notices, front-running or valuation
   investigations); scheme events (mergers, reclassification, mandate
   changes, exit-load or expense changes on held schemes); and AUM
   anomalies (sudden large outflows — institutional money leaves first).

   Silence is a finding: "nothing material found in the last N days" per
   AMC, dated. Don't manufacture concerns.

3. **Grade against exposure:**
   - **Red** (thesis-breaking — SEBI action, star-manager exit on a held
     concentrated scheme) → follow up via `mutual-fund-analysis` /
     `sip-review`, with an interim view per the completeness checklist.
   - **Amber** (manager change with a credible successor, mandate tweak)
     → note what to re-check and when.
   - **Info** (expense change, scheme rename).

4. **Present:** a per-AMC table (AMC, ₹ exposure, funds held, flag,
   finding + source/date), detail only for red and amber, then the
   disclosure block if a view was given. Cite outlet + date on every
   claim — this skill is entirely news-derived. Formal version: Fund
   House Watch in `reference/REPORT-TEMPLATES.md`.
