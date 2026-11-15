# ChartMogul

BoldCreate plugin that connects agents to ChartMogul through its hosted [Model Context Protocol](https://modelcontextprotocol.io/) server over streamable HTTP.

Access your ChartMogul revenue metrics, customer data and sales pipeline: MRR, ARR, churn, retention, cohorts, subscriptions and customer activity.

## Install

- **BoldCreate Connect:** open **Marketplace**, find **ChartMogul**, and click **Install**.
- **Cursor:** add this repository as a team marketplace, then install **ChartMogul**.
- **Claude Code:** `/plugin marketplace add syncfusion/BoldCreate-Plugins`, then `/plugin install chartmogul@boldcreate-plugins`.

## MCP

```json
{
  "mcpServers": {
    "chartmogul": {
      "type": "http",
      "url": "https://mcp.chartmogul.com"
    }
  }
}
```

## Setup

No settings.

The first time the agent calls a tool, each member signs in to their own ChartMogul account in the browser (OAuth) and authorizes access. ChartMogul's page says you need to be an Owner, Admin, Staff or Read-only team member.

## Before you connect

ChartMogul's page says MCP access is on by default. An Admin or Owner can turn it off under **Settings > Security & Access > Team Security**, with the setting **Allow MCP server access**. If it is off, sign-in will not work.

## Endpoint

The server address is `https://mcp.chartmogul.com`, with no `/mcp` path. A request to `https://mcp.chartmogul.com/mcp` returned HTTP 404 in a live check, so do not add one.

## Authentication

A live check on 2026-11-15 found:

- An unauthenticated request returned HTTP 401 with `WWW-Authenticate: Bearer realm="Doorkeeper"`. That header has no link to the protected-resource metadata, so a client that finds OAuth only from the header will not find it. The metadata is served at `/.well-known/oauth-protected-resource` on the same host.
- That metadata names `https://mcp.chartmogul.com` as the authorization server, with the scope `mcp`.
- The authorization server metadata lists a registration endpoint (`/oauth/register`, dynamic client registration) and PKCE with S256. The grants are `authorization_code` and `refresh_token`. The only token endpoint method is `none`, so registration does not issue a client secret. It does not list client ID metadata documents.

So members connect without a client ID or client secret from your team. No registration request was sent to the server during the check. A plain GET to `/oauth/register` returned 404, which is normal for an endpoint that only takes POST, but it means that I could not confirm the address from a response.

ChartMogul's page also says the authorization screen shows a warning that ChartMogul has not verified the app, unless the app is Claude or ChatGPT. Approve only apps you recognise.

**Not checked.** The tool list behind sign-in was not read, and whether ChartMogul accepts BoldCreate Connect's redirect address was not checked.

## What the server does

ChartMogul says the server connects with read-only access, and that data is fetched only in response to your questions. Its list of tools (help article 348) covers:

- Metrics: MRR, ARR, ARPA, ASP, LTV, churn, retention, cohorts, cash flow, trial conversion, sales cycle length, benchmarks and scenario analysis.
- Breakdowns by region, plan, custom attribute and tag.
- Customers, contacts, subscriptions, invoices, line items, transactions, notes, emails and the activity feed.
- Sales pipelines, stages, opportunities, tasks, leads and trials.
- Saved reports, dashboards, segments and sequences.

The same list also has three write tools: **Write custom attribute values**, **Write tags** and **Write tasks**. Write tags can add, remove, rename or delete a tag, and Write tasks can create, update or delete a task. ChartMogul's page says "read-only access" while its tool list has these three, so treat the server as read-mostly. The agent can still change data in your account. Review its plan before approving any write.

ChartMogul also says its write tools reject input that contains a NUL character and return an `invalid_input` error.

## Tools

The list above comes from ChartMogul's help article. The tool names in it are descriptive labels, and I did not read the real tool names or count from the server.

## Left out

- **The `chartmogul-mcp-server` repository.** ChartMogul's own repository of that name (MIT) is an archived, local server. It runs over stdio with `uv` and needs a `CHARTMOGUL_TOKEN` API token in the environment. It is not the hosted server, and this plugin does not use a pasted token.
- **Skills, rules and agents.** ChartMogul publishes none for this server. I found no plugin or skills repository in its GitHub organization, and none in the Anthropic, Cursor, Devin or OpenAI marketplaces.

## Docs

- ChartMogul MCP server: https://dev.chartmogul.com/docs/chartmogul-mcp-server/
- Help Center article: https://help.chartmogul.com/article/348-chartmogul-mcp-server

## Logo

ChartMogul's icon from `chartmogul.com`, scaled and centred on a white tile. It is ChartMogul's own mark and a trademark of its owner. It is not covered by the MIT licence.

## License

MIT
