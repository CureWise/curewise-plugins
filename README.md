# CureWise Plugin

Marketplace-ready plugin assets for connecting CureWise MCP across supported clients.

## AI model disclosure

Some CureWise tools do not answer with Claude. The `chat` tool and Cross Check (`ask_cross_check`) send your request to third-party AI models that write the answer. Those models are operated by OpenAI, Anthropic, and Google; CureWise routes your request to them and returns their output. The other tools (profile, clinical trials, documents, health records) read/write data in your CureWise account and do not generate answers with another model.

## What this repo contains

- `.codex-plugin/plugin.json`: OpenAI/Codex plugin directory manifest.
- `.claude-plugin/plugin.json`: Claude Plugin Directory manifest with `userConfig` for API-key prompt.
- `.agents/plugins/marketplace.json`: OpenAI/Codex marketplace catalog for desktop testing.
- `.mcp.json`: Claude MCP definition using `Authorization: Bearer ${user_config.api_key}`.
- `.claude-plugin/marketplace.json`: Local Claude marketplace for plugin testing.
- `plugin.json`: Portable Agent Plugins manifest at repo root.
- `server.json`: Global MCP Registry descriptor for broader MCP discovery.
- `mcp.json`: Portable MCP config sample using `Authorization: Bearer YOUR_API_KEY` placeholder.
- `docs/usage-perplexity.md`: Perplexity custom connector and Agent API MCP setup.
- `docs/usage-openai.md`: OpenAI/Codex local setup and public submission guidance.
- `DEVELOPMENT.md`: Local development and validation guidance (secure credential entry only).
- `connectors/perplexity.json`: Reference Perplexity connector settings (non-authoritative).
- `connectors/openai.json`: Reference OpenAI/Codex connector settings (non-authoritative).
- `skills/curewise/SKILL.md`: Optional helper skill for better in-product guidance.

## MCP server endpoint

- URL: `https://app.curewise.com/api/mcp`
- Transport: streamable HTTP

## Authentication

Generate a key at:
- `https://app.curewise.com/me/api-keys`

Primary auth flows (secure credential entry only):

- **Claude Code / Claude Plugin Directory**: install the plugin, then enter `api_key` when Claude prompts via `userConfig`. The key is stored in Claude’s secure credential store (not written to disk by this plugin).
- **OpenAI / Codex / Perplexity**: use the client’s connector or plugin settings UI. See `docs/usage-openai.md` and `docs/usage-perplexity.md`.

Do **not** write API keys into shell profiles or other on-disk config files. This plugin never reads a key from your machine or environment. CureWise never stores credentials on disk; keys are provided only through host secure prompts (Claude `userConfig`). Committed sample configs use the fake placeholder `YOUR_API_KEY`. See `DEVELOPMENT.md` for local development and validation.

Expected header after the host injects your key:

```text
Authorization: Bearer YOUR_API_KEY
```

## Platform compatibility summary

- **Claude Code / Claude Plugin Directory**: repository-driven plugin manifests in this repo, API-key auth supported via `userConfig`.
- **OpenAI/Codex**: repo-driven manifests supported; API-key auth for local/developer installs, with OAuth-related review requirements for public submission. See `docs/usage-openai.md`.
- **Perplexity**: custom remote connector (web UI) and MCP tool (Agent API) supported; managed catalog listing depends on broader registry/discovery flow. See `docs/usage-perplexity.md`.

## Claude setup notes

1. Add local marketplace: `claude plugin marketplace add ./curewise-plugin`.
2. Install plugin: `claude plugin install curewise@curewise-local`.
3. Enter `api_key` when prompted by Claude (secure prompt; do not paste keys into local config files).
4. Claude reads `.mcp.json` and injects `${user_config.api_key}` into request headers.
5. Validate plugin manifest: `npx -y @anthropic-ai/claude-code plugin validate --strict .claude-plugin/plugin.json`.

## Security and privacy

- Privacy policy: `https://curewise.com/privacy`
- Terms of service: `https://curewise.com/terms`
- Support: `support@curewise.com`

CureWise never stores credentials on disk. This repository never commits real API keys. Use placeholders in committed configs and provide credentials only through host secure prompts (Claude `userConfig`).

## Validation

Claude:

```bash
npx -y @anthropic-ai/claude-code plugin validate --strict .claude-plugin/plugin.json
```

## Submission

- Claude plugin directory: [clau.de/plugin-directory-submission](https://clau.de/plugin-directory-submission)
- OpenAI/Codex public directory: submit via OpenAI plugin dashboard flow after meeting review prerequisites in `docs/usage-openai.md`.
- Global MCP Registry: publish `server.json` to [registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io) via `mcp-publisher` for ecosystem discovery.
