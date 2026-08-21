---
name: fund-house-watch
description: Watch for AMC-level red flags across the fund houses behind the user's mutual funds — fund-manager exits, SEBI action, unusual AUM swings, scheme mergers/reclassifications — the things that break a fund thesis before returns show it. Use when the user asks about an AMC, a fund-manager change, or wants a periodic "anything wrong at my fund houses" check. Read-only.
---

# Fund House Watch

Read-only AMC-level surveillance. See root `CLAUDE.md` for the hard
read-only rule and `RESEARCH-STANDARDS.md` for news freshness rules
(SEBI/AMFI primary sources preferred) and the disclosure block (applies
only when a fund-level view results).

## Steps

1. **Build the AMC list from holdings.** `get_mutualfund_details` — the
   distinct fund houses behind the user's funds, and which of the
   user's funds (and how much ₹) each one carries. Watch only these
   unless the user names another.

2. **Search per AMC** — `WebSearch`, date-anchored per the freshness
   rules, typically one query per AMC ("<AMC> news fund manager SEBI
   <month year>"), a second only if something surfaces. Looking for:
   - **Fund-manager exits/changes** — especially on the user's specific
     schemes; note tenure of the replacement.
   - **Regulatory action** — SEBI orders, show-cause notices,
     front-running or valuation-practice investigations.
   - **Scheme events** — mergers, category reclassification, mandate
     changes, exit-load/expense changes on held schemes.
   - **AUM anomalies** — sudden large outflows in a held scheme or the
     AMC broadly (institutional money leaving first is a classic early
     signal).
   Silence is a finding: "nothing material found in the last N days"
   per AMC, dated — don't manufacture concerns.

3. **Grade what's found**, tied to the user's exposure:
   - **Red** (thesis-breaking: SEBI action on the AMC, star-manager exit
     on a held concentrated scheme) → recommend a follow-up via
     `mutual-fund-analysis` / `sip-review` on the affected fund, with an
     interim view meeting the Recommendation completeness checklist.
   - **Amber** (watch: manager change with a credible successor, mandate
     tweak) → note what to re-check and when.
   - **Info** (routine: expense change, scheme rename).

4. **Present:** a per-AMC table (AMC, user's ₹ exposure, funds held,
   flag level, finding + source/date), detail only for red/amber, then
   the disclosure block if any view was given. Cite outlet + date on
   every claim — this skill is entirely news-derived. Optional: Fund
   House Watch template in `REPORT-TEMPLATES.md`, saved under `reports/`
   if wanted.
