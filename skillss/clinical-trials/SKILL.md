---
name: clinical-trials
description: Search a public directory of research studies by keyword or NCT ID, open a listing, and save or unsave listings the user wants to keep for later.
---

# Clinical Trials Skill

Use this skill when the user asks to:
- Search research study listings by keyword or NCT ID
- Open details for a listing they already identified
- List, save, or remove saved listings in their CureWise account

This skill retrieves listings the user asked to look up. It does not enroll anyone in a study, contact a site, or decide what care they should pursue.

## Workflow

1. **Keyword search:** Use `search_clinical_trials` when the user gives a keyword, condition name, or NCT ID.

2. **Saved listings:** Use `search_my_clinical_trials` only when the user asks for listings already associated with their CureWise account. Use `list_saved_trials` to show bookmarks.

3. **Listing detail:** Use `get_clinical_trial` with an NCT ID the user provided or that appeared in a search result.

4. **Saved comparison report:** Use `check_eligibility` only when the user explicitly asks for the stored informational report for a specific NCT ID they already selected. Treat the result as account data, not a determination of what they should do.

5. **Bookmarks:** Use `save_trial` / `unsave_trial` only when the user clearly asks to save or remove a listing.

## Rules

- Do not tell the user they should join, avoid, or qualify for a study.
- Do not call `check_eligibility` unless the user named a specific NCT ID and asked for that report.
- Confirm before `save_trial` or `unsave_trial` if the request is ambiguous.
- If a search returns nothing, say so and offer a broader keyword search.
