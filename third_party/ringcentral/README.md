# RingCentral

BoldCreate plugin that connects agents to RingCentral through its hosted [Model Context Protocol](https://modelcontextprotocol.io/) servers for RingEX Phone and Team Chat.

Work with RingCentral phone and Team Chat: review calls, voicemail, SMS, and faxes; find colleagues; and manage messages, tasks, notes, events, teams, webhooks, and Adaptive Cards.

## Install

- **BoldCreate Connect:** open **Marketplace**, find **RingCentral**, and click **Install**.
- **Cursor:** add this repository as a team marketplace, then install **RingCentral**.
- **Claude Code:** `/plugin marketplace add syncfusion/BoldCreate-Plugins`, then `/plugin install ringcentral@boldcreate-plugins`.

## MCP

```json
{
  "mcpServers": {
    "ringcentral-phone": {
      "type": "http",
      "url": "https://mcp.labs.ringcentral.com/ringex/phone"
    },
    "ringcentral-team-chat": {
      "type": "http",
      "url": "https://mcp.labs.ringcentral.com/ringex/team-chat"
    }
  }
}
```

## Setup

No settings.

The first time the agent calls a tool on each server, the member signs in to their own RingCentral account in the browser (OAuth). There are two servers, so a member can be asked to sign in twice.

## Servers

RingCentral's MCP documentation lists these servers, all on `mcp.labs.ringcentral.com`:

| Server | Address | Status in RingCentral's docs | Tools in its docs | In this plugin |
| --- | --- | --- | --- | --- |
| RingEX Phone | `/ringex/phone` | Preview | 11 | Yes |
| RingEX Chat | `/ringex/team-chat` | Preview | 8 | Yes |
| RingEX Meetings | `/ringex/video` | Experimental | 7 | No |
| RingEX Admin | `/ringex/admin` | Experimental | 26 (its changelog says 31) | No |
| App Connect | `unified-crm-extension.labs.ringcentral.com/mcp` | Beta | 9 | No |

This plugin installs the two Preview servers, because the 16 skills use only phone and Team Chat. The Meetings and Admin servers are experimental, and Admin can change users, roles, call routing and emergency-call settings across an account. App Connect is a CRM connector and is a separate product. All three are left out. The host name contains `labs`, so RingCentral may change these servers.

## Authentication

A live check on 2026-11-13 found, for the Phone and Team Chat servers:

- An unauthenticated request returned HTTP 401 with `WWW-Authenticate: Bearer` and a link to the server's own protected-resource metadata.
- Each server is its own authorization server. Its metadata lists a registration endpoint (dynamic client registration) at `/oauth/register` under the server path, PKCE with S256, and no client secret (`none`).
- Phone asks for the scopes `ReadAccounts`, `ReadContacts`, `ReadCallLog`, `AI`, `ReadMessages`, `ReadPresence` and `SMS`. Team Chat asks for `ReadAccounts` and `TeamMessaging`.

So members connect without a client ID or client secret from your team. No registration request was sent to the server during the check.

**Not checked.** Tool lists behind sign-in were not read, so the tool counts above come from RingCentral's documentation. RingCentral's docs describe the transport as "SSE over HTTPS", while its plugin manifest says streamable HTTP. Only the 401 answer was checked, so which transport the servers use after sign-in was not tested. Whether RingCentral accepts BoldCreate Connect's redirect address was not tested.

## Skills

The `skills/` folder holds the 16 agent skills published in [ringcentral/ringex-claude-plugin](https://github.com/ringcentral/ringex-claude-plugin/tree/7e44d617023d415e377d684e00abc778e6336f07/skills) at commit `7e44d617023d415e377d684e00abc778e6336f07`, copied unchanged. The source is licensed under MIT, copyright 2026 RingCentral, Inc. A copy of its `LICENSE` is in `skills/LICENSE`. The folder hashes were compared with the ones GitHub returns for that commit.

| Group | Skills |
| --- | --- |
| Phone and messaging | `call-followup-email`, `call-recap`, `voicemail-inbox`, `sms-inbox`, `send-sms`, `fax-inbox`, `communications-brief` |
| Team Chat | `post-to-chat`, `read-team-chat`, `manage-teams`, `manage-tasks`, `manage-notes`, `manage-events`, `manage-webhooks`, `manage-adaptive-cards` |
| Directory | `colleague-lookup` |

Each skill is one `SKILL.md` file. There are no scripts, no install commands and no outside links. RingCentral's skill text says writes need a confirmation first. For example, `send-sms` says to preview the exact message and wait for an explicit yes.

**Where the skills come from.** RingCentral also keeps a copy of its skills and plugin files in [ringcentral/ringcentral-mcp-docs](https://github.com/ringcentral/ringcentral-mcp-docs). That repository's README says "All rights reserved" for the documentation site, so nothing from it is used. This plugin uses the standalone plugin repository above.

**Left out.** The plugin repository's other files: the `.claude-plugin` and `.agents` marketplace files, `plugin-manifest.json`, `mcp-manifest.json`, the build scripts and the pre-commit hook.

## What the servers can change

The skills and the servers can send SMS messages, post to Team Chat, create or delete teams, tasks, notes, events and webhooks, and post Adaptive Cards. Review the agent's plan before approving a write. The plugin's skills ask for confirmation first, but a different setup may not.

## Docs

- RingCentral MCP skills: https://developers.ringcentral.com/mcp/skills/
- RingCentral MCP documentation source: https://github.com/ringcentral/ringcentral-mcp-docs
- Plugin source: https://github.com/ringcentral/ringex-claude-plugin

## Logo

The RingCentral app icon from RingCentral's plugin repository, scaled onto a white tile. It is RingCentral's own mark and a trademark of its owner. It is not covered by the MIT licence. I could not view the rendered logo.

## License

MIT
