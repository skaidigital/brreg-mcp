# Norwegian Company Data MCP

A remote MCP server over the Norwegian business registers (Brønnøysundregistrene). Ask your AI
assistant to find, size up and reach Norwegian companies:

- search companies by industry, municipality, legal form, employees, founding or registration date,
  website pattern and revenue range
- full company profiles: contact details, activity, CEO, board, auditor and accountant, latest
  revenue and profit
- up to three years of annual accounts with growth, margin and equity ratio
- the corporate group a company belongs to, with ownership shares
- SN2025 industry codes from Norwegian or English words
- company websites the register is missing, verified by the organisation number on the site, and
  the platform each site runs on: Shopify, WooCommerce, Magento, Wix and more

No API key or account is needed. Data comes from the open
[Brønnøysund APIs](https://data.brreg.no/enhetsregisteret/api/dokumentasjon/en/index.html) under the
[Norwegian Licence for Open Government Data (NLOD) 2.0](https://data.norge.no/nlod/en/2.0).

**Endpoint:** `https://tools.skaidigital.com/brreg/mcp` (streamable HTTP, no auth)

The server is hosted and closed-source. This repository holds only the client configuration and
directory listings.

## Tools

| Tool | What it does |
| --- | --- |
| `search_norwegian_companies` | Find companies by industry, municipality (names like Bergen work), postal code, legal form, employees, founding or registration date, website pattern such as `*myshopify*`, filed accounts and revenue range. Active companies only by default. |
| `get_norwegian_company` | One company's full profile by organisation number, with roles and the latest filed revenue and profit. |
| `get_norwegian_company_financials` | Up to three years of company or group accounts, with growth, operating margin and equity ratio. |
| `get_norwegian_company_group` | The corporate group (konsern) a company belongs to: the top parent and every subsidiary with ownership share. |
| `find_norwegian_industry_codes` | Current SN2025 industry codes from Norwegian or English words or a code prefix. Older SN2007 codes match nothing in the register. |
| `find_norwegian_company_websites` | Websites of one to five companies, from the register, the email domain or the company name, verified on the site, with the platform each site runs on. |

Revenue is not searchable in the register, so a revenue filter checks one page of candidates at a
time; narrow with employee bounds that fit the range. Person roles are returned without birth dates.
Usage is capped per client and in total to keep the service free.

## Setup

### Cursor

Add to `~/.cursor/mcp.json`, or `.cursor/mcp.json` in a project:

```json
{
  "mcpServers": {
    "norwegian-companies": {
      "url": "https://tools.skaidigital.com/brreg/mcp"
    }
  }
}
```

### Claude Desktop

Open **Settings → Connectors → Add custom connector** and enter
`https://tools.skaidigital.com/brreg/mcp`.

On plans without custom connectors, add this to `claude_desktop_config.json` instead (requires
Node.js):

```json
{
  "mcpServers": {
    "norwegian-companies": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://tools.skaidigital.com/brreg/mcp"]
    }
  }
}
```

### Claude Code

```sh
claude mcp add --transport http norwegian-companies https://tools.skaidigital.com/brreg/mcp
```

### VS Code

Add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "norwegian-companies": {
      "type": "http",
      "url": "https://tools.skaidigital.com/brreg/mcp"
    }
  }
}
```

### Gemini CLI

```sh
gemini extensions install https://github.com/skaidigital/brreg-mcp
```

## Example prompts

- "Find retail companies in Oslo with 10–50 employees and 20–100 MNOK in revenue. Which have a
  website, and which run Shopify?"
- "Who is the CEO and board of Equinor, and how has revenue developed over three years?"
- "List software companies in Bergen founded after 2020."
- "Which companies belong to the same group as organisation number 996576027?"

## About

Built by [Skai Digital](https://www.skaidigital.com), a Shopify and headless commerce agency. Skai
also runs the [Shopify Speed Check MCP](https://github.com/skaidigital/shopify-speed-mcp).

The configuration files in this repository are MIT licensed.
