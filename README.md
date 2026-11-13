# ProductGuru MCP server: product lookup by barcode

A public, read-only MCP server at `https://myproduct.guru/mcp` (Streamable HTTP, no account, no API key).
This repository holds its client configuration: a [Gemini CLI](https://github.com/google-gemini/gemini-cli)
extension, a Cursor plugin manifest and setup notes for other MCP clients.

Give it a barcode and it returns what ProductGuru has recorded for that product: name, brand, category and
recorded specifications such as dimensions, weight or model number. For those products it also lists any
product-safety recall notice from the government registers ProductGuru follows, with the agency's name, the date,
the stated reason and a link to the agency's own notice.

## Install in Gemini CLI

```bash
gemini extensions install https://github.com/PROGU223/productguru-mcp
```

Restart Gemini CLI, then check the connection with `/mcp`. You should see the `productguru` server with one tool,
`product_lookup`.

No account, API key or sign-in is needed for this server.

Server documentation: https://myproduct.guru/developers

### Without the extension

Add the server to `~/.gemini/settings.json` (all projects) or `.gemini/settings.json` (one project):

```json
{
  "mcpServers": {
    "productguru": {
      "httpUrl": "https://myproduct.guru/mcp"
    }
  }
}
```

or run:

```bash
gemini mcp add --transport http productguru https://myproduct.guru/mcp
```

## Other MCP clients

The server needs no account, key or local process. Point any client that supports remote (Streamable HTTP)
MCP servers at `https://myproduct.guru/mcp`.

**Claude Code**

```bash
claude mcp add --transport http productguru https://myproduct.guru/mcp
```

**Claude (web and desktop):** Settings → Connectors → Add custom connector → URL `https://myproduct.guru/mcp`.

**Cursor:** add to `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

```json
{
  "mcpServers": {
    "productguru": {
      "url": "https://myproduct.guru/mcp"
    }
  }
}
```

**Cline:** add to `cline_mcp_settings.json` (details in [llms-install.md](llms-install.md)):

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

**VS Code:** add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "productguru": {
      "type": "http",
      "url": "https://myproduct.guru/mcp"
    }
  }
}
```

The server is also listed in the [Official MCP Registry](https://registry.modelcontextprotocol.io) as
`guru.myproduct/product-lookup`.

## Example prompts

- "Look up the barcode 3000121297768. Has it been recalled?"
- "What product is EAN 5908274117071?"

## The tool

| | |
|---|---|
| Name | `product_lookup` |
| Input | `ean`: one EAN-13, EAN-8 or 12-digit UPC-A barcode (check digit validated) |
| Returns | name, brand, category, recorded attributes, recall notices (agency, date, reason, notice link), the URL of the product's page on myproduct.guru |
| Annotations | read-only, non-destructive, idempotent, closed-world |

## What it does not do

- It returns no prices, offers, images, reviews or marketing descriptions.
- It gives no medical, legal or financial advice. Medicines are not covered.
- When ProductGuru has no published page for a barcode, the answer says so and makes no statement about recalls.
- A product without a linked notice is not a safety guarantee: many official notices carry no barcode. The agency
  is always the source of a recall; confirm any single case with the agency.
- One barcode per call. There is no search or browse.

## Limits

Fair use: 20 look-ups per minute and 200 per day per client. For a higher limit, write to
support@myproduct.guru.

## Privacy

The server logs, per call, a salted hash of the client address, the barcode, the result and the latency, kept
for 30 days. It receives only the barcode, not your conversation. Full policy: https://myproduct.guru/privacy

## Licence

The files in this repository (configuration and documentation) are released under the MIT licence. The licence
does not cover the data the server returns.
