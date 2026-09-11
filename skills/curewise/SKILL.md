---
name: curewise
description: Use CureWise MCP tools to work with patient profile, chats, clinical trials, documents, and health records.
---

# CureWise Skill

Use this skill when a user asks to:
- Retrieve account profile settings or model options
- Continue a CureWise chat thread and read prior messages
- Search or review clinical trials, including eligibility checks
- Browse, filter, or search synced medical documents and uploads
- Query structured health records across labs, medications, diagnoses, and visits
- Run Cross Check to compare model responses for one clinical question

## Tool usage guidance

1. Start with read-only tools (`get_profile`, `get_models`, `list_*`, `get_*`) unless the user asks for a state change.
2. Before write actions (`save_trial`, `unsave_trial`, `chat`, `ask_cross_check`, `check_eligibility`, `extract_search_params`), confirm intent if it is ambiguous.
3. Prefer paginated listing tools first, then fetch detail for specific IDs.
4. For clinical trial flows:
   - Use `search_my_clinical_trials` for personalized discovery.
   - Use `get_clinical_trial` before `check_eligibility` to verify the selected NCT ID.
5. When sharing sensitive outputs, summarize clinically relevant sections instead of dumping raw payloads when possible.

## Authentication note

This plugin uses an API key and forwards it as:

`Authorization: Bearer <CUREWISE_API_KEY>`

Users generate keys at `https://app.curewise.com/me/api-keys`.
