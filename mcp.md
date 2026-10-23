---
title: "eCourtsIndia MCP server and API quickstart"
---

This page explains how to connect AI assistants to [eCourtsIndia](https://ecourtsindia.com/). Source and issues: [ecourtsindia-mcp-server on GitHub](https://github.com/rchtjn2-gif/ecourtsindia-mcp-server). Back to the [eCourtsIndia guide](./).

<p align="center"><img src="logo.png" alt="eCourtsIndia MCP server logo" width="120"></p>

# eCourtsIndia MCP Server: Indian court cases, orders, cause lists and statutes for AI agents

> Search 27.6 crore Indian court cases, read orders and judgments, check cause lists and look up any section of Indian law, from Claude, ChatGPT, Cursor or any MCP client.

**MCP endpoint:** `https://mcp.ecourtsindia.com/mcp` · **Transport:** Streamable HTTP · **Tools:** 44 · **Website:** [ecourtsindia.com](https://ecourtsindia.com)

This repository is the public listing for the **hosted** eCourtsIndia MCP server: connection instructions, the registry manifest (`server.json`) and client config files. There is nothing to install or self-host; the server runs at the URL above.

## Connect in one step

Add this URL to any MCP client that supports remote servers:

```
https://mcp.ecourtsindia.com/mcp
```

Clients without OAuth support can use the token URL instead: `https://mcp.ecourtsindia.com/mcp?token=YOUR_API_KEY`

### Claude (claude.ai, Claude Desktop, Claude mobile)
Settings → Connectors → **Add custom connector** → paste the URL above → Connect.

### Claude Code
```bash
claude mcp add --transport http ecourtsindia https://mcp.ecourtsindia.com/mcp
```

### ChatGPT
Settings → Apps & Connectors → Advanced → enable Developer mode → **Create** a connector → paste the URL above.

### Cursor, Windsurf, Cline and other JSON-config clients
```json
{
  "mcpServers": {
    "ecourtsindia": {
      "url": "https://mcp.ecourtsindia.com/mcp"
    }
  }
}
```

### VS Code (GitHub Copilot)
Add to `.vscode/mcp.json`:
```json
{
  "servers": {
    "ecourtsindia": {
      "type": "http",
      "url": "https://mcp.ecourtsindia.com/mcp"
    }
  }
}
```

### Gemini CLI
```bash
gemini extensions install https://github.com/rchtjn2-gif/ecourtsindia-mcp-server
```

## Authentication
OAuth 2.1 (dynamic client registration). Your MCP client opens an eCourtsIndia sign-in; approve it and you are connected. Clients without OAuth can append `?token=YOUR_API_KEY` (a Partner API key that starts with `eci_live_`, from https://ecourtsindia.com/dashboard/api/keys).

## What data it covers
- **27.6 crore case records** from the Supreme Court of India, all 25 High Courts and their benches, 600+ district and taluka courts
- **18 tribunal types**: NCLT, NCLAT, ITAT, CESTAT, DRT/DRAT, NGT, AFT, SAT, TDSAT, APTEL, CAT, CCI, GSTAT, RCT, consumer commissions and more
- **Orders and judgments** as clean text (OCR'd from the court PDF) plus AI summaries: outcome, relief and provisions applied
- **Daily cause lists** for 8,000+ district and taluka courts
- **The statute book**: 10,086 bare Acts, 272,623 provisions, the CPC First Schedule, 43,376 rules and notifications, IPC to BNS / CrPC to BNSS / Evidence Act to BSA mapping, and income-tax, TDS, GST and customs rate tables
- **Indian electoral roll** search (a separate dataset published by the ECI and state CEOs)

## Tools (44)
| Group | Tools |
|---|---|
| Search | `search_cases`, `get_search_capabilities` |
| Case records | `get_case_details`, `get_case_brief`, `get_case_with_latest_order`, `search_and_get_first_case`, `search_and_brief_top_cases`, `batch_get_case_details` |
| Orders and judgments | `list_case_orders`, `get_order_markdown`, `get_order_ai_analysis` |
| Live refresh from court servers | `refresh_case`, `refresh_and_wait_for_case`, `bulk_refresh_cases`, `check_refresh_status`, `monitor_portfolio` |
| Cause lists | `search_causelist`, `get_court_docket`, `check_cnr_causelist`, `get_available_causelist_dates` |
| Court structure (free) | `get_states`, `get_districts`, `get_complexes`, `get_courts` |
| Statute book (free) | `search_statute`, `lookup_provision`, `lookup_case_provisions`, `map_provision`, `get_cpc_rule`, `lookup_offence`, `list_instruments`, `get_instrument`, `get_act`, `check_act_status`, `lookup_tax_rate` |
| Litigation background check | `submit_legal_check`, `get_legal_check`, `list_legal_checks` |
| Electoral roll | `search_electoral_roll`, `lookup_epic`, `get_electoral_capabilities` |
| Account and reference | `buy_credits`, `lookup_enum`, `fetch_live_enums` |

## Example prompts
- "What is the latest status and next hearing date of CNR DLHC010012342024?"
- "Find Supreme Court judgments on Section 138 of the Negotiable Instruments Act from 2024 and summarise the outcomes."
- "Is my case listed in tomorrow's cause list at Saket District Court?"
- "What does Section 103 of the BNS say, and which IPC section did it replace? Is it bailable?"
- "Run a litigation background check on 'Acme Infra Private Limited' before we sign the vendor contract."
- "List every order passed in this NCLT insolvency case and summarise the last three."

## Pricing
Free signup with ₹200 of credits, no card needed. 24 of the 44 tools never use credits (statute book, court structure, enums, refresh status). Paid tools draw prepaid credits; see https://mcp.ecourtsindia.com/pricing/.

## FAQ
### Is there an MCP server for Indian court cases?
Yes. eCourtsIndia runs a hosted MCP server at https://mcp.ecourtsindia.com/mcp that covers the Supreme Court, every High Court, 600+ district courts and 18 tribunals, with orders, cause lists and the statute book.

### How do I check Indian case status from ChatGPT or Claude?
Add https://mcp.ecourtsindia.com/mcp as a custom connector, sign in, and ask for the case by CNR number, party name, advocate or case number. The assistant calls get_case_details or search_cases and can refresh the record live from the court.

### Does it cover the new criminal laws (BNS, BNSS, BSA)?
Yes. lookup_provision returns the text of any BNS, BNSS or BSA section, and map_provision maps IPC, CrPC and Evidence Act sections to their new equivalents.

### Is it official?
No. eCourtsIndia is a non-government initiative that organises public court records. It is not affiliated with any court or government body.

### Is it free?
Signup is free with ₹200 of credits, and the statute book and court-structure tools are always free. Case search, orders and refreshes use prepaid credits.

## Links
- [MCP guide for agents (llms.txt)](https://mcp.ecourtsindia.com/llms.txt)
- [REST API docs](https://ecourtsindia.com/api/docs)
- [Server card](https://mcp.ecourtsindia.com/.well-known/mcp.json)
- [Pricing](https://mcp.ecourtsindia.com/pricing/)
- [Sign up (₹200 free credit)](https://ecourtsindia.com)
- [Privacy policy](https://ecourtsindia.com/privacy)
- [Terms of service](https://ecourtsindia.com/terms-of-service)

## About this repository
This repo holds listing and configuration files only. The eCourtsIndia MCP server is a hosted service operated by eCourtsIndia; its use is governed by the terms linked above. The files in this repository are released under the MIT licence.
