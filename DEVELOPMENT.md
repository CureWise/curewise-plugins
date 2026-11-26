# Local Development & CI/CD Setup

## Secure credential handling

CureWise never stores credentials on disk. End users should enter keys only
through host secure prompts:

- **Claude Code**: `userConfig` API-key prompt (`sensitive: true`)
- **Cursor**: **Settings → Extensions → CureWise** / **Plugins → Configure**

Do **not** write keys into shell profiles (`~/.zshrc`, `~/.bashrc`),
`~/.cursor/mcp.json`, or other on-disk config files.

For local development tooling and CI/CD only, use short-lived injection methods
below. Keys should exist in memory for the current process/session, then go away.

### Option 1: Credential managers (recommended for local CLI)

Pull the secret into the **current shell session only**—do not append it to a
profile file.

```bash
# 1Password CLI (adapt vault/item path to your team)
eval "$(op signin)"
# Inject for this session only; do not redirect into ~/.zshrc
CUREWISE_API_KEY="$(op read 'op://Engineering/CureWise/api-key')"
```

```bash
# AWS Secrets Manager (adapt secret id / region to your account)
CUREWISE_API_KEY="$(aws secretsmanager get-secret-value \
  --secret-id curewise/dev-api-key \
  --query SecretString \
  --output text)"
```

```bash
# macOS Keychain (only if your team already stores the item there)
CUREWISE_API_KEY="$(security find-generic-password -a CureWise -s CureWiseAPIKey -w)"
```

Then launch the CLI/client from that same session. When the session ends, the
value is gone.

### Option 2: CI/CD secret injection

Inject at job runtime from the platform secret store. Never commit the value.

**GitHub Actions:**

```yaml
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Validate plugin manifests
        env:
          CUREWISE_API_KEY: ${{ secrets.CUREWISE_API_KEY }}
        run: node scripts/validate-template.mjs
```

**GitLab CI:**

```yaml
validate:
  script:
    - node scripts/validate-template.mjs
  variables:
    # Set CUREWISE_API_KEY as a masked CI/CD variable in project settings
    CUREWISE_API_KEY: $CUREWISE_API_KEY
```

### Option 3: Host UI (preferred for interactive clients)

- Claude: install the plugin and enter `api_key` when prompted.
- Cursor: install the plugin and set `CUREWISE_API_KEY` in plugin Configure.
- ChatGPT / Perplexity: use the connector settings UI (see `docs/usage-openai.md`
  and `docs/usage-perplexity.md`).

No shell injection is required for these paths.

## Local validation

This repository is plugin packaging (manifests, MCP config, skills). Validate
shape and manifests without embedding secrets in files:

```bash
# Cursor template / repo-shape check
node scripts/validate-template.mjs

# Claude plugin manifest (strict)
npx -y @anthropic-ai/claude-code plugin validate --strict .claude-plugin/plugin.json
```

Functional MCP checks: enter a real key via the host UI (or a short-lived
session inject for CLI), then call a read-only tool such as `get_profile` or
`list_documents`. Create keys at `https://app.curewise.com/me/api-keys`.

## Important notes

- Never commit API keys to git.
- Do not add `.env` / `.env.local` files with live secrets to this repo.
- Credentials should only exist in memory during your session or CI job.
- For CI/CD, inject credentials at runtime; never check them in.
- Directory policy: do not instruct end users to read ambient machine
  credentials or edit on-disk MCP/shell configs. Prefer `userConfig` / Configure.
