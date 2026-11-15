# Brightcove

BoldCreate plugin that connects agents to Brightcove through its hosted [Model Context Protocol](https://modelcontextprotocol.io/) server over streamable HTTP.

Connect AI assistants to your Brightcove account to work with videos, players, live streams, analytics and other resources your account is entitled to.

## Install

- **BoldCreate Connect:** open **Marketplace**, find **Brightcove**, and click **Install**.
- **Cursor:** add this repository as a team marketplace, then install **Brightcove**.
- **Claude Code:** `/plugin marketplace add syncfusion/BoldCreate-Plugins`, then `/plugin install brightcove@boldcreate-plugins`.

## MCP

```json
{
  "mcpServers": {
    "brightcove": {
      "type": "http",
      "url": "https://mcp.brightcove.com/mcp"
    }
  }
}
```

## Setup

No settings.

The first time the agent calls a tool, each member signs in to their own Brightcove account in the browser (OAuth) and approves the connection.

## Before you connect

Brightcove's page says:

- The MCP server must be enabled for the account. If the connection is rejected, ask your account administrator or Brightcove Support to enable it.
- The signed-in user needs the permissions for the operations the assistant runs. For accounts that use Roles and Permissions, the user also needs the MCP access role.
- If the connection stops working after a while, the access token has expired. Reconnect and sign in again.

## Which organization this is

`github.com/BrightcoveLearning` holds Brightcove's documentation sources and code samples. It has no MCP server and no plugin. The hosted server is documented by Brightcove at [Brightcove MCP Server](https://apis.support.brightcove.com/general/brightcove-mcp-server.html). I found no Brightcove repository with agent skills or a plugin, in this organization or in `github.com/brightcove`, which holds the player SDKs.

## Authentication

A live check on 2026-11-15 found:

- An unauthenticated request to `/mcp` returned HTTP 401 with `WWW-Authenticate: Bearer` and a link to the protected-resource metadata. That metadata is served at `/.well-known/oauth-protected-resource/mcp`, not at the root of the host. It names `https://oauth.brightcove.com` as the authorization server and lists no scopes.
- The authorization server metadata lists a registration endpoint (`https://oauth.brightcove.com/connect/register`, dynamic client registration) and PKCE with S256. Its token endpoint methods include `none`, so a client can register without a secret.
- A plain GET to the registration address returned a redirect (302). No registration request was sent, so I could not confirm that it accepts a registration.
- `GET /mcp` returned HTTP 405 and the server's `Allow` header lists only `POST`.

Brightcove's page says any MCP client that supports remote servers with OAuth can connect, and names Claude, ChatGPT and Claude Code as common clients. BoldCreate Connect is not named, so whether Connect's redirect address is accepted was not checked.

## Tools

Brightcove's page says the tools depend on the account and on the user's permissions. It names these areas:

| Area | Examples from Brightcove's page |
| --- | --- |
| Content and CMS | Search, create and update videos, playlists, folders and labels. |
| Ingest | Create ingest jobs and manage captions, audio tracks and metadata. |
| Players | Create and update players and embeds. |
| Live | Create and manage live jobs and schedules. |
| Analytics | Query video and account analytics. |
| Playback and delivery | Playback authorization, playback rights and delivery rules. |
| Engagement | Gallery, Interactivity, Audience and Social. |
| Cloud Playout and SSAI | Channels, schedules and server-side ad insertion. |

Brightcove says tools that change or delete data are marked as destructive, so the assistant asks for confirmation first. Review the plan before approving. The tools can create and change players, live jobs and delivery rules, and can delete videos and other content.

Brightcove's page gives no tool count, and the tool list is behind sign-in, so it was not read.

Brightcove also says that if a client reports too many tools, it should use dynamic tool loading. This plugin does not set that up.

**Not checked.** The tool list behind sign-in, whether the registration endpoint accepts a registration, and which redirect addresses are allowed.

## Skills

None. The `skills/` folder is not part of this plugin, because Brightcove publishes no skills that I could find.

## Docs

- Brightcove MCP Server: https://apis.support.brightcove.com/general/brightcove-mcp-server.html
- Brightcove support: https://support.brightcove.com

## Logo

Brightcove's `android-chrome-512x512.png` icon from `apis.support.brightcove.com`, scaled and centred on a white tile. It is a white letter on a dark square, as Brightcove publishes it. It is Brightcove's own mark and a trademark of its owner. It is not covered by the MIT licence. The favicon at `www.brightcove.com/favicon.ico` is not a valid icon file, so it was not used. I did not view the logo.

## License

MIT
