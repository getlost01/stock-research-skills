# Setup

The install commands for Claude Code, Codex, and Cursor are in the
[README](../README.md#install). This covers everything after that.

## 1. Authenticate Groww

The plugin ships the `growwmcp` server config, so it appears on its own —
nothing to hand-edit. First use opens Groww's OAuth flow in your browser;
the token is cached afterwards, so this is one-time.

## 2. Create your plan file

In the project where you'll use this:

```bash
cp "$CLAUDE_PLUGIN_ROOT/reference/PORTFOLIO-PLAN.example.md" ./PORTFOLIO-PLAN.md
echo "PORTFOLIO-PLAN.md" >> .gitignore
```

Fill in your target allocation, risk profile, and concentration limits.

The **fixed-income inventory** and **SIP register** tables matter most:
Groww's MCP cannot see direct bonds, FDs, SGBs, or live SIP amounts, so
those tables are the only source of truth for `bond-ladder-planner`,
`rate-watch`, and `sip-review`. Those skills ask you rather than
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
      "mcp__growwmcp__modify_sip"
    ]
  }
}
```

This step is yours because a plugin cannot ship enforced permissions —
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

**Groww auth stopped working.** The OAuth token cache in
`~/.mcp-auth/mcp-remote-<version>/` is version-scoped, so a different
`mcp-remote` version looks exactly like being logged out. It's pinned to
`0.1.37` for this reason. Check `~/.mcp-auth/` for the token folder
before assuming your account needs re-authorizing.

**Skills don't appear.** Confirm the plugin is enabled (`/plugin list` in
Claude Code), then `/reload-plugins`. Plugin skills are namespaced, so
they show as `stock-research-skills:<name>`.

**A skill asks for data you expected it to have.** Direct bonds, FDs,
SGBs, and live SIP amounts aren't exposed by Groww's MCP — fill in the
relevant `PORTFOLIO-PLAN.md` tables (step 2).

**A recommendation looks thin.** Skills are required to state their
horizon, numeric basis, and specific invalidators — if one doesn't, that's
a bug worth reporting. See [How it works](how-it-works.md) for the bar
every view is held to.

## Running from source

For development, see [CONTRIBUTING.md](../CONTRIBUTING.md#running-from-source).
