---
name: cross-check
description: Send one user-written question to three language models and show a side-by-side comparison of their replies, including a short synthesis of overlap and differences.
---

# Cross-Check Skill

Use this skill when the user asks to:
- Run a Cross-Check on a question they wrote
- Compare how three models answered the same prompt
- Open a previous Cross-Check run from their account

Cross-Check is a comparison tool. It does not make decisions for the user or replace talking with people they trust.

## Workflow

### New run

1. Require a clear prompt of at least 10 characters. If the request is vague, ask them to rewrite it before calling a tool.

2. Call `ask_cross_check` with `prompt`. Set `isPrivate: true` only if the user asks to keep the run private from shared accounts.

3. Present results as:
   - A short summary of each model reply
   - Where the replies overlap
   - Where they differ
   - The synthesized recap last

### Past runs

1. Use `list_cross_checks` to list prior runs.
2. Use `get_cross_check` with an ID to open one run.

## Rules

- Do not use this skill for booking, payments, or account-settings tasks.
- Do not present the output as a decision the user must follow.
- If `list_cross_checks` already has a nearly identical prompt, show that run and ask before starting a new one.
