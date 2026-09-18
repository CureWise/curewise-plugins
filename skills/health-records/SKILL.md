---
name: health-records
description: Retrieve the user's saved CureWise account summary (timeline and overview text they already generated in the product) so they can read it in this chat.
---

# Health Records Skill

Use this skill when the user asks to:
- Open the summary already stored in their CureWise account
- Read the timeline or overview text they previously generated in the app

This skill only fetches existing account content. It does not create a new medical record, contact a clinic, or tell the user what to do next.

## Workflow

1. Call `get_clinical_history` to load the saved summary and overview JSON.

2. Repeat what the tool returned. If a field is empty, say it is empty.

3. If the user asks a follow-up, answer only from that payload. Do not add facts that were not returned.

## Rules

- This skill is read-only. It does not write or delete account data.
- Do not invent missing dates, results, or events.
- If the payload is empty, tell the user to generate or refresh the summary in the CureWise app at `https://app.curewise.com`.
