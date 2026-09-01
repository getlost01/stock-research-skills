# Setup

The install commands for Claude Code, Codex, and Cursor are in the
[README](../README.md#install). This covers everything after that.

## 1. Pick and authenticate your broker(s)

The plugin ships MCP configs for four brokers — Groww (`growwmcp`),
Zerodha Kite (`kite`), INDmoney (`indmoney`), and Upstox (`upstox`) — so
they appear on their own, nothing to hand-edit. First use of each opens
that broker's OAuth flow in your browser; tokens are cached afterwards
(Upstox is the exception — its docs say the link must be re-authorized
daily).

Then tell the skills which one(s) to actually use — just ask:

```text
which brokers should I set up?
```

That writes `BROKERS.md` at your project root listing your broker(s) in
preference order (most people list exactly one), and git-ignores it.
Prefer doing it by hand? Copy `reference/BROKERS.example.md` out of the
plugin and add `BROKERS.md` to `.gitignore` yourself.

If you only use one broker, you can also delete the other entries from
your local copy of the plugin's `.mcp.json` so you're not prompted to
authenticate accounts you don't have.

Which broker covers which data — and which tools on the newer brokers
are still unverified — is in the plugin's
`reference/BROKER-CAPABILITIES.md`. Groww remains the best-tested;
Zerodha, INDmoney, and Upstox rows are transcribed from their docs and
should be confirmed live once before you lean on them. Angel One has no
official MCP server yet, so it isn't supported. That file's **Setup
docs (per broker)** section links each broker's own install/auth page
directly, plus what's been researched (but not wired) for Dhan, Fyers,
5paisa, and others.

## 2. Create your plan file

In the project where you'll use this, just ask —

```text
help me set up my portfolio plan
```

— which runs `portfolio-plan-builder`: it reads your actual holdings
first, then interviews you (targets, risk limits, rebalancing rules,
theses, fixed income, SIPs, tax) and writes the file, showing you the
change before saving. Editing it by hand is equally fine.

The **fixed-income inventory** and **SIP register** tables matter most:
no broker's MCP can see direct bonds, FDs, or SGBs, and only INDmoney
exposes live SIP data, so those tables are the source of truth for
`bond-ladder-planner`, `rate-watch`, and (unless INDmoney is active)
`sip-review`. Those skills ask you rather than
inventing an inventory, so leaving the tables blank is what makes them
seem uninformed.

## 3. Add the read-only deny list

Paste this into your project's `.claude/settings.json`:

```json
{
  "permissions": {
    "deny": [
      "mcp__growwmcp__place_order",
      "mcp__growwmcp__modify_order",
      "mcp__growwmcp__cancel_order",
      "mcp__growwmcp__place_gtt_order",
      "mcp__growwmcp__execute_order",
      "mcp__growwmcp__create_order",
      "mcp__growwmcp__place_mutualfund_order",
      "mcp__growwmcp__start_sip",
      "mcp__growwmcp__cancel_sip",
      "mcp__growwmcp__modify_sip",
      "mcp__kite__place_order",
      "mcp__kite__modify_order",
      "mcp__kite__cancel_order",
      "mcp__kite__place_gtt_order",
      "mcp__kite__modify_gtt_order",
      "mcp__kite__delete_gtt_order"
    ]
  }
}
```

INDmoney and Upstox publish no order tools at all (their servers are
read-only by design), so there's nothing to deny there today — but if
either ever grows one, add it here and to the plugin's
`reference/BROKER-CAPABILITIES.md` write-tools table immediately.

(These are the write tools published by Groww and Zerodha's servers —
the two wired brokers that have any.) This step is yours because a
plugin cannot ship enforced permissions —
Claude Code ignores `permissions` in plugin-supplied settings by design.
It's the only layer a machine enforces rather than an instruction the
model follows, so it's worth the thirty seconds. Verify with
`/permissions`.

See [Read-only boundary](read-only.md) for what the other layers do and
don't guarantee.

## Updating

**Claude Code**

```text
/plugin marketplace update stock-research-skills
```

**Codex**

```bash
codex plugin marketplace upgrade stock-research-skills
codex plugin add stock-research-skills@stock-research-skills
```

## Troubleshooting

**Broker auth stopped working.** The OAuth token cache in
`~/.mcp-auth/mcp-remote-<version>/` is version-scoped, so a different
`mcp-remote` version looks exactly like being logged out — on every
broker at once. It's pinned to `0.1.38` for this reason. Check
`~/.mcp-auth/` for the token folder before assuming your account needs
re-authorizing. Upstox specifically requires re-authorizing daily by
design, so a morning re-auth prompt there is normal.

**Skills use the wrong broker, or say a broker isn't active.** Skills
only use brokers listed in your `BROKERS.md` (step 1), in the order
listed — a connected-but-unlisted broker is deliberately ignored. Check
that file exists at your project root and lists what you expect.

**Skills don't appear.** Confirm the plugin is enabled (`/plugin list` in
Claude Code), then `/reload-plugins`. Plugin skills are namespaced, so
they show as `stock-research-skills:<name>`.

**A skill asks for data you expected it to have.** Direct bonds, FDs,
and SGBs aren't exposed by any broker's MCP, and mutual fund / SIP data
varies by broker (Groww exposes none; INDmoney does) — fill in the
relevant `PORTFOLIO-PLAN.md` tables (step 2), or run
`portfolio-plan-builder` to fill them in conversationally. The full
who-covers-what map is `reference/BROKER-CAPABILITIES.md`.

**A recommendation looks thin.** Skills are required to state their
horizon, numeric basis, and specific invalidators — if one doesn't, that's
a bug worth reporting. See [How it works](how-it-works.md) for the bar
every view is held to.

## Running from source

For development, see [CONTRIBUTING.md](../CONTRIBUTING.md#running-from-source).
