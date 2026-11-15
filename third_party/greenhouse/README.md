# Greenhouse

BoldCreate plugin that connects agents to Greenhouse through its hosted [Model Context Protocol](https://modelcontextprotocol.io/) server over streamable HTTP.

Connect AI tools to Greenhouse Recruiting to work with live recruiting data: surface insights, run analyses, answer questions and complete supported actions.

## Install

- **BoldCreate Connect:** open **Marketplace**, find **Greenhouse**, and click **Install**.
- **Cursor:** add this repository as a team marketplace, then install **Greenhouse**.
- **Claude Code:** `/plugin marketplace add syncfusion/BoldCreate-Plugins`, then `/plugin install greenhouse@boldcreate-plugins`.

## MCP

```json
{
  "mcpServers": {
    "greenhouse": {
      "type": "http",
      "url": "https://mcp.greenhouse.io/mcp"
    }
  }
}
```

## Before you connect

Greenhouse's support page for the server says:

- Greenhouse MCP is in **open beta**. It is available on Greenhouse Recruiting Core, Plus and Pro. Organizations on a legacy tier should contact their Greenhouse account team.
- A Site Admin starts the connection in the AI tool and enables the scopes during setup. Admins manage the scopes under **Dev Center > MCP Access**. The scopes set an upper limit that members cannot go beyond.
- Members sign in with the email address tied to their Greenhouse Recruiting account.
- The redirect URL of the AI tool may need to be allowlisted by Greenhouse. The page lists Claude, ChatGPT, Google Antigravity, Glean, Copilot Studio, Amazon Q and Grok AI as supported tools. It does not list BoldCreate Connect, so whether Connect's redirect URL is accepted was not checked.
- If scopes change later, members are signed out and must reconnect.

Greenhouse's skills repository also documents a sign-in message, "Only organization administrators can authorize MCP applications." It says non-admin authorization is switched on by Greenhouse's account team, not by a Site Admin.

## Setup

No settings.

The first time the agent calls a tool, each member signs in to Greenhouse in the browser (OAuth). Access follows the member's own Greenhouse permissions and the scopes the admin enabled.

## Authentication

A live check on 2026-11-13 found:

- An unauthenticated request to `/mcp` returned HTTP 401 with `WWW-Authenticate: Bearer` and a link to the protected-resource metadata. The body says to perform OAuth through the authorization server named there.
- The metadata names `https://auth.greenhouse.io` as the authorization server, with the scope `greenhouse:mcp_client`. The authorization server metadata lists the scopes `openid`, `profile` and `email`.
- The authorization server metadata lists a registration endpoint (`https://auth.greenhouse.io/register`, dynamic client registration), PKCE with S256, and the `authorization_code` and `refresh_token` grants. Token endpoint methods are `none`, `client_secret_basic`, `client_secret_post` and `private_key_jwt`.

**Not checked.** No registration request was sent. A plain GET to the registration address returned HTTP 302. A third-party project's notes name `https://mcp.greenhouse.io/oauth/register` as the registration address instead, and a GET to that address returned 404. So it was not confirmed which address accepts registrations, or whether Greenhouse accepts BoldCreate Connect's redirect URL.

## Tools

Greenhouse's page says the server supports both read and write actions, and that what is available depends on the supported capabilities, the scopes the organization enabled and the member's permissions. Sources disagree on how many tools the server has, and the tool list behind sign-in was not read, so no count is given here.

Review what the agent plans before you approve any write.

## Other addresses

Greenhouse's own skills documentation mentions a second address, `https://mcp.us.greenhouse.io/mcp`. It answered an unauthenticated request with HTTP 401 and an OAuth link. This plugin does not use it. The documentation warns that having both connected shows every tool twice, so connect only one.

## Skills

This plugin has no skills. Greenhouse publishes nine recruiter skills in [grnhse/greenhouse-skills](https://github.com/grnhse/greenhouse-skills). They are not included, because their licence does not allow it.

The Greenhouse Skills License says the skills are proprietary and for Greenhouse customers' internal hiring only. It says no one may copy, publish or distribute them to anyone other than authorized users, including by posting them to a public or shared repository or marketplace. It also says it grants no right to use Greenhouse's names or logos.

A Greenhouse customer can install them from Greenhouse's repository under that licence.

## Docs

- Greenhouse MCP: https://support.greenhouse.io/hc/en-us/articles/52090956642075-Greenhouse-MCP
- Set up Greenhouse MCP with supported AI tools: https://support.greenhouse.io/hc/en-us/articles/52096319906971
- Greenhouse MCP security FAQ: https://support.greenhouse.io/hc/en-us/articles/53944054829083-Greenhouse-MCP-security-FAQ

I could not read Greenhouse's developer page for MCP, which asks for a sign-in. I did not read the security FAQ.

## Logo

Greenhouse's own icon from `greenhouse.com`, scaled onto a white tile. It is Greenhouse's mark and a trademark of its owner. It is not covered by the MIT licence. I did not view the logo.

## License

MIT. This covers the files in this folder only. It does not cover Greenhouse's skills, marks or service.
