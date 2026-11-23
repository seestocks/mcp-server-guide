# SeeStocks MCP Server

Search products sold by Spanish online shops, compare their prices in every shop (shipping included) and check their real price history, directly from your AI assistant.

[SeeStocks](https://es.seestocks.com) is a price comparison site and Google CSS partner for Spanish online shops. Its remote MCP server is **public, read-only and needs no account or API key**.

```
https://es.seestocks.com/mcp
```

Transport: Streamable HTTP. Authentication: none.

## Tools

| Tool | What it does |
|---|---|
| `buscar_productos` | Search products by text, EAN barcode or shop reference, with optional brand, shop and price filters and price sorting. Returns only relevant results, each with price, shipping, shop, number of shops and a link. |
| `ficha_producto` | Every shop that sells a product, sorted by price, with shipping and total, plus the history of its lowest price: 90-day minimum and maximum, whether today is the 90-day low, last change and recent prices. |
| `ofertas_marca` | Recent real price drops and discounted products of a brand, plus the brand's price statistics. |

The tools chain through the product `id` returned by `buscar_productos`.

## Setup

### Claude (web, desktop and mobile)

1. Open **Settings → Connectors** and choose **Add custom connector**.
2. Name: `SeeStocks`. URL: `https://es.seestocks.com/mcp`. No sign-in is needed.

### ChatGPT

Search for **SeeStocks** in the ChatGPT app directory (coming soon).

### Claude Code

```bash
claude mcp add --transport http seestocks https://es.seestocks.com/mcp
```

### VS Code

`.vscode/mcp.json`:

```json
{
  "servers": {
    "seestocks": { "type": "http", "url": "https://es.seestocks.com/mcp" }
  }
}
```

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "seestocks": { "url": "https://es.seestocks.com/mcp" }
  }
}
```

### Any other client

Any MCP client that supports the Streamable HTTP transport can connect to `https://es.seestocks.com/mcp`.

## Example prompts

- "Busca una piscina desmontable Bestway de menos de 300 €."
- "¿Dónde está más barata la primera, contando el envío?"
- "¿Es buen momento para comprarla o ha estado más barata?"
- "¿Qué ofertas hay ahora de Bosch?"

Queries work best in Spanish, the language of the shops.

## How to read the data

- Prices are the shops' selling prices, in euros.
- A shipping cost of `null` means the shop does not publish it, not that shipping is free.
- **Price drops** (`bajada`) are real price changes recorded by SeeStocks in that shop.
- The **recommended retail price** (`pvp_tienda_eur`) is declared by the shop and not verified by SeeStocks: a discount on it is not presented as a price drop.
- The price history follows the lowest price across shops and does not say which shop had each price.
- Ranking is never paid: [how SeeStocks orders results](https://es.seestocks.com/como-ordenamos-los-resultados.html).

## Limits

Requests to `/mcp` are rate-limited per IP address. `buscar_productos` returns at most 20 results per call.

## HTTP API

The same data is available as JSON, documented in [llms.txt](https://es.seestocks.com/llms.txt):

- `GET https://es.seestocks.com/api/buscar?q=…`
- `GET https://es.seestocks.com/api/producto/{id}`
- `GET https://es.seestocks.com/api/marca/{marca}/ofertas`

## Privacy

The server needs no personal data. SeeStocks logs the query, filters, tool and calling platform to measure usage and improve search; it does not receive your conversation. See the [privacy policy](https://es.seestocks.com/privacidad.html) (section 11).

## Support

- Connector page: https://es.seestocks.com/conector-ia
- Email: hola@seestocks.com

SeeStocks is operated by OT ECOMMERCE INTERNATIONAL S.L. (Spain).
