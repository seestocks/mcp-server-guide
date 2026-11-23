# SeeStocks MCP Server

Find where to buy a product in Spanish online shops, compare its price in every shop (shipping included) and check its real price history, directly from your AI assistant.

[SeeStocks](https://es.seestocks.com) is a price comparison site and Google CSS partner for Spanish online shops. Its remote MCP server is **public, read-only and needs no account or API key**.

```
https://es.seestocks.com/mcp
```

Transport: Streamable HTTP. Authentication: none.

Listed in the [official MCP Registry](https://registry.modelcontextprotocol.io/v0.1/servers?search=com.seestocks/price-comparison) as `com.seestocks/price-comparison`.

## Tools

| Tool | What it does |
|---|---|
| `comparar_precios` | For a product the user has already chosen: pass brand + model or its EAN barcode (up to 5 products per call) and get every shop that sells it, cheapest first, with shipping, total, link and 90-day price history. If SeeStocks does not have it, it says so instead of suggesting other products. |
| `buscar_productos` | Search products by text, EAN barcode or shop reference, with optional brand, shop and price filters. Returns only relevant results, best match first, each with price, shipping, shop, number of shops and a link. |
| `ficha_producto` | Every shop that sells a product, sorted by price, with shipping and total, plus the history of its lowest price: 90-day minimum and maximum, whether today is the 90-day low, last change and recent prices. |
| `ofertas_marca` | Recent real price drops and discounted products of a brand, plus the brand's price statistics. |
| `explorar_categoria` | Overview of a type of product (e.g. "freidora de aire"): number of products and shops, price range (minimum, 10th percentile, median, 90th percentile, maximum), the cheapest products, the products sold by most shops and the biggest real price drops of the last 30 days, leaving out spare parts and accessories. Use it to find the cheapest products of a type. Optional price and brand filters. |

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

- "Compara precios de la freidora Habitex CC485D."
- "Busca una piscina desmontable Bestway de menos de 300 €."
- "¿Dónde está más barata la primera, contando el envío?"
- "¿Es buen momento para comprarla o ha estado más barata?"
- "¿Qué ofertas hay ahora de Bosch?"
- "¿Cuánto cuesta una freidora de aire y cuáles son las más baratas?"

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
- `GET https://es.seestocks.com/api/categoria/{id}/ofertas`

## Privacy

The server needs no personal data. SeeStocks logs the query, filters, tool and calling platform to measure usage and improve search; it does not receive your conversation. See the [privacy policy](https://es.seestocks.com/privacidad.html) (section 11).

## Support

- Connector page: https://es.seestocks.com/conector-ia
- Email: hola@seestocks.com

SeeStocks is operated by OT ECOMMERCE INTERNATIONAL S.L. (Spain).
