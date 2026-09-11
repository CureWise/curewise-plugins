# CureWise OpenAI and Codex Usage

This guide covers local OpenAI/Codex usage from this repository and the path to
public marketplace submission.

## What works now

- Local and developer-mode installs from this repository.
- API-key authentication via `CUREWISE_API_KEY` for local plugin installs.
- OAuth 2.1 connector flow for OpenAI/Claude custom connector surfaces.
- MCP endpoint: `https://app.curewise.com/api/mcp`.

This repository includes:

- `.codex-plugin/plugin.json` (OpenAI/Codex directory manifest)
- `.agents/plugins/marketplace.json` (repo marketplace catalog)
- `mcp.json` (portable MCP config using `streamable-http`)

OAuth discovery + connector endpoints now available in CureWise webapp:

- `/.well-known/oauth-authorization-server`
- `/.well-known/oauth-protected-resource`
- `/oauth/authorize`
- `/api/oauth/register`
- `/api/oauth/token`

## Option 1: Codex CLI (local install)

1. Export your CureWise key:

```bash
export CUREWISE_API_KEY="cw_your_key_here"
```

2. Add this repository as a plugin marketplace:

```bash
codex plugin marketplace add https://github.com/curewise/curewise-plugin.git
```

3. Install the plugin:

```bash
codex plugin install curewise
```

4. Verify with a read-only tool first (`get_profile`, `list_documents`).

## Option 2: ChatGPT desktop app (developer mode)

1. In ChatGPT desktop, open settings and enable developer mode.
2. Open the plugins area and add this repository marketplace or MCP connector.
3. Set `CUREWISE_API_KEY` for the plugin connection.
4. Connect and run a read-only tool before any write operations.

## Option 3: OpenAI ChatGPT web custom connector (OAuth)

1. In ChatGPT workspace/admin connector setup, choose OAuth authentication.
2. Use MCP server URL: `https://app.curewise.com/api/mcp`.
3. ChatGPT reads the `WWW-Authenticate` `resource_metadata` pointer from MCP
   401 responses and discovers OAuth endpoints from:
   - `https://app.curewise.com/.well-known/oauth-protected-resource`
   - `https://app.curewise.com/.well-known/oauth-authorization-server`
4. Complete OAuth consent and continue with a read-only tool test first.

## Claude custom connector note

The same MCP OAuth discovery flow can be used by `claude.ai` custom connector
surfaces. Claude Code plugin installs can continue using the existing
`.claude-plugin/plugin.json` + `.mcp.json` API-key `userConfig` flow.

## Public marketplace submission requirements

Public listing in the shared ChatGPT/Codex directory needs OpenAI review and
additional requirements beyond repo packaging:

- Verified developer/business identity in the OpenAI platform.
- Production HTTPS MCP URL and successful domain verification challenge.
- Required tool annotations on every MCP tool (`readOnlyHint`,
  `openWorldHint`, `destructiveHint`) with justifications.
- Review assets (test cases, release notes, and other submission materials).
- Reviewer-ready demo credentials for OpenAI submission/review.

## Plan gating note

Availability can vary by plan/workspace policy. In workspace setups, an admin
may need to enable custom/developer connector access.

## PHI and compliance note

Use policy and compliance review remains required before exposing PHI-capable
workflows in any public marketplace listing.
