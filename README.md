# ProductGuru product lookup for Gemini CLI

A [Gemini CLI](https://github.com/google-gemini/gemini-cli) extension that connects Gemini CLI to the public,
read-only ProductGuru MCP server at `https://myproduct.guru/mcp`.

Give it a barcode and it returns what ProductGuru has recorded for that product: name, brand, category and
recorded specifications such as dimensions, weight or model number. For those products it also lists any
product-safety recall notice from the government registers ProductGuru follows, with the agency's name, the date,
the stated reason and a link to the agency's own notice.

## Install

```bash
gemini extensions install https://github.com/PROGU223/productguru-mcp
```

Restart Gemini CLI, then check the connection with `/mcp`. You should see the `productguru` server with one tool,
`product_lookup`.

No account, API key or sign-in is needed for this server.

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

The server logs, per call, a salted client hash, the barcode, the result and the latency, for 30 days. It does not
receive or store your conversation.

## Licence

The files in this repository (configuration and documentation) are released under the MIT licence. The licence
does not cover the data the server returns.
