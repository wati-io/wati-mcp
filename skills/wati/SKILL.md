---
name: wati
description: Use when working with a Wati workspace or Astra AI agents through the Wati MCP tools, such as contacts, conversations, leads, campaigns, broadcasts, message templates, segments, or building, testing and publishing Astra agents.
---

# Working with Wati

The Wati MCP tools act on a live business account. Messages reach real customers.

## Before the first call
- If no Wati tools are available, the server isn't connected or signed in. Ask the user to run `/mcp`, select `wati` and complete the browser login. EU-hosted accounts use `https://eu-mcp.wati.io/mcp` instead of the default URL.
- Discover tools from the live list. Don't assume tool names.

## Rules
1. **Confirm before anything customer-facing or irreversible**: sending messages, launching or scheduling broadcasts and campaigns, submitting or editing templates, publishing or deploying Astra agents, deleting or bulk-editing contacts. Say exactly what will happen and to how many people, then wait for a clear yes.
2. **Read before you write.** Fetch the current template, agent instructions or contact record before you change it, and show the user the diff.
3. **Test Astra changes before publishing.** Run the evaluate/test tools and share the results first.
4. **Pull only what the task needs.** Don't dump full contact lists or conversation histories into the chat.
5. **Never ask for passwords or API keys.** Access is OAuth only.

## Troubleshooting
- `401`: the session expired. Reconnect via `/mcp`.
- `403`: the Wati user lacks permission. Ask the workspace admin.
- `Unknown tool`: reload the Wati tools and retry.
