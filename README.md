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

Primary auth flows:

- **Cursor**: install plugin, then set `CUREWISE_API_KEY` from **Plugins -> Configure** (declared in `.cursor-plugin/plugin.json` `variables`).
- **Claude Code**: install plugin, then enter `api_key` from the enable-time `userConfig` prompt.

## Platform compatibility summary

- **Cursor**: repository-driven plugin manifests in this repo, API-key auth supported.
- **Claude Code / Claude Plugin Directory**: repository-driven plugin manifests in this repo, API-key auth supported via `userConfig`.
- **OpenAI/Codex**: repo-driven manifests supported; API-key auth for local/developer installs, with OAuth-related review requirements for public submission. See `docs/usage-openai.md`.
- **Perplexity**: custom remote connector (web UI) and MCP tool (Agent API) supported; managed catalog listing depends on broader registry/discovery flow. See `docs/usage-perplexity.md`.

Fallback (local development / non-dashboard installs):

```bash
echo 'export CUREWISE_API_KEY="cw_your_key_here"' >> ~/.zshrc
source ~/.zshrc
```

Expected header:

```text
Authorization: Bearer cw_...
```

Important: for env-var fallback, the variable must be present in the shell/session used to launch the client process.

## Claude setup notes

1. Add local marketplace: `claude plugin marketplace add ./curewise-plugin`.
2. Install plugin: `claude plugin install curewise@curewise-local`.
3. Enter `api_key` when prompted by Claude.
4. Claude reads `.mcp.json` and injects `${user_config.api_key}` into request headers.
5. Validate plugin manifest: `npx -y @anthropic-ai/claude-code plugin validate --strict .claude-plugin/plugin.json`.

## Cursor setup notes

1. Validate the repo shape: `node scripts/validate-template.mjs`.
2. Install the plugin, then set `CUREWISE_API_KEY` in **Plugins -> Configure**.
3. If your install path does not expose plugin Configure, use env-var fallback (`export CUREWISE_API_KEY=...`) or set a literal key in `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "curewise": {
      "type": "streamable-http",
      "url": "https://app.curewise.com/api/mcp",
      "headers": {
        "Authorization": "Bearer cw_your_key_here"
      }
    }
  }
}
```

## Security and privacy

- Privacy policy: `https://curewise.com/privacy`
- Terms of service: `https://curewise.com/terms`
- Support: `support@curewise.com`

This repository never stores real API keys. Use placeholders in committed configs and provide credentials only in runtime settings.

## Validation

Claude:

```bash
npx -y @anthropic-ai/claude-code plugin validate --strict .claude-plugin/plugin.json
```

Cursor:

- Validate JSON manifests.
- Confirm plugin loads from local test directory.
- Verify MCP connection with a test API key.

## Submission

- Cursor marketplace: [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish)
- Claude plugin directory: [clau.de/plugin-directory-submission](https://clau.de/plugin-directory-submission)
- OpenAI/Codex public directory: submit via OpenAI plugin dashboard flow after meeting review prerequisites in `docs/usage-openai.md`.
- Global MCP Registry: publish `server.json` to [registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io) via `mcp-publisher` for ecosystem discovery.
