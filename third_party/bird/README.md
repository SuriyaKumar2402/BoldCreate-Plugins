# Bird

BoldCreate plugin that connects agents to Bird through its hosted [Model Context Protocol](https://modelcontextprotocol.io/) server over streamable HTTP.

Drive Bird from your agent: send and inspect email, SMS, WhatsApp and voice, set up channels and domains, and audit a domain's email authentication.

## Install

- **BoldCreate Connect:** open **Marketplace**, find **Bird**, and click **Install**.
- **Cursor:** add this repository as a team marketplace, then install **Bird**.
- **Claude Code:** `/plugin marketplace add syncfusion/BoldCreate-Plugins`, then `/plugin install bird@boldcreate-plugins`.

## MCP

```json
{
  "mcpServers": {
    "bird": {
      "type": "http",
      "url": "https://mcp.bird.com"
    }
  }
}
```

## Setup

No settings.

The first time the agent calls a tool, each member signs in to Bird in the browser (OAuth) and approves a consent screen. Bird's page says there is no CLI to install and no API key to create.

## Which organization this is

`github.com/messagebird` is the GitHub organization of Bird. Its current repositories call the product Bird (for example `bird-sdk-typescript`, "the official TypeScript SDK for the Bird API"), and its Homebrew tap installs a `bird` command. Older repositories there still use the MessageBird name. The plugin source is [messagebird/bird-ai](https://github.com/messagebird/bird-ai).

## Endpoint

Bird's page names two hosted addresses. Both use streamable HTTP and the same sign-in.

| Address | What the client sees | When Bird says to use it |
| --- | --- | --- |
| `https://mcp.bird.com` | The full tool catalog | Most clients. It also supports MCP Apps widgets. |
| `https://mcp.bird.com/dynamic` | Two tools, `search` and `execute` | Only for clients that have no tool search of their own or that limit how many tools a server may expose. |

This plugin uses `https://mcp.bird.com`. `https://mcp.bird.com/mcp` also answered with a 401 in the check below, but Bird's page does not name it.

## Authentication

A live check on 2026-11-13 found:

- An unauthenticated request returned HTTP 401 with `WWW-Authenticate: Bearer` and a link to the protected-resource metadata at `https://mcp.bird.com/.well-known/oauth-protected-resource`.
- That metadata names `https://platform.bird.com` as the authorization server.
- The authorization server metadata lists a registration endpoint (`/v1/oauth/register`, dynamic client registration) and PKCE with S256. Token endpoint methods are `none`, `client_secret_basic` and `client_secret_post`. The grants are `authorization_code`, `refresh_token` and the device code grant. It does not list client ID metadata documents.
- It advertises 98 scopes, a read and a write scope for each area. The areas include email, SMS, WhatsApp, voice, eSIM, numbers, domains, webhooks, API keys, members and, with an `org:` prefix, billing, settings and access restrictions.

So members connect without a client ID or client secret from your team. No registration request was sent to the server during the check.

Bird's page says:

- The client registers itself, and the consent screen marks it as "not verified by Bird" because the client's name is self-asserted.
- The grant is capped to what the client asked for, what the member approved and what the member actually holds. `org:owner` and platform-admin scopes are never delegable.
- A member can revoke the grant under **Connected apps** in their Bird profile.
- A workspace that enforces mTLS refuses OAuth calls, and the hosted MCP server cannot reach it. Bird's skill text says the member must set up a client certificate for that case.

**Not checked.** The tool list behind sign-in was not read, and Bird's page gives no tool count. Whether Bird accepts BoldCreate Connect's redirect address at registration was not checked.

## What the server can do

Bird's page says the tools cover every channel Bird runs, the setup each channel needs before it can send, email deliverability, and audience and configuration tasks. It says the toolset is curated, and that destructive tools are annotated so a client can ask first.

Some tools are left off the hosted server by design. These are the sign-up tools and the file-upload tools, which need the local `bird` command.

The scopes above show how far an approved grant can reach: sending messages on every channel, buying eSIM data, managing numbers and domains, creating API keys, and, for a member who holds the right role, organization billing and members. Review what the agent plans to do before approving a write, and approve only the scopes you need.

## Skills

The `skills/` folder holds `email-audit`, from [messagebird/bird-ai](https://github.com/messagebird/bird-ai/tree/ffcce97009774ae2709512e86d9d2a5d1dc2831f/plugins/bird/skills/email-audit) at commit `ffcce97009774ae2709512e86d9d2a5d1dc2831f`, copied unchanged. Bird describes it as grading a domain's live DMARC, SPF, DKIM, BIMI and MX records and reading the findings back as a prioritized fix list. The folder's tree hash matches the one GitHub returns for it. A copy of the repository's `LICENSE` (MIT, copyright 2026 Bird) is in `skills/LICENSE`.

What the skill does:

- It names the command `bird email tools audit <domain>` first, and the MCP tool `email_tools_audit` second. Bird's text says the audit reads DNS only, needs no sign-in, sends no mail and inspects no message.
- It is a plain `SKILL.md` with no scripts, installs or outside links.
- **Not checked.** I could not confirm that the hosted server exposes `email_tools_audit`, because I did not read the tool list. Bird's `llms.txt` does not mention it. Without that tool the skill still explains how to read DMARC, SPF, DKIM, BIMI and MX records, but the agent would need another way to look them up.

**Left out.**

- The `bird-cli` skill. It has 95 files and runs through the `bird` command-line program, not through this connection. Its text says it requires the CLI and that the agent should install it if `command -v bird` finds nothing. The CLI signs in separately with `bird auth login`. This plugin does not install programs.
- Bird's README quick start, which installs the CLI with a `curl ... | sh` line and describes a sign-up flow that creates an account from the terminal.
- The manifests and marketplace files for other clients, the OpenCode and Antigravity files, and the `antigravity.sh` script.

## Docs

- MCP server: https://bird.com/docs/ai/mcp-server
- Bird documentation: https://bird.com/docs
- Plugin source: https://github.com/messagebird/bird-ai

## Logo

Bird's mark from the `assets/logo.png` file in [messagebird/bird-ai](https://github.com/messagebird/bird-ai), scaled and centred on a white tile. It is a white bird on a black square, as Bird publishes it, and `bird.com` serves the same mark as its favicon. It is Bird's own mark and a trademark of its owner. It is not covered by the MIT licence.

## License

MIT
