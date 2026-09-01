# Broker Research & Setup

Contributor/setup reference — **not loaded at skill runtime**. The
runtime half (capability→tool map, write-tool deny list) is
`BROKER-CAPABILITIES.md`; keep the two in sync when a tool changes.

Covers: which broker rows are trustworthy, where each broker's setup
docs live, which brokers were researched and rejected, and the
procedure for adding one.

## Verification status

Every non-Groww row in the capability map is transcribed from a
broker's own docs/repo, not exercised against a live account by this
repo. Call each tool once against your own account before trusting it,
then update the date here.

| Broker | Server key | Status |
|---|---|---|
| Groww | `growwmcp` | Verified live, **2026-08-21** (see `RESEARCH-STANDARDS.md`) |
| Zerodha (Kite) | `kite` | Doc-verified against the [official repo](https://github.com/zerodha/kite-mcp-server), **2026-09-01** — unchanged. Live call still pending |
| INDmoney | `indmoney` | Doc-verified, all 14 tools, against [indmoney.com/mcp](https://www.indmoney.com/mcp), **2026-09-01** — unchanged. Live call still pending |
| Upstox | `upstox` | Doc-verified against the **official** [`upstox/mcp-server-upstox-api`](https://github.com/upstox/mcp-server-upstox-api) repo, **2026-09-01** — first time real tool names were found (14 tools, all read-only). That repo's example runs locally (`localhost:8787`); not yet confirmed identical to the hosted `mcp.upstox.com/mcp` this plugin wires — live call still pending |
| Angel One | *(not wired)* | No official MCP. Community servers only (`bhavesh0009/angel-one-mcp-server`, `ameernoufil/angel-one`), several with order-placement tools on a self-supplied API key/TOTP — higher-risk trust model. Don't wire casually |
| Dhan (DhanHQ) | *(not wired)* | Official MCP exists (`docs.dhanhq.co/mcp`), researched **2026-09-01**. Real 3-step consent flow (browser 2FA → 24h JWT) — closer to OAuth trust than Angel One. Read tools (`get_holdings`, `get_positions`, `get_order_list`/`get_order_book`, `get_order_by_id`, `get_trade_book`) sit beside trade tools in one server, like Groww/Kite. **Best unwired candidate** — hosted URL and full tool list need a live connection to confirm |
| Fyers | *(not wired)* | Official MCP at `mcp.fyers.in` (SSE), researched **2026-09-01**. Auth is a bearer "FIA token" in a header, not a hosted consent screen — closer to a self-managed key than OAuth |
| 5paisa | *(not wired)* | Official MCP announced (press, June 2026); no tool list published. Public SDKs use client-held API key + TOTP. Lowest priority |
| ICICI Direct (Breeze) | *(not wired)* | No official MCP. Community servers only (`aranjan/icici-mcp`, `nicodishanthj/icici-direct-mcp-server`), order-placement-first on self-supplied key + TOTP — same risk class as Angel One |
| Tickertape | *(n/a)* | Not a broker and has no API/MCP. The only `tickertape-mcp` package is an unofficial wrapper, disclaims affiliation, and covers **US** stocks/ETFs only despite the name |

Also researched and rejected: multi-broker aggregators
(`sharuniyer/indian-broker-mcp`, `Sparker0i/indian-stock-mcp-agent` —
the latter drives broker *web apps* via Playwright automation, not an
API). Not a broker's own server, so out of scope.

A capability that turns out dead or renamed: fix the capability map
first, then any skill that cites it — same discipline as the Groww
**Tool availability** table in `RESEARCH-STANDARDS.md`.

## Setup docs (per broker)

**Wired**

- **Groww** — [setup guide](https://groww.in/updates/groww-mcp) · keys at [groww.in/trade-api/api-keys](https://groww.in/trade-api/api-keys)
- **Zerodha (Kite)** — [official repo](https://github.com/zerodha/kite-mcp-server)
- **INDmoney** — [indmoney.com/mcp](https://www.indmoney.com/mcp)
- **Upstox** — [MCP integration docs](https://upstox.com/developer/api-documentation/mcp-integration/) · [official tool repo](https://github.com/upstox/mcp-server-upstox-api)

**Researched, not wired** — reference only; none of these links is an
instruction to wire the broker in. Follow **Adding a new broker** first.

- **Dhan** — [docs.dhanhq.co/mcp](https://docs.dhanhq.co/mcp/) · keys at `web.dhan.co` → My Profile → Access DhanHQ APIs
- **Fyers** — [official MCP page](https://fyers.in/mcp) · keys at [myapi.fyers.in](https://myapi.fyers.in/dashboard/)
- **5paisa** — [5paisa MCP page](https://www.5paisa.com/technology/mcp-ai-trading-assistant) · keys at [tradestation.5paisa.com/apidoc](https://tradestation.5paisa.com/apidoc)
- **Angel One** (community, higher-risk) — [`bhavesh0009/angel-one-mcp-server`](https://github.com/bhavesh0009/angel-one-mcp-server), [`ameernoufil/angel-one`](https://github.com/ameernoufil/angel-one)
- **ICICI Direct** (community, higher-risk) — [`nicodishanthj/icici-direct-mcp-server`](https://github.com/nicodishanthj/icici-direct-mcp-server)

## How plugin-shipped MCP servers reach the user

Confirmed against Claude Code docs, **2026-09-01**: a plugin's
`.mcp.json` servers **auto-start when the plugin is enabled and
auto-disconnect when it's disabled, with no approval prompt** — unlike
user/project `.mcp.json` entries, which do prompt. So users get all
four brokers without hand-editing anything, and removing an unused
broker means editing their local copy of the plugin's `.mcp.json`.

Two caveats worth keeping honest in user-facing docs:

- **`$CLAUDE_PLUGIN_ROOT` is not documented as available in the user's
  own shell** — only inside hooks and Claude Code's own config
  substitution. Don't hand users a `cp "$CLAUDE_PLUGIN_ROOT/..."`
  command; have them ask the agent to create the file instead.
  (Installed plugins do live under
  `~/.claude/plugins/cache/{marketplace}/{plugin}/{version}/`, but that
  path is version-scoped and brittle.)
- **Codex and Cursor parity is unconfirmed.** Whether they auto-register
  a plugin's MCP servers the same way isn't documented in Claude Code's
  docs and hasn't been tested here. Verify before asserting it.

Plugins **cannot** ship enforced permissions — `permissions` in
plugin-supplied settings is ignored by design, which is why the deny
list is something the user installs themselves. See `READ-ONLY-POLICY.md`.

## Adding a new broker

1. Confirm an MCP server exists for it (official or self-hosted) and
   what it authenticates against. If none exists, this plugin can't
   support that broker yet — say so rather than inventing tool names.
2. Add its entry to `.mcp.json` (`command`/`args`, or `type: "http"` +
   `url` if it doesn't need `mcp-remote`'s OAuth-cache wrapper — check
   whether it needs a stable local port distinct from the others).
3. Connect it yourself and, capability by capability, call each tool
   once and confirm it returns real data — don't transcribe its docs
   into the table unverified. Add a row to **Verification status**
   with today's date once you have.
4. Fill in its column across the **Capability map** in
   `BROKER-CAPABILITIES.md`, one row per capability it covers; leave
   cells blank where it has nothing.
5. Enumerate its write/order tools in that file's **Write tools (never
   call)** table, and add the matching `mcp__<serverkey>__<tool>`
   entries to `.claude/settings.json`'s deny list and to
   `docs/install.md`'s copy-paste block.
6. Add it as an option in `BROKERS.example.md`.
7. If a skill needs a capability only this broker provides, note that in
   the skill's own steps — don't assume every user has it configured.
