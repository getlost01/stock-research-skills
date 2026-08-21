# Read-only boundary and disclaimer

## The rule

This project never places, modifies, or cancels any order, and never
takes any account-changing action — regardless of how a request is
phrased. That covers equity and F&O orders, GTT/stop orders, mutual fund
purchase and redemption, starting/stopping/modifying a SIP, IPO
applications, and tendering shares into a buyback.

Only read tools are used: holdings, positions, order status (read),
quotes/LTP/depth, historical candles, fundamentals, technicals,
screeners, ETF/MF/IPO details, market movers, calendar, and margin
**calculators** — calculation only, never execution.

Ask it to "buy 10 TCS" and you get the analysis, a clear recommendation,
and a note that you place it yourself in the Groww app.

There are no workarounds either. A "dry run", a confirmation prompt, or
assembling order parameters for you to paste would all defeat the point,
so the policy rules those out explicitly.

## What actually enforces it

Three layers, and it's worth being precise about what each one buys you:

| Layer | What it is | Enforced by |
|---|---|---|
| 1. Policy | `reference/READ-ONLY-POLICY.md`, read by every skill | the model following instructions |
| 2. Per-skill | every `SKILL.md` restates the boundary for its asset class | the model following instructions |
| 3. Deny list | the block you paste into `.claude/settings.json` | the harness, mechanically |

**Layer 3 is the only one a machine enforces**, and a plugin cannot ship
it for you: Claude Code deliberately ignores `permissions` in
plugin-supplied settings (a plugin's `settings.json` honours only a
couple of narrow keys). That's a sensible security decision on
Anthropic's part — you shouldn't be able to install a plugin that
rewrites your permission rules — but it does mean the enforcement step is
yours. The block is in
[Setup](install.md#3-add-the-read-only-deny-list).

So: this project will not tell you it is *technically incapable* of
trading. What's true is that it is instructed not to, thoroughly and in
several places, and it hands you the switch for the rest. On something
wired to a real brokerage account, that distinction is worth stating
plainly rather than rounding up to a guarantee.

If you find a way to make it execute a trade, that's a security issue —
report it via GitHub Security Advisories on this repo rather than a
public issue.

## Disclaimer

This software is for research and educational purposes only. It is
**not** investment, financial, legal, or tax advice.

- **Not affiliated** with, endorsed by, or connected to Groww or any of
  its entities. "Groww" names the API this connects to, nothing more.
- **Not a SEBI-registered Research Analyst or Investment Adviser.** This
  project borrows the *discipline* of that world — show your basis,
  disclose what you don't know, never imply assured returns — but not the
  credential, and it must never be represented as one.
- **Output comes from a language model** working on data that may be
  incomplete, delayed, or wrong, and it can be confidently mistaken.
  Verify every number independently before acting on it.
- **No return is assured or guaranteed.** Securities investments carry
  market risk, including loss of principal.
- **Tax figures are rough estimates**, not filing-grade. Use Groww's
  official Capital Gains Statement and consult a qualified CA.
- **You are solely responsible for your own trades.** Consider consulting
  a SEBI-registered investment adviser before acting on anything here.

Provided "as is", without warranty of any kind. See
[LICENSE](../LICENSE).
