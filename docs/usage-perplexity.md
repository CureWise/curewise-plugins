# CureWise Perplexity Connector Usage

This guide explains both supported Perplexity integration paths:

- Perplexity web app custom remote connector
- Perplexity Agent API MCP tool integration

## Prerequisites

- A paid Perplexity plan (free plans cannot add custom connectors).
- A CureWise API key from `https://app.curewise.com/me/api-keys`.
- MCP endpoint: `https://app.curewise.com/api/mcp`.

## Organization setting (Perplexity teams)

If you are on an organization account, an admin must enable:

- **Allow members to add custom connectors**

This setting is off by default. If it is disabled, non-admin users may not see the custom connector option.

## Add the connector

In Perplexity:

1. Open **Account settings**.
2. Go to **Connectors**.
3. Click **+ Custom connector**.
4. Choose **Remote**.
5. Enter the connector details:
   - **Name**: `CureWise`
   - **MCP Server URL**: `https://app.curewise.com/api/mcp`
   - **Authentication**: `API Key`
   - **API key value**: your full `cw_...` key
   - **Transport**: `Streamable HTTP`
   - **Icon**: optional (`assets/logo.png` is 56 KB, under the 128 KB limit)
6. Confirm the acknowledgement checkbox and click **Add**.
7. Open the connector card and complete enable/authentication.

## Important transport note

Use **Streamable HTTP** only.

SSE is not supported by the hosted CureWise route in stateless mode. A GET-based SSE attempt fails with:

`SSE transport is not supported in stateless mode. Use POST for all requests.`

## Verify the connection

Run a read-only tool call first, for example:

- `get_profile`
- `list_documents`

If these succeed, the connector is authenticated correctly.

## Perplexity Agent API MCP tool integration

If you are building with the Perplexity Agent API, add CureWise as an MCP tool
directly in your API request:

```python
tools=[{
  "type": "mcp",
  "server_url": "https://app.curewise.com/api/mcp",
  "server_label": "curewise",
  "authorization_token": "cw_your_key_here"
}]
```

Notes:

- `authorization_token` should use a dedicated CureWise API key.
- Start with read-only tool calls (`get_profile`, `list_documents`) before
  enabling operational workflows.
- This integration path is separate from Perplexity's web UI connector form.

## Key hygiene

- Create a dedicated API key for Perplexity (for example: `Perplexity connector`).
- Do not reuse the same key across every client surface.
- Revoke unused or leaked keys from `https://app.curewise.com/me/api-keys`.

## Troubleshooting note

Some community reports mention additional discovery files such as `/.well-known/mcp-connector.json`.
This is not documented in Perplexity's official help article for remote connectors, so treat it as an optional debugging lead only if the connector fails to save.

## Marketplace and discovery status

- CureWise is not currently in Perplexity's managed connector catalog.
- For now, users connect via custom remote connector (web UI) or MCP tool
  configuration (Agent API).
- For broader discovery, publish `server.json` to the global MCP Registry
  (`registry.modelcontextprotocol.io`) using `mcp-publisher`.
