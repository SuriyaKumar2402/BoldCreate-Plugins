# Shippo

BoldCreate plugin that connects agents to Shippo through its hosted [Model Context Protocol](https://modelcontextprotocol.io/) server over streamable HTTP.

Ship packages with Shippo: compare carrier rates, buy labels, validate addresses, track packages, handle customs and process batches from your agent.

## Install

- **BoldCreate Connect:** open **Marketplace**, find **Shippo**, and click **Install**.
- **Cursor:** add this repository as a team marketplace, then install **Shippo**.
- **Claude Code:** `/plugin marketplace add syncfusion/BoldCreate-Plugins`, then `/plugin install shippo@boldcreate-plugins`.

## MCP

```json
{
  "mcpServers": {
    "shippo": {
      "type": "http",
      "url": "https://mcp.shippo.com"
    }
  }
}
```

## Setup

No settings.

The first time the agent calls a tool, each member signs in to their own Shippo account in the browser (OAuth). Shippo's page says there is nothing to install and no API key to manage.

## Which organization this is

`github.com/goshippo` is the GitHub organization of Shippo. Its `ai` repository holds Shippo's agent skills and plugins. The plugin source is [goshippo/ai](https://github.com/goshippo/ai).

## Endpoint

The server address is `https://mcp.shippo.com`, with no path. A request to `https://mcp.shippo.com/mcp` returned HTTP 403 in a live check, so do not add one.

## Authentication

A live check on 2026-11-15 found:

- An unauthenticated request returned HTTP 401 and the JSON error "Missing Bearer token". The standard `WWW-Authenticate` header was not in the reply. The same text, with a link to the metadata, came back under the name `x-amzn-remapped-www-authenticate`, which an Amazon gateway adds. A client that finds OAuth only from `WWW-Authenticate` will not find it.
- The protected-resource metadata at `/.well-known/oauth-protected-resource` names `https://goshippo.com` as the authorization server, with the scope `*`.
- The authorization server metadata at `https://goshippo.com` lists a registration endpoint (`/oauth/clients`, dynamic client registration), PKCE with S256, and the `authorization_code` grant. It lists no refresh-token grant and no client ID metadata documents. Its only token endpoint method is `none`, so registration does not issue a client secret.

So members connect without a client ID or client secret from your team. No registration request was sent to the server during the check. A plain GET to the registration address returned HTTP 405, which shows it exists but does not show that it accepts a registration from this client.

Shippo's help pages name Claude, ChatGPT and Cursor. They do not list BoldCreate Connect. **Not checked:** whether Shippo accepts Connect's redirect address.

## Skills

The `skills/` folder holds 9 skills and a shared `shippo/references/` folder, copied unchanged from the Claude Code plugin in [goshippo/ai](https://github.com/goshippo/ai/tree/cf8e96532f243883ff84f6d4035e72884b46d132/providers/claude/plugin/skills) at commit `cf8e96532f243883ff84f6d4035e72884b46d132`. Each folder's tree hash was compared with the one GitHub returns for that commit. A copy of the repository's `LICENSE` (MIT, copyright 2025 Shippo) is in `skills/LICENSE`.

| Skill | What Shippo says it does |
| --- | --- |
| `address-validation` | Validate, parse and standardize US and international addresses. |
| `batch-shipping` | Process CSV files of shipments and generate labels in bulk. |
| `label-purchase` | Purchase domestic and international shipping labels with customs handling. |
| `rate-shopping` | Compare rates across USPS, UPS, FedEx, DHL and 30+ carriers. |
| `shipping-analysis` | Analyze costs, optimize package dimensions, compare carriers and review historical spend. |
| `shippo-best-practices` | Decision router for Shippo integrations: which API to use, test and live mode, response handling and critical rules. |
| `shippo-support-ticket` | Build a classified, routing-tagged support ticket (text and JSON) for one shipment or label. Read-only. |
| `tracking` | Track packages across carriers with status history, substatus codes and webhooks. |
| `upgrade-shippo` | Guide for upgrading SDK versions, MCP server updates and breaking-change migration. |

**The copy that ships.** Shippo keeps its skills in a top-level `skills/` folder and mirrors them into the plugin. The mirror differs from the source only by a "DO NOT EDIT. Auto-generated" comment under each file's header. This plugin uses the mirror, because it is the copy Shippo ships.

**Scripts and installs.** The skills are plain Markdown. They contain no scripts and no install commands, and every link to a file in the folder resolves.

**Tool names.** `shippo/references/tool-reference.md` says the server exposes four tools (`shippo_list_tools`, `shippo_describe_tool`, `shippo_read_execute_tool` and `shippo_write_execute_tool`) and that the operation names in the skills, such as `CreateShipment`, are passed to them. I did not read the tool list behind sign-in, so I could not confirm this.

**Support-ticket skill.** `shippo-support-ticket` says its audience is Shippo support agents, and its output uses Shippo's internal routing tags. It produces text and JSON to copy. It does not open a ticket or send anything itself.

**Left out.** Shippo's repository also holds a Codex plugin, a ClawHub skill digest, a "knowledge pack" and a local MCP server under `tools/`. They are not used here.

## Before you buy a label

Shippo's page says getting rates and validating addresses cost nothing, and that buying a label charges your Shippo account. The `label-purchase` skill tells the agent to state "this will charge your Shippo account" with the carrier, service and cost, and to wait for an explicit yes. Review the plan yourself before approving any purchase.

## Docs

- Shippo MCP server: https://docs.goshippo.com/guides/mcp-server
- Shippo agent skills and plugins: https://github.com/goshippo/ai

## Logo

Shippo's mark from `tools/modelcontextprotocol/assets/shippo-logo.png` in [goshippo/ai](https://github.com/goshippo/ai), scaled and centred on a white tile. It is a white mark on a green square, as Shippo publishes it. It is Shippo's own mark and a trademark of its owner. It is not covered by the MIT licence.

## License

MIT
