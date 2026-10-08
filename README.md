# Wati MCP

Connect any AI assistant to your **Wati** workspace and **Astra** AI agents through the official hosted [Model Context Protocol](https://modelcontextprotocol.io) server.

```
https://mcp.wati.io/mcp        # global
https://eu-mcp.wati.io/mcp     # EU-hosted accounts
```

There's nothing to install or host. You sign in with your Wati account over OAuth, and no API key is needed.

## The fastest way: let your agent do it

Paste this into Claude, ChatGPT, Cursor, Manus or any other agent:

```
Read https://raw.githubusercontent.com/wati-io/wati-mcp/main/AGENTS.md and connect me to Wati.
```

[`AGENTS.md`](AGENTS.md) has step-by-step setup for every major client, the rules agents should follow when using Wati, and troubleshooting.

## What you can do

- **Wati workspace:** search contacts, conversations and leads; review history; manage campaigns and templates; analyze trends; build segments and find follow-ups.
- **Astra AI agents:** build, test, deploy and improve agents; edit instructions, tone, tools and escalation rules; evaluate before publishing.

Try: *"Show the leads who haven't replied in 3 days"* or *"List my Astra agents and summarize their escalation rules."*

## Quick setup

| Client | How |
|---|---|
| Claude (web / desktop) | **Customize → Connectors → Add custom connector**, URL `https://mcp.wati.io/mcp` |
| ChatGPT | **Plugins → +**, MCP Server URL `https://mcp.wati.io/mcp`, Authentication **OAuth** |
| Claude Code | `claude mcp add --transport http wati https://mcp.wati.io/mcp`, then `/mcp` to sign in |
| Claude Code (plugin) | `/plugin marketplace add wati-io/wati-mcp`, then `/plugin install wati@wati` |
| Cursor, VS Code, Codex, Gemini CLI, Windsurf, others | See [`AGENTS.md`](AGENTS.md#4-set-it-up-in-your-client) |

## Requirements

- Wati **Growth, Pro or Business** plan (trial accounts work during the trial)
- For Astra: a Wati account with Astra enabled and connected
- An AI client that supports remote MCP servers or custom connectors

Wati MCP is free for paid Wati users during the launch period.

## What's in this repo

| Path | Purpose |
|---|---|
| `AGENTS.md` | Setup guide written for AI agents to follow |
| `server.json` | Listing for the [MCP Registry](https://registry.modelcontextprotocol.io) |
| `.claude-plugin/`, `.mcp.json`, `skills/` | Claude Code plugin and marketplace |

## Help

- Setup article: [How to set up Wati MCP Server in Claude or ChatGPT](https://support.wati.io/en/articles/14805217-how-to-setup-wati-mcp-server-in-claude-or-chatgpt)
- Support: [support.wati.io](https://support.wati.io)

> Looking for the older self-hosted Python server that uses API tokens? See [`wati-io/wati-mcp-server`](https://github.com/wati-io/wati-mcp-server). For most users, the hosted server above is the recommended option.
