# Contractor License Lookup MCP Server

[![LightNow capabilities](https://lightnow.ai/badge/com.mambabuilt/mcp-contractor-license-lookup)](https://lightnow.ai/servers/com.mambabuilt/mcp-contractor-license-lookup) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-contractor-license-lookup/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-contractor-license-lookup) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-contractor-license-lookup%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-contractor-license-lookup&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-contractor-license-lookup)](https://www.npmjs.com/package/@mambalabsdev/mcp-contractor-license-lookup) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-contractor-license-lookup)](https://www.npmjs.com/package/@mambalabsdev/mcp-contractor-license-lookup) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-contractor-license-lookup)](https://github.com/mambalabsdev/mcp-contractor-license-lookup/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-contractor-license-lookup)

An MCP server for the Mamba Labs Contractor License Lookup actor on Apify. Give it a state and a trade to list licensed contractors, or a license number or business name to verify one. Each run reads the state licensing board's own public register.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Two modes:

- **list**: give `states` and `trades` (for example CA and roofing), optionally `cities` or a `county`. You get one row per licensed contractor with license number, status, expiration date, classifications, owners or qualifying parties, address, and phone.
- **lookup**: give `lookups`, one per line, for example `WA | 04CONCL862CR` or `LA | Acadian Builders`. You get the matching license with its status.

States covered: California (CSLB Data Portal lists), Texas (TDLR open data), Washington (Labor and Industries open data), Oregon (CCB open data), and Louisiana (LSLBC public search). California name and number lookups work only inside a trade list, because the CSLB license check page uses a reCAPTCHA. Each search ends with one free summary row. All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-contractor-license-lookup": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-contractor-license-lookup"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `lookup_contractor_licenses` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "List the licensed roofing contractors in San Diego County, California."
- "Is Washington contractor license 04CONCL862CR active, and who are the principals?"
- "Find licensed HVAC contractors in Austin, Texas, with their phone numbers."

## Tool

`lookup_contractor_licenses`. One tool. Example arguments:

```json
{
  "mode": "list",
  "states": ["CA"],
  "trades": ["roofing"],
  "county": "San Diego",
  "max_results": "100"
}
```

## Inputs

Every buyer facing input of the actor, unchanged. Omitted inputs take the actor's own defaults. Numbers and booleans are sent as strings.

| Input | Type | Description |
|---|---|---|
| `mode` | string | list (default) or lookup |
| `states` | array | CA, TX, WA, OR, LA |
| `trades` | array | List mode. Trade words |
| `cities` | array | List mode. Cities, searched separately |
| `county` | string | List mode. County (CA, TX, OR) |
| `business_name` | string | List mode: name filter. Lookup mode: one more name |
| `lookups` | array | Lookup mode. Lines such as `WA \| 04CONCL862CR` |
| `license_number` | string | Lookup mode. One more license number |
| `active_only` | string | true or false. Default true |
| `include_owners` | string | Washington principals. Default true |
| `max_results` | string | Rows per search, up to 5000 |
| `max_charge_usd` | string | Optional cost cap for the run |
| `proxy_mode` | string | auto or residential. Default auto |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `license-row`, per license returned | $0.004 | $0.0038 | $0.0036 | $0.0034 |
| `apify-actor-start`, per run start | $0.0005 | $0.0005 | $0.0005 | $0.0005 |

Search summary rows, and skipped, blocked, and no result rows, are free.

## Your keys

The actor needs no key. It reads the state boards' public registers and holds no key of any kind. Proxy traffic adds nothing to your price: Mamba Labs pays the platform costs of the run. `proxy_mode` is a reliability setting.

## Full actor documentation

[apify.com/mambalabs/contractor-license-lookup](https://apify.com/mambalabs/contractor-license-lookup)

## License

MIT. See [LICENSE](LICENSE).
