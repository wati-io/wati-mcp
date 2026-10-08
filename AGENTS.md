# Connect to the Wati MCP Server: setup guide for AI agents

> **For AI agents:** if a user pointed you at this file, they want you to connect
> their AI client to Wati. Follow the steps below in order. You cannot complete
> the login for them. Wati uses OAuth, so the user signs in themselves in a browser.
>
> **For humans:** paste this into your agent:
> `Read https://raw.githubusercontent.com/wati-io/wati-mcp/main/AGENTS.md and connect me to Wati.`

---

## 1. What this server is

The Wati MCP server lets an AI assistant work inside a customer's **Wati** workspace
(WhatsApp, Instagram, TikTok, and other channels) and their **Astra** AI agents.

| Area | What the assistant can do |
|---|---|
| Wati workspace | Search and manage contacts, conversations and leads; read conversation history; manage campaigns and message templates; analyze activity trends; build segments and find follow-ups |
| Astra AI agents | Build, test, deploy and improve agents; edit instructions, tone, tools and escalation rules; evaluate responses before publishing |

Do not hard-code tool names. Read the live list from `tools/list` after you connect, because tools are added over time.

## 2. Connection facts

| Field | Value |
|---|---|
| Server URL (global) | `https://mcp.wati.io/mcp` |
| Server URL (EU-hosted accounts) | `https://eu-mcp.wati.io/mcp` |
| Transport | Streamable HTTP (remote MCP) |
| Auth | OAuth 2.1, authorization code + PKCE (S256), dynamic client registration |
| OAuth metadata | `https://mcp.wati.io/.well-known/oauth-authorization-server` |
| Scope | `all` |
| API key / token | **None.** Never ask the user for a Wati password or API key. |

An unauthenticated request returns `401`. That is expected: it starts the OAuth flow.

## 3. Before you start, check these with the user

1. **Region.** Ask: *"Is your Wati account hosted in the EU?"* If yes, or if they are unsure and are an EU business, use `https://eu-mcp.wati.io/mcp`. Otherwise use the global URL.
2. **Wati plan.** MCP access needs a Growth, Pro or Business plan. Trial accounts work during the trial.
3. **Astra (optional).** To manage AI agents, the Wati account must have Astra enabled and connected.

## 4. Set it up in your client

Find your own client below. If it is not listed, use **4.9 Generic**.

### 4.1 Claude (claude.ai web / Claude desktop)
The user does this in the UI; you cannot add connectors for them.
1. **Customize → Connectors → Add → Add custom connector**
2. Name `Wati`, URL `https://mcp.wati.io/mcp` (or the EU URL), then **Add**
3. Click **Connect**, choose **Wati** or **Astra**, **Continue**, then sign in
4. Approve permissions if asked

### 4.2 Claude Code
```bash
claude mcp add --transport http wati https://mcp.wati.io/mcp
```
Then ask the user to run `/mcp` in an interactive Claude Code session, select `wati`, and complete the browser login.

### 4.3 ChatGPT (web / desktop)
The user does this in the UI:
1. **Plugins → +**
2. Name `Wati`, description `Custom connector to manage Wati and Astra agents.`, MCP Server URL `https://mcp.wati.io/mcp`, Authentication **OAuth**
3. Tick the warning checkbox, **Create**, then complete the Wati sign-in

### 4.4 Cursor (`~/.cursor/mcp.json` or `.cursor/mcp.json`)
```json
{
  "mcpServers": {
    "wati": { "url": "https://mcp.wati.io/mcp" }
  }
}
```

### 4.5 VS Code / GitHub Copilot (`.vscode/mcp.json`)
```json
{
  "servers": {
    "wati": { "type": "http", "url": "https://mcp.wati.io/mcp" }
  }
}
```

### 4.6 OpenAI Codex CLI (`~/.codex/config.toml`)
```toml
[mcp_servers.wati]
url = "https://mcp.wati.io/mcp"
```
Then run `codex mcp login wati`.

### 4.7 Gemini CLI (`~/.gemini/settings.json`)
```json
{
  "mcpServers": {
    "wati": { "httpUrl": "https://mcp.wati.io/mcp" }
  }
}
```

### 4.8 Windsurf (`~/.codeium/windsurf/mcp_config.json`)
```json
{
  "mcpServers": {
    "wati": { "serverUrl": "https://mcp.wati.io/mcp" }
  }
}
```

### 4.9 Generic: Manus, Muse, and any other MCP client
- **If the client supports remote MCP servers with OAuth** (often called a "custom connector" or "custom MCP"), add a server with URL `https://mcp.wati.io/mcp`, transport **HTTP / Streamable HTTP**, and auth **OAuth**. Leave the client ID and secret empty, because the server supports dynamic registration.
- **If the client only supports local (stdio) servers**, bridge it with `mcp-remote`:
  ```json
  {
    "mcpServers": {
      "wati": {
        "command": "npx",
        "args": ["-y", "mcp-remote", "https://mcp.wati.io/mcp"]
      }
    }
  }
  ```
  A browser window opens for the Wati login on first use.
- **If the client supports neither**, tell the user it cannot connect to Wati MCP yet. Do not invent a workaround.

## 5. Verify the connection

After the login, call `tools/list` and run one read-only request:
- *"Which Wati and Astra tools can you use?"*
- *"List my Astra agents."*
- *"Show my 5 most recent conversations."*

Report to the user which tools are available. The first request can be slow; later ones are faster.

## 6. Rules for agents using Wati

1. **Confirm before anything a customer will see or that cannot be undone.** That covers sending messages, launching or scheduling broadcasts and campaigns, submitting or editing templates, publishing or deploying Astra agents, and deleting or bulk-editing contacts. State exactly what will happen, then wait for a clear yes.
2. **Read before you write.** Look at the current state, such as the existing template or the agent's live instructions, before you change it.
3. **Test Astra changes before publishing.** Use the evaluate/test tools first, then show the user the result.
4. **Keep customer data in Wati.** Pull only the records the task needs. Don't paste whole contact lists into the chat.
5. **Never handle credentials.** OAuth only. Never ask for or store the user's password or tokens.

## 7. Troubleshooting

| Symptom | Fix |
|---|---|
| No "Connectors" / custom MCP option | The client plan doesn't support custom MCP; upgrade the client plan |
| No login popup | Allow pop-ups for the client, then retry |
| Wati login error | Sign in to Wati in another browser tab first, then retry |
| `Unknown tool` | Ask the assistant to reload the Wati tools and try again |
| `401 Unauthorized` | Session expired: disconnect and reconnect the connector |
| `403 Forbidden` | The Wati user lacks permission; ask the workspace admin |
| Assistant ignores Wati | Make sure the connector is enabled for this chat |
| Data looks empty or wrong region | Check the account isn't EU-hosted and on the wrong URL |

## 8. Reference

- Official setup article: <https://support.wati.io/en/articles/14805217-how-to-setup-wati-mcp-server-in-claude-or-chatgpt>
- Wati MCP is free for paid Wati users during the launch period.
- Mobile apps are less reliable than desktop or web for MCP connectors.
