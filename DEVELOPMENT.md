# Local Development & Validation

This repository is plugin packaging only: manifests, MCP config, skills, and
docs. It contains no runnable scripts and reads no credentials.

## Credential handling

CureWise never stores credentials on disk and this plugin never reads a key
from your machine or environment. Users provide a key only through the host’s
secure prompt:

- **Claude Code**: enter the API key when prompted. It is declared as a
  `userConfig` option (`sensitive: true`) in `.claude-plugin/plugin.json` and
  referenced in `.mcp.json` as `${user_config.api_key}`. Claude stores it in its
  secure credential store.
- **ChatGPT / Codex / Perplexity**: enter the key in the client’s connector or
  plugin settings UI (see `docs/usage-openai.md` and `docs/usage-perplexity.md`).

In committed sample configs, the key always appears as the fake placeholder
`YOUR_API_KEY`. Never commit a real key, and never add instructions that read a
key from a shell profile, environment variable, or on-disk MCP config file.

Create a real key for your own testing at
`https://app.curewise.com/me/api-keys`, and enter it only through the host UI
above.

## Validation

Validate the Claude plugin manifest with the official tool:

```bash
npx -y @anthropic-ai/claude-code plugin validate --strict .claude-plugin/plugin.json
```

For a functional check, install the plugin in a host, enter your key through the
host UI, then call a read-only tool such as `get_profile` or `list_documents`
before any write operations.

## Important notes

- Never commit API keys to git.
- Do not add `.env` / `.env.local` files with live secrets to this repo.
- Do not add runnable scripts; the directory accepts only Markdown, text, JSON,
  YAML, and allowed image types from non-Verified organizations.
- Keep credential guidance to host secure prompts (`userConfig`) only; do not
  document reading ambient machine credentials.
