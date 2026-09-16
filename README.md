# mcp-covalent

Covalent / GoldRush MCP — unified wallet balances + transactions across 100+ chains

Part of [Pipeworx](https://pipeworx.io) — an MCP gateway connecting AI agents to 1576+ live data sources.

## Tools

| Tool | Description |
|------|-------------|
| `covalent_token_balances` | Token balances for wallet X on chain Y — every ERC-20/native token held by an address, with USD quotes. Works across 100+ chains via Covalent/GoldRush. Example: covalent_token_balances({ address: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", chain: "eth-mainnet", _apiKey: "your-key" }) |
| `covalent_transactions` | Recent transactions for wallet X on chain Y — latest on-chain transactions for an address with USD-valued amounts and gas. Works across 100+ chains via Covalent/GoldRush. Example: covalent_transactions({ address: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", chain: "eth-mainnet", _apiKey: "your-key" }) |
| `covalent_token_holders` | Top holders of token X on chain Y — the largest wallet holders of an ERC-20 token contract, with balances. Works across 100+ chains via Covalent/GoldRush. Example: covalent_token_holders({ token_address: "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48", chain: "eth-mainnet", _apiKey: "your-key" }) |

## Quick Start

Add to your MCP client (Claude Desktop, Cursor, Windsurf, etc.):

```json
{
  "mcpServers": {
    "covalent": {
      "url": "https://gateway.pipeworx.io/covalent/mcp"
    }
  }
}
```

### What this endpoint actually serves

`tools/list` at `https://gateway.pipeworx.io/covalent/mcp` returns the tools in the table
above **plus the shared Pipeworx meta-tools** — `ask_pipeworx`,
`discover_tools`, `search_within`, `remember`/`recall` and the rest of the
gateway-wide set. So the tool count you see is larger than this table: a
single-pack endpoint currently lists roughly 30 shared tools alongside the
pack's own. The connection's `initialize` response states its exact scope, and
is the authoritative answer for a given day.

This is deliberate, not multiplexing by accident. The meta-tools are what let a
scoped connection answer a question this pack does not cover — via
`ask_pipeworx`, which routes across the whole catalog — without you adding a
second MCP server. There is currently no way to mount a pack endpoint without
them; if the extra schemas cost you more context than the routing is worth,
connect to the full gateway once rather than to several pack endpoints.

Or connect to the full Pipeworx gateway to get every pack's tools listed
directly, instead of just this one's:

```json
{
  "mcpServers": {
    "pipeworx": {
      "url": "https://gateway.pipeworx.io/mcp"
    }
  }
}
```

Both URLs reach the same gateway and the same 1576+ data sources. The
only difference is which pack's tools are listed **directly**; `ask_pipeworx`
reaches all of them from either one.

## Standalone (no gateway account)

This package also runs as a local stdio MCP server — no Pipeworx account, no
gateway round-trip:

```json
{
  "mcpServers": {
    "covalent": {
      "command": "npx",
      "args": ["-y", "@pipeworx/mcp-covalent"]
    }
  }
}
```

Or run it directly to confirm it starts:

```bash
npx -y @pipeworx/mcp-covalent
```

It speaks MCP over stdin/stdout and answers `initialize`/`tools/list`/`tools/call`
for **only** this pack's tools — none of the shared meta-tools the gateway
connection above adds. Same source, same tools, no ask_pipeworx routing.

## Using with ask_pipeworx

Instead of calling tools directly, you can ask questions in plain English —
this works on the pack endpoint above as well as on the full gateway:

```
ask_pipeworx({ question: "your question about Covalent data" })
```

The gateway picks the right tool and fills the arguments automatically.

## More

- [Docs and guides](https://pipeworx.io/docs)
- [pipeworx.io](https://pipeworx.io)

## License

MIT
