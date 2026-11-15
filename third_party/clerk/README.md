# Clerk

BoldCreate plugin that connects agents to Clerk through its hosted [Model Context Protocol](https://modelcontextprotocol.io/) server over streamable HTTP.

Connect agents to Clerk to manage applications, instance settings, users, organizations and domains from your Clerk workspace.

## Install

- **BoldCreate Connect:** open **Marketplace**, find **Clerk**, and click **Install**.
- **Cursor:** add this repository as a team marketplace, then install **Clerk**.
- **Claude Code:** `/plugin marketplace add syncfusion/BoldCreate-Plugins`, then `/plugin install clerk@boldcreate-plugins`.

## MCP

```json
{
  "mcpServers": {
    "clerk": {
      "type": "http",
      "url": "https://mcp.clerk.com/mcp"
    }
  }
}
```

## Setup

No settings.

The first time the agent calls a tool, each member signs in with the Clerk account they use for the Clerk Dashboard, selects a workspace, reviews the permissions and approves. Clerk's page says the server acts as that member, so it can only do what the member's workspace role allows.

## Permissions

Clerk's page lists these scopes.

| Scope | Allows the server to |
| --- | --- |
| `user:org:read` | Identify the selected workspace |
| `applications:read` | Read the applications in that workspace |
| `applications:manage` | Create applications and production instances, and read and change instance configuration, such as OAuth credentials |
| `users:read` | List the users of an instance |
| `users:manage` | Create users, update their profiles and metadata, and set whether they bypass Device Trust |
| `organizations:read` | List organizations, and list memberships when `users:read` is also granted |
| `application_domains:read` | Read domains, the DNS records they need, and their verification status |
| `application_domains:manage` | Start a DNS check for a domain |

Clerk marks the tools that make changes, so an agent can ask first. Creating a production instance cannot be undone, and Clerk's tool needs `confirm: true` for it. Changing instance configuration offers a dry run.

## What Clerk receives

Clerk's page says the server sends Clerk an analytics event for each tool call. It holds the tool name, whether the call worked, the error type, the member's Clerk user ID, and the client's name, version and OAuth client ID. It does not include other tool inputs or results. The `submit_feedback` tool sends a report to the Clerk team when an agent uses it.

## Authentication

A live check on 2026-11-15 found:

- An unauthenticated request to `/mcp` returned HTTP 401 with `WWW-Authenticate: Bearer`, a link to the protected-resource metadata and the scope list above.
- The metadata names `https://clerk.clerk.com` as the authorization server. Its metadata lists a registration endpoint (`/oauth/register`, dynamic client registration), PKCE with S256, and support for client ID metadata documents.
- No registration request was sent to the server.

**Not checked.** The tool list behind sign-in was not read. Whether Clerk accepts BoldCreate Connect's redirect address was not checked. Clerk's page names Claude, Codex, Cursor, VS Code, Windsurf and Zed.

## Not included

- **Skills.** Clerk publishes 20 skills in [clerk/skills](https://github.com/clerk/skills). They were left out. Most say to install the Clerk CLI or SDK packages with `npx` or a package manager, which this repository does not import. The repository also has no licence file, only an MIT line in its README and in each skill's header, so there is no licence text to keep beside the files.
- **The Clerk plugin for Cursor** ([clerk/cursor-plugin](https://github.com/clerk/cursor-plugin)). It loads the same skills through a Git submodule, and it sets an `X-Clerk-MCP-Consumer` header that this plugin does not.

## Docs

- Clerk MCP server: https://clerk.com/docs/guides/ai/mcp/clerk-mcp-server

## Logo

The `assets/clerk-logo.svg` file from [clerk/skills](https://github.com/clerk/skills), centred on a white tile. The source mark is white on a dark square, so the tile shows a dark square. It is Clerk's own logo and a trademark of its owner. It is not covered by the MIT licence. I could not view the logo.

## License

MIT
