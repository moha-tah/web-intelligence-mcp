# Web & Company Intelligence MCP

Give your AI agent (Claude, Cursor, VS Code Copilot, n8n, any MCP client) five web-intelligence tools that return clean JSON and cost **cents per result**.
There's nothing to host: the tools run on [Apify](https://apify.com/moha-tah) and are served through Apify's official MCP server.

| Tool | What your agent gets | Price |
|---|---|---|
| [`moha-tah/tech-stack-detector`](https://apify.com/moha-tah/tech-stack-detector) | 7,600+ technologies detected from HTML, headers, scripts **and DNS** (email provider, SaaS verifications), plus a match filter such as "uses Shopify?" | $0.004 per domain, unreachable sites free |
| [`moha-tah/website-company-profile`](https://apify.com/moha-tah/website-company-profile) | Company name, description, logo, LinkedIn/X, country, careers page, ATS with live open-job count, email provider and DMARC policy. No personal data. | $0.003 per domain, unreachable sites free |
| [`moha-tah/sitemap-extractor`](https://apify.com/moha-tah/sitemap-extractor) | All URLs from a site's sitemaps (auto-discovered), or only the **new/changed** URLs since the last check | $0.0003 per URL · monitor mode $0.002 per site + $0.001 per change |
| [`moha-tah/url-status-checker`](https://apify.com/moha-tah/url-status-checker) | Status, full redirect chain, loops, noindex, canonical and the Googlebot robots.txt verdict: *is this page indexable?* | $0.0008 per URL |
| [`moha-tah/eu-fuel-prices`](https://apify.com/moha-tah/eu-fuel-prices) | Official fuel prices for 43,000+ stations in France, Spain and Italy, filtered by radius, brand or fuel | $0.0005 per station |

The free Apify plan's monthly credits cover typical agent use.
Agents without an Apify account can pay per run in USDC through [x402 agentic payments](https://docs.apify.com/platform/integrations/x402).

## Connect

Server URL:

```
https://mcp.apify.com/?tools=moha-tah/tech-stack-detector,moha-tah/website-company-profile,moha-tah/sitemap-extractor,moha-tah/url-status-checker,moha-tah/eu-fuel-prices
```

Authenticate with OAuth (clients that support it prompt you to sign in) or with an API token header: `Authorization: Bearer <APIFY_TOKEN>`. Get a token from a [free Apify account](https://console.apify.com/sign-up) → Settings → API & Integrations.

**Claude Code**

```bash
claude mcp add --transport http web-intel "https://mcp.apify.com/?tools=moha-tah/tech-stack-detector,moha-tah/website-company-profile,moha-tah/sitemap-extractor,moha-tah/url-status-checker,moha-tah/eu-fuel-prices" --header "Authorization: Bearer $APIFY_TOKEN"
```

**Claude Desktop / claude.ai**

Settings → Connectors → *Add custom connector* → paste the server URL → sign in to Apify.

**Cursor** (`~/.cursor/mcp.json`) and **VS Code** (`.vscode/mcp.json`)

```json
{
  "mcpServers": {
    "web-intel": {
      "url": "https://mcp.apify.com/?tools=moha-tah/tech-stack-detector,moha-tah/website-company-profile,moha-tah/sitemap-extractor,moha-tah/url-status-checker,moha-tah/eu-fuel-prices",
      "headers": { "Authorization": "Bearer ${APIFY_TOKEN}" }
    }
  }
}
```

For VS Code, use `"servers"` instead of `"mcpServers"` and add `"type": "http"`.

Only need one tool? Keep just that Actor in the `tools=` list.

## Example prompts

- "Which of these 50 domains use HubSpot and Google Workspace? Return a table."
- "Enrich stripe.com, qonto.com and pennylane.com: logo, LinkedIn, ATS and open jobs."
- "What pages did notion.com add to its sitemap since yesterday?"
- "Check these 20 URLs and tell me which ones aren't indexable, and why."
- "Cheapest E10 within 5 km of the Eiffel Tower right now."

## n8n workflows

Ready-to-import n8n workflows built on the same tools are in [`n8n/`](n8n/):
- company enrichment → Google Sheets;
- tech-stack lead qualification → Slack;
- competitor new pages → Slack;
- broken or de-indexed key pages → Slack;
- cheapest fuel → Telegram.

## Data & compliance

- The tools respect robots.txt.
- They collect company-level data only (no personal profiles or contact persons).
- Fuel prices come from official government open data (Licence Ouverte v2.0 / Spanish and Italian open data), and each record carries its attribution.

## License

MIT for this repository (configs, docs, n8n workflows). The tools themselves are Apify Actors billed per result.
