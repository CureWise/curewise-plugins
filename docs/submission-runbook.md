# Submission Runbook

This runbook tracks execution of the submission to Claude Plugin Directory.

## 1) Local validation

### Claude plugin manifest

Run strict validation against the plugin manifest directly:

```bash
npx -y @anthropic-ai/claude-code plugin validate --strict .claude-plugin/plugin.json
```

Expected output includes:
- `Validating plugin manifest: .../.claude-plugin/plugin.json`
- `Validation passed`

### Claude local marketplace (optional for local testing)

Run strict validation for `.claude-plugin/marketplace.json`:

```bash
npx -y @anthropic-ai/claude-code plugin validate --strict .claude-plugin/marketplace.json
```

## 2) Claude Plugin Directory submission

Portal:
- `https://clau.de/plugin-directory-submission`

Required before clicking submit:
- Public GitHub repo URL: `https://github.com/curewise/curewise-plugin`
- Privacy policy URL: `https://curewise.com/privacy`
- Support contact: `support@curewise.com`
- Test account with no MFA and populated data
- Tool annotations + clear read/write separation in MCP server
- Credential docs use Claude `userConfig` only (no shell-profile or on-disk MCP key instructions)

Manual action:
- Sign in with Console role (Developer/Admin/Owner) and submit from portal.

## 3) Perplexity and OpenAI connector note

- Perplexity custom remote connectors are configured in Perplexity's UI and are not submitted from this repository.
- OpenAI ChatGPT custom connectors are configured in OpenAI's workspace UI and are not submitted from this repository.
- This repo can store reference setup docs and non-authoritative config examples, but neither platform ingests these files as marketplace submissions.
