---
name: etf-tracking-quality
description: Judge an ETF or index fund on tracking difference vs. its index, expense ratio, premium/discount to iNAV, spread and depth at the intended order size, and AUM — comparing same-index alternatives. Use when the user asks which index ETF to hold, about tracking error, or whether an ETF is trading away from fair value. Read-only.
---

# ETF Tracking Quality

Read-only. `reference/READ-ONLY-POLICY.md` (hard rule) and
`reference/RESEARCH-STANDARDS.md` (the mutual fund/ETF peer framework —
judge a passive vehicle on tracking, not returns — plus data efficiency,
completeness checklist, disclosure) apply. Steps name capabilities —
resolve each against `reference/BROKER-CAPABILITIES.md`.

`stock-research` treats an ETF as an instrument with a price. This treats
it as a *wrapper*: on the same index, two ETFs hold the same thing, so
the only differences that survive are cost, tracking, and what the user
pays to get in and out. Groww's ETF screener is dead (see **Tool
availability**) and no configured broker publishes iNAV, TER, or tracking
error — expect the web for those, labelled and dated.

## Steps

1. **Pin down the index and the order size.** Which index (or commodity,
   or bond basket) the exposure is meant to be, and roughly how much the
   user intends to put in per trade and how often — a 6bps TER gap and a
   30bps spread rank differently for a monthly ₹10,000 buy than for a
   one-off ₹10 lakh. Ask if it isn't stated; **Deployable capital** in
   `PORTFOLIO-PLAN.md` often already answers it.

2. **Build the same-index candidate set** — the symbol-search capability
   filtered to ETFs, plus the web for names it misses, and the index
   funds tracking the same index. Keep it to ~5–8; more is noise, since
   the underlying is identical.

3. **Cost and tracking, web-sourced and dated** (AMC factsheet, AMFI,
   Value Research; prefer the primary source):
   - **TER** — the only figure that is certain in advance.
   - **Tracking difference** — the actual return gap vs. the index over
     1Y/3Y. This is what the user loses, and it is usually larger than
     the TER; lead with it.
   - **Tracking error** — the standard deviation of that gap, i.e. how
     *consistent* the shortfall is. Report both; they answer different
     questions and are routinely conflated.

4. **Execution quality, live.** One batched LTP call across the
   candidates, then quote + depth on the two or three finalists:
   bid-ask spread in bps, depth at the touch against the intended order
   size, and the printed price vs. iNAV (AMC or exchange page — dated).
   A persistent premium means paying above the assets; thin depth means
   the exit costs more than the entry. State the round-trip cost in bps
   next to the TER so they're comparable.

5. **Structure flags.**
   - **AUM and its trend** — a small, shrinking ETF risks wide spreads
     and eventual merger, whatever its TER says.
   - **ETF vs. index fund vs. FoF** — an index fund executes at NAV with
     no spread but no intraday exit and often a higher TER; a fund-of-fund
     adds a cost layer and different tax treatment. For small recurring
     amounts the NAV route frequently wins on total cost; say which
     applies to *this* order size rather than declaring one better.
   - Dividend/securities-lending handling where it affects the gap.

6. **Portfolio fit.** Pull equity holdings — two ETFs on the same index
   is one position with two tickers, and that's a finding, not
   diversification. Check exclusions, the target bucket, and existing
   overlap with active funds tracking the same large caps.

7. **Verdict.** Which vehicle, at what order size, with the numeric basis
   (TER + tracking difference + round-trip cost, summed into an
   all-in annual drag where possible), a horizon, and 2–3 specific
   invalidators — a TER cut by a rival, AUM falling through a threshold,
   tracking difference widening past a stated figure. Execution notes
   (limit orders near iNAV, avoid the first and last minutes) are
   analysis, not instruction: **the user places every trade themselves**,
   and this skill places none. Disclosure block. Formal version:
   `reference/templates/etf-tracking-quality.md`.

8. **Hand off.** `mutual-fund-analysis` for active funds and the
   index-fund route in depth, `gold-and-commodity` for gold/silver ETFs
   and SGBs, `new-investment-screener` for *which* exposure to take,
   `stock-research` for a thematic or single-sector ETF where the index
   itself is the bet.
