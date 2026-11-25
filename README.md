# Wayback Machine Snapshots MCP Server

[![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-wayback-machine-snapshots%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-wayback-machine-snapshots&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-wayback-machine-snapshots)](https://www.npmjs.com/package/@mambalabsdev/mcp-wayback-machine-snapshots) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-wayback-machine-snapshots)](https://www.npmjs.com/package/@mambalabsdev/mcp-wayback-machine-snapshots) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-wayback-machine-snapshots)](https://github.com/mambalabsdev/mcp-wayback-machine-snapshots/blob/main/LICENSE)

An MCP server for the Mamba Labs Wayback Machine Snapshots actor on Apify. Wayback Machine capture history as rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Takes web addresses or domains and returns one row per Wayback Machine capture from the archive's capture index, with the archived page text when asked.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-wayback-machine-snapshots": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-wayback-machine-snapshots"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `list_wayback_snapshots` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "When did the Stripe pricing page change in 2019, and what did it say?"
- "List the pages archived on linear.app, one capture each."

## Tool

`list_wayback_snapshots`. One tool. Example arguments:

```json
{
  "urls": [
    "https://stripe.com/pricing"
  ],
  "collapse": "changes",
  "only_successful": true,
  "max_snapshots_per_url": 10
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `urls` | array | no | Web addresses (https://stripe.com/pricing) or domains (stripe.com). Up to 10,000 captures each. Clay: put the column inside a one item list. |
| `match_type` | string | no | exact (default): this one address. prefix: every address that starts with it, for example stripe.com/docs. host: every address on this host. domain: this host and every subdomain. Host and domain searches of very large sites can time out in the archive's index. |
| `date_from` | string | no | Optional. Earliest capture, as a year, month, or date: 2019, 2019-06, or 2019-06-15. |
| `date_to` | string | no | Optional. Latest capture, as a year, month, or date. |
| `closest_to` | string | no | Optional. Return the captures nearest this year, month, or date instead of a date range, nearest first. With this set, Max snapshots per address defaults to 1, From and To are not used, and with Read page text only HTTP 200 captures are kept. |
| `collapse` | string | no | none (default): every capture. day, month, or year: the first capture in each. changes: skip a capture whose content matches the one before it. |
| `only_successful` | boolean | no | yes: keep only captures the site answered with HTTP 200. no (default): every status, redirects and revisit records included. |
| `only_html` | boolean | no | yes: keep only text/html captures, which drops images, scripts, and files on prefix, host, and domain searches. no (default): every type. |
| `newest_first` | boolean | no | yes: return the newest captures first, in one index request. no (default): oldest first. |
| `max_snapshots_per_url` | number | no | Captures to return for each address or domain, 1 to 10,000. Default 100, or 1 with Closest to date. |
| `include_text` | boolean | no | yes: also read each HTML capture's archived page for its title, description, visible text, and word count, billed as a snapshot with text. Captures that are not HTML 200 pages are billed as a plain snapshot. no (default): index rows only. |
| `text_max_chars` | number | no | With Read page text: characters of page text to keep per capture, 200 to 50,000. Default 5,000. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `snapshot-row`, one capture from the index | $0.0043 | $0.004085 | $0.00387 | $0.003655 |
| `snapshot-text-row`, one capture with its page text | $0.008 | $0.0076 | $0.0072 | $0.0068 |
| `apify-actor-start`, per run start | $0.005 | $0.005 | $0.005 | $0.005 |

Repeats, summaries, refused requests, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/wayback-machine-snapshots](https://apify.com/mambalabs/wayback-machine-snapshots)

## License

MIT. See [LICENSE](LICENSE).
