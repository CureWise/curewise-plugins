---
name: curewise
description: Access CureWise — an AI health navigation platform for people navigating cancer. Use to retrieve account profile and model preferences, read chat threads, or when none of the more specific CureWise skills (clinical-trials, health-records, cross-check, documents) apply.
---

# CureWise Skill

Use this skill for:
- Retrieving account profile settings (`get_profile`) or available AI models (`get_models`)
- Reading past CureWise chat threads (`list_chat_threads`, `get_chat_thread`, `get_chat_messages`)
- General CureWise account questions

For focused workflows, prefer the dedicated skills:
- **clinical-trials** — search and save public research-study listings
- **health-records** — open the summary already stored in the user's account
- **cross-check** — compare three model replies to the same prompt
- **documents** — search and read files the user uploaded or synced

## Tool usage guidance

1. Always start with read-only tools (`get_profile`, `get_models`, `list_*`, `get_*`).
2. When sharing health data, summarize clinically relevant sections — do not dump raw payloads.
3. CureWise is educational only and does not provide medical advice, diagnosis, or treatment.

## Authentication

This plugin uses an OAuth-minted API key forwarded as:

`Authorization: Bearer <token>`

Users connect via OAuth when installing the plugin. Manual API keys can also be generated at `https://app.curewise.com/mcp`.
