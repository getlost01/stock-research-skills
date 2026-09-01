# Active Brokers

**This is the template.** Ask *"which brokers should I set up?"* and
this gets copied to `BROKERS.md` at your project root and git-ignored
for you — or copy it there by hand and add `BROKERS.md` to
`.gitignore` yourself.

`BROKERS.md`, at your project root, says which broker MCP server(s) are
active and, when more than one is, which to prefer per capability.
Skills read this before consulting `reference/BROKER-CAPABILITIES.md` —
a broker not listed here is never used, even if its MCP server happens
to be connected (e.g. `.mcp.json` ships all four by default; that isn't
the same as opting in).

## Active

List one or more, in preference order. First listed wins when more than
one active broker covers the same capability.

```
1. groww
```

To add another, uncomment/add its line and reorder as you like:

```
1. groww      # fundamentals, screeners, technicals, F&O tools, IPO listings
2. kite       # order/trade history, F&O positions if you trade via Zerodha
3. indmoney   # real mutual fund data, net-worth snapshot
4. upstox     # holdings/positions/orders/MF if you trade via Upstox (tool names unverified — see BROKER-CAPABILITIES.md)
```

Angel One isn't available — no official MCP server exists for it; see
the **Verification status** note in `reference/BROKER-RESEARCH.md`.

## Single-broker setup (default)

Most users only need one line. Leave it at `groww` (or whichever one
broker you actually trade through) and skip the rest of this file —
skills fall back to "not available from any active broker" for anything
that broker's MCP doesn't cover, same as today.

## Notes

- Connecting a broker's MCP server here still requires its own
  authentication (see `docs/install.md`) — listing it here doesn't log
  you in.
- If you only use one broker, consider removing the other entries from
  your local `.mcp.json` too, so you're not prompted to authenticate
  accounts you don't have.
- Preference order only matters where brokers overlap. Most capabilities
  in `BROKER-CAPABILITIES.md` are covered by exactly one broker, so
  order rarely changes the answer — it matters most for LTP/quotes and
  holdings, which several brokers expose.
