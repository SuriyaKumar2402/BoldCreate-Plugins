# Remote

BoldCreate plugin that connects agents to Remote through its hosted [Model Context Protocol](https://modelcontextprotocol.io/) server over streamable HTTP.

Access Remote's global employment platform from your agent: employments, contracts, payroll, time off, org structure and reports.

## Install

- **BoldCreate Connect:** open **Marketplace**, find **Remote**, and click **Install**.
- **Cursor:** add this repository as a team marketplace, then install **Remote**.
- **Claude Code:** `/plugin marketplace add syncfusion/BoldCreate-Plugins`, then `/plugin install remote@boldcreate-plugins`.

## MCP

```json
{
  "mcpServers": {
    "remote": {
      "type": "http",
      "url": "https://mcp.remote.com/mcp"
    }
  }
}
```

## Setup

No settings.

The first time the agent calls a tool, each member signs in to their own Remote account in the browser (OAuth). Remote's page says there are no API keys, client secrets or scopes to set up. You need a Remote account that can reach the data you want.

## Authentication

A live check on 2026-11-13 found:

- An unauthenticated request to `/mcp` returned HTTP 401 with `WWW-Authenticate: Bearer` and a link to the protected-resource metadata. That metadata is served at `/.well-known/oauth-protected-resource/mcp`; the root path returns 404.
- The metadata names `https://api.employ.remote.com` as the authorization server and lists six scopes: `mcp:remote_admin:read` and `:write`, `mcp:employer:read` and `:write`, and `mcp:employee:read` and `:write`.
- The authorization server metadata lists a registration endpoint (`/oauth/register`, dynamic client registration), client ID metadata documents, and PKCE with S256.
- Token endpoint methods are `none`, `client_secret_basic`, `client_secret_post` and `private_key_jwt`.

So members connect without a client ID or client secret from your team. No registration request was sent to the server during the check.

Remote's page says it tested Claude Desktop, claude.ai, Claude Code and Cursor, and that other clients should work if they support OAuth 2.0 with dynamic client registration over streamable HTTP. BoldCreate Connect is not on that list, so whether Connect's redirect address is accepted was not checked.

**Not checked.** The tool list behind sign-in was not read.

## What it can reach

From Remote's page, Remote.com MCP is for employer users (company admins and managers) and for employees (EOR employees and contractors), and each member sees only the tools their Remote role allows. It lists these areas:

- Read tools: employees and employment records, contracts and amendments, payroll runs and pay schedules, time off and leave balances, org structure, legal entities, timesheets, billing and invoices.
- Write tools, described as employee self-service: submit time off requests, update personal details, home address and emergency contact, and create and update career goals and journal notes.

Remote says writes follow the same approval and audit flows as in the product, so a time off request still goes to the manager. The server can change real HR records. Review the agent's plan before you approve a write.

## Other addresses

Remote's pages name only `https://mcp.remote.com/mcp`. Two other addresses also answered with a sign-in request on 2026-11-13, and this plugin does not use them:

- `https://api.remote.com/mcp` points to `https://api.employ.remote.com/mcp` as its resource.
- `https://gateway.remote.com/mcp` has its own authorization server, with the scope `all:write`. I found no Remote page that describes it.

A third-party repository also names a sandbox address, `https://mcp.remote-sandbox.com/mcp`, which answered 401 with a sign-in link. I found it on no Remote page.

## Skills

The `skills/` folder holds 7 of the 8 agent skills published in [remoteoss/remote-for-ai](https://github.com/remoteoss/remote-for-ai/tree/4e241875984e0f512f4f80464a0449612acf6e45/skills) at commit `4e241875984e0f512f4f80464a0449612acf6e45`, copied unchanged. The source is licensed under MIT, copyright Remote Technology, Inc. The original licence text is in `skills/LICENSE`. The folder hashes and the licence file were compared with the values GitHub returns for that commit.

| Skill | What it is for |
| --- | --- |
| `remote-payroll-and-payslips` | Payslips, salary history and payroll breakdowns, using the MCP tools |
| `remote-reporting` | Creating and downloading reports for employers |
| `remote-time-off-workflow` | Submitting, approving, declining and cancelling time off, balances and holidays |
| `remote-api-auth` | Getting access tokens for the Remote REST API |
| `remote-api-forms` | Building request bodies for the REST API's form endpoints |
| `remote-api-integration` | Finding and calling REST API endpoints |
| `remote-api-webhooks` | Receiving and verifying Remote webhooks |

Only the first three work with the MCP tools. The four `remote-api-*` skills are for people building their own integration against the REST API at `gateway.remote.com`. Their own text says they are not for operating a workspace through the MCP, and they cover API tokens and partner OAuth flows. They are the plugin's own skills, so they are included, but they do not use this plugin's connection.

**What the skills do.** They have no scripts and no install commands. One line in `remote-api-forms` suggests using Remote's open-source `@remoteoss/json-schema-form` library for form screens.

**Left out.**

- The skill `remote-api-onboarding`. Its `description` line in `SKILL.md` is not valid YAML, because an unquoted value contains a colon and a space ("(hire/onboard): the basic-information ..."). This repository's checks require valid frontmatter, and imported files are not edited. It covers hiring through the REST API, so the three MCP skills do not depend on it. `remote-api-forms` and `remote-api-integration` still name it in their text (for example "Running the full hire sequence -> `remote-api-onboarding`"), so those two references point to a skill that is not installed.
- The repository's other manifests (Claude Code, Codex, Gemini and Cursor), its marketplace files and its `AGENTS.md`, `GEMINI.md` and `CONTRIBUTING.md`. The org's other repositories are also not used: `ai-agent-toolkit` runs a local MCP server with a Remote API key, and `remote-cli` installs a command-line program with a `curl | sh` line.

## Docs

- Introduction to Remote MCP: https://developer.remote.com/docs/introduction-to-remote-mcp
- Remote agent skills: https://developer.remote.com/docs/remote-agent-skills
- Plugin source: https://github.com/remoteoss/remote-for-ai

## Logo

The mark from `assets/logo.png` in Remote's plugin repository, centred on a white tile. It is Remote's own mark and a trademark of its owner. It is not covered by the MIT licence.

## License

MIT
