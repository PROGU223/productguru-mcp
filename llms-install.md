# Installing the ProductGuru MCP server (notes for AI agents such as Cline)

ProductGuru is a **remote** MCP server. There is nothing to clone, build or install, and no API key,
account or environment variable is needed.

- Endpoint: `https://myproduct.guru/mcp`
- Transport: Streamable HTTP
- Authentication: none

## Cline

Add this entry to `cline_mcp_settings.json` (MCP Servers → Configure), keeping any existing servers:

```json
{
  "mcpServers": {
    "productguru": {
      "type": "streamableHttp",
      "url": "https://myproduct.guru/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

`"type": "streamableHttp"` must be set explicitly; without it Cline falls back to the legacy SSE transport.

Or, in the Cline panel: MCP Servers → Remote Servers → name `productguru`, URL `https://myproduct.guru/mcp`,
transport Streamable HTTP → Add Server.

## Check that it works

The server exposes one tool, `product_lookup`, with one argument, `ean` (an EAN-13, EAN-8 or 12-digit UPC-A
barcode). Call it with `5908274117071`: the answer names a two-arm steering wheel puller by ENERGY. Call it with
`3000121297768`: the answer lists two official recall notices from RappelConso (France).

Fair use: 20 look-ups per minute and 200 per day per client.
