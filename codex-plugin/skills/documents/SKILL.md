---
name: documents
description: Search, browse, and read health documents — including synced EHR records from connected health systems and files uploaded directly by the user (labs, pathology reports, imaging summaries, doctor's notes, insurance letters).
---

# Documents Skill

Use this skill when the user asks to:
- Find a specific document (e.g. "my pathology report", "last MRI summary", "insurance letter from June")
- Browse or list their documents by date, provider, facility, or type
- Read the content of a document
- Search for a keyword or topic across all their documents

## Workflow

### Searching by keyword or topic

1. Use `search_documents` with the user's search terms. This returns ranked snippets.
2. If the user wants to read a specific result in full, call `get_document_content` with the document ID from the search result.

### Browsing or filtering documents

1. Use `get_document_filter_options` first to see what providers, facilities, and document types are available.
2. Use `list_documents` with appropriate filters (date range, provider, facility, document type).
3. Offer to retrieve content for any document the user selects.

### Reading a document

1. Use `get_document_content` with the document ID.
2. Summarize clinically relevant sections rather than dumping the full raw text, unless the user explicitly asks for the full content.

## Rules

- Prefer `search_documents` when the user describes what they are looking for by topic or keyword.
- Prefer `list_documents` with filters when the user wants to browse by date, provider, or document type.
- Do not call `get_document_content` speculatively — only retrieve content when the user has identified or selected a document.
- If no documents are found, suggest the user sync their health system or upload documents at `https://app.curewise.com`.
- This skill is read-only. No tool in this workflow modifies or deletes documents.
