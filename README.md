# CureWise Plugin

Marketplace-ready plugin assets for connecting CureWise MCP across supported clients.

## What this repo contains

- `.codex-plugin/plugin.json`: OpenAI/Codex plugin directory manifest.
- `.claude-plugin/plugin.json`: Claude Plugin Directory manifest with `userConfig` for API-key prompt.
- `.agents/plugins/marketplace.json`: OpenAI/Codex marketplace catalog for desktop testing.
- `.mcp.json`: Claude MCP definition using `Authorization: Bearer ${user_config.api_key}`.
- `.claude-plugin/marketplace.json`: Local Claude marketplace for plugin testing.
- `.cursor-plugin/plugin.json`: Cursor marketplace manifest.
- `.cursor-plugin/marketplace.json`: Cursor marketplace manifest list for validator checks.
- `plugin.json`: Portable Agent Plugins manifest at repo root.
- `server.json`: Global MCP Registry descriptor for broader MCP discovery.
- `mcp.json`: Portable MCP config using `Authorization: Bearer ${CUREWISE_API_KEY}`.
- `assets/logo.png`: Cursor marketplace logo.
- `docs/usage-perplexity.md`: Perplexity custom connector and Agent API MCP setup.
- `docs/usage-openai.md`: OpenAI/Codex local setup and public submission guidance.
- `DEVELOPMENT.md`: Secure local development, CI, and credential-manager guidance.
- `connectors/perplexity.json`: Reference Perplexity connector settings (non-authoritative).
- `connectors/openai.json`: Reference OpenAI/Codex connector settings (non-authoritative).
- `skills/curewise/SKILL.md`: Optional helper skill for better in-product guidance.
- `scripts/validate-template.mjs`: Cursor template validator.

## MCP server endpoint

- URL: `https://app.curewise.com/api/mcp`
- Transport: streamable HTTP

## Authentication

Generate a key at:
- `https://app.curewise.com/me/api-keys`

Primary auth flows (secure credential entry only):

- **Claude Code / Claude Plugin Directory**: install the plugin, then enter `api_key` when Claude prompts via `userConfig`. The key is stored in Claude’s secure credential store (not written to disk by this plugin).
- **Cursor**: install the plugin, then set `CUREWISE_API_KEY` from **Settings → Extensions → CureWise** / **Plugins → Configure** (declared in `.cursor-plugin/plugin.json` `variables`). Cursor stores the value in its secure configuration.
- **OpenAI / Codex / Perplexity**: use the client’s connector or plugin settings UI. See `docs/usage-openai.md` and `docs/usage-perplexity.md`.

Do **not** write API keys into shell profiles, `~/.cursor/mcp.json`, or other on-disk config files. CureWise never stores credentials on disk; keys are provided only through host secure prompts or credential injection. For local CI and credential-manager workflows, see `DEVELOPMENT.md`.

Expected header:

```text
Authorization: Bearer cw_...
```

## Platform compatibility summary

- **Cursor**: repository-driven plugin manifests in this repo, API-key auth supported via plugin Configure UI.
- **Claude Code / Claude Plugin Directory**: repository-driven plugin manifests in this repo, API-key auth supported via `userConfig`.
- **OpenAI/Codex**: repo-driven manifests supported; API-key auth for local/developer installs, with OAuth-related review requirements for public submission. See `docs/usage-openai.md`.
- **Perplexity**: custom remote connector (web UI) and MCP tool (Agent API) supported; managed catalog listing depends on broader registry/discovery flow. See `docs/usage-perplexity.md`.

## Claude setup notes

1. Add local marketplace: `claude plugin marketplace add ./curewise-plugin`.
2. Install plugin: `claude plugin install curewise@curewise-local`.
3. Enter `api_key` when prompted by Claude (secure prompt; do not paste keys into local config files).
4. Claude reads `.mcp.json` and injects `${user_config.api_key}` into request headers.
5. Validate plugin manifest: `npx -y @anthropic-ai/claude-code plugin validate --strict .claude-plugin/plugin.json`.

## Cursor setup notes

1. Validate the repo shape: `node scripts/validate-template.mjs`.
2. Install the plugin.
3. Open **Settings → Extensions → CureWise** (or **Plugins → Configure**) and enter `CUREWISE_API_KEY`.
4. Confirm the MCP connection with a read-only tool such as `get_profile`.

## Security and privacy

- Privacy policy: `https://curewise.com/privacy`
- Terms of service: `https://curewise.com/terms`
- Support: `support@curewise.com`

CureWise never stores credentials on disk. This repository never commits real API keys. Use placeholders in committed configs and provide credentials only through host secure prompts (Claude `userConfig`, Cursor plugin Configure) or short-lived injection described in `DEVELOPMENT.md`.

## Validation

Claude:

```bash
npx -y @anthropic-ai/claude-code plugin validate --strict .claude-plugin/plugin.json
```

Cursor:

- Validate JSON manifests.
- Confirm plugin loads from local test directory.
- Verify MCP connection with a test API key entered via plugin Configure.

## Submission

- Cursor marketplace: [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish)
- Claude plugin directory: [clau.de/plugin-directory-submission](https://clau.de/plugin-directory-submission)
- OpenAI/Codex public directory: submit via OpenAI plugin dashboard flow after meeting review prerequisites in `docs/usage-openai.md`.
- Global MCP Registry: publish `server.json` to [registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io) via `mcp-publisher` for ecosystem discovery.
