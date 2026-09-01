---
name: new-investment-screener
description: Screen for new stock, ETF, or fund ideas by theme, criteria, or a gap in the portfolio. Use when the user asks to find new stocks/ETFs/funds, screen by criteria (value, growth, sector, dividend, momentum), or asks what to add. Read-only.
---

# New Investment Screener

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (peer framework, mandatory news on
finalists, disclosure) apply. Steps below name capabilities — resolve
each against `reference/BROKER-CAPABILITIES.md` for the broker(s) in
`BROKERS.md`. The fundamentals/technical screeners and market movers are
Groww-only; if Groww isn't active, say so and build the starting universe
from `WebSearch` instead of dropping the screen.

## Steps

1. **Clarify the brief** if it's vague: theme/sector, criteria
   (value/growth/quality/dividend/momentum), instrument type, and roughly
   how much they're deploying. State the criteria **numerically** before
   screening ("P/E < 20, ROE > 15%, 3Y revenue CAGR > 12%" — not
   "reasonably valued growth") so the screen and the shortlist can both be
   checked against the same bar. Don't screen blind.

2. **Check what would actually help.** Equity holdings capability (every
   active broker) plus `PORTFOLIO-PLAN.md`'s target allocation and fund
   list (Groww returns no fund data; INDmoney's holdings/net-worth
   capability may, if active — unverified) — is there a
   real underweight this screen should target, or is the user exploring?

3. **Screen.** Fundamentals-screener capability against the stated
   criteria; symbol search / market-movers capability for an open-ended
   starting universe. Technical setups (breakouts, momentum, oversold)
   can't be screened for — Groww's technical screener is down — so screen
   fundamentally or by mover list first, then read the setups off the
   technical-indicators capability over the shortlist, 10 names per call.
   ETFs: symbol search filtered to ETF, plus web-sourced fees and tracking
   error, since Groww's ETF screener errors (see **Tool availability**).

4. **Filter out bad fits.** Drop anything that would breach the plan's
   concentration limits, sits on its **exclusions** list, or that the
   **decision log** shows the user already declined. Size the shortlist
   to **deployable capital** and the preferred deployment style, not to a
   round number. Flag heavy overlap with existing holdings — the same
   exposure in a different wrapper.

5. **Rank the shortlist** (3–7 ideas) with reasoning per pick, not
   "screener said so". Pull single-name fundamentals for the finalists,
   then run each through the full multi-timeframe technical framework in
   `RESEARCH-STANDARDS.md` (trend/momentum/volatility/volume, not one
   indicator) before it earns a spot — a name that screens well
   fundamentally but is in a broken technical structure gets that noted,
   not silently dropped or silently ranked top.

6. **News on the finalists (mandatory).** One `WebSearch` per
   shortlisted name for anything that would change the pick — results
   just out, a downgrade. Finalists only, not the screened universe.

7. **Present:** shortlist table (name, the metrics matching the brief,
   one-line rationale, material news), then a short "why not others" if
   useful, then the disclosure block. These are ideas for the user to
   evaluate and place themselves. Formal version:
   `reference/templates/new-investment-screener.md` — worth it for several
   candidates, not for a two-name shortlist.
