# Expensify

BoldCreate plugin that connects agents to Expensify through its hosted [Model Context Protocol](https://modelcontextprotocol.io/) server over streamable HTTP.

Connect agents to Expensify to analyze spending, compare periods, and get plain-English summaries of expense data.

## Install

- **BoldCreate Connect:** open **Marketplace**, find **Expensify**, and click **Install**.
- **Cursor:** add this repository as a team marketplace, then install **Expensify**.
- **Claude Code:** `/plugin marketplace add syncfusion/BoldCreate-Plugins`, then `/plugin install expensify@boldcreate-plugins`.

## MCP

```json
{
  "mcpServers": {
    "expensify": {
      "type": "http",
      "url": "https://www.expensify.com/mcp"
    }
  }
}
```

## Setup

No settings.

The first time the agent calls a tool, each member signs in to Expensify in the browser (OAuth) and approves access. Expensify's plugin page says the data is limited to the account and workspaces the member authorizes.

## Authentication

A live check on 2026-11-15 found:

- A request to `https://www.expensify.com/mcp` is redirected (HTTP 308) to the same address with a trailing slash. The server then returned HTTP 401 with `WWW-Authenticate: Bearer`, a link to the protected-resource metadata and the scope `mcp:tools`. `https://mcp.expensify.com/mcp` redirects the same way and points to the same metadata.
- The authorization server metadata at `https://www.expensify.com` lists a registration endpoint (`/oauth/register`, dynamic client registration), PKCE with S256, and the scopes `mcp:tools` and `integrations:api`. Token endpoint methods are `none` and `client_secret_post`.
- No registration request was sent to the server.

**Not checked.** The tool list behind sign-in was not read. Expensify's own help page for the server returned 404, so the only Expensify description is the plugin repository. Whether Expensify accepts BoldCreate Connect's redirect address was not checked. The repository names Claude only.

## Skills

The `skills/` folder holds the 2 skills in [Expensify/expensify-plugin](https://github.com/Expensify/expensify-plugin/tree/b8dfd6cd2036028747a5afa4237b2b37c2b89886/skills) at commit `b8dfd6cd2036028747a5afa4237b2b37c2b89886`, copied unchanged. The source is licensed under MIT, copyright 2026 Expensify, Inc. The original licence is in `skills/LICENSE`. The folder hashes and the licence were compared with the ones GitHub returns for that commit.

| Skill | What it does |
| --- | --- |
| `spend-brief` | Writes a plain-English spend summary for a time period, by total, category, person and merchant, with flags for unusual items and next steps. |
| `flux-analysis` | Compares spending across two periods and explains the biggest changes by category and merchant. |

Both skills read data only. They call the server's `GetCurrentUser`, `GetWorkspaceList` and `Search` tools. They have no scripts, installs or outside links.

**Left out.** The Claude marketplace and plugin manifests and the icon file in the same repository. The icon was used for the logo only.

## Docs

- Plugin source: https://github.com/Expensify/expensify-plugin
- Expensify privacy policy: https://www.expensify.com/privacy

## Logo

The `icon.png` file from [Expensify/expensify-plugin](https://github.com/Expensify/expensify-plugin), centred on a white tile. It is Expensify's own icon and a trademark of its owner. It is not covered by the MIT licence. I could not view the logo.

## License

MIT
