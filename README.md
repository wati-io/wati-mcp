# Wati MCP

Connect any AI assistant to your **Wati** workspace and **Astra** AI agents through the official hosted [Model Context Protocol](https://modelcontextprotocol.io) server.

```
https://mcp.wati.io/mcp        # global
https://eu-mcp.wati.io/mcp     # EU-hosted accounts
```

There's nothing to install or host. You sign in with your Wati account over OAuth, and no API key is needed.

## The fastest way: let your agent do it

Paste this into Claude, ChatGPT, Meta Muse, Cursor, Manus or any other agent:

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
| Meta Muse | Ask Muse: *"Create a Custom Connector named wati: remote streamable HTTP, URL https://mcp.wati.io/mcp, OAuth sign-in"* |
| Claude Code | `claude mcp add --transport http wati https://mcp.wati.io/mcp`, then `/mcp` to sign in |
| Claude Code (plugin) | `/plugin marketplace add wati-io/wati-mcp`, then `/plugin install wati@wati` |
| Muse Code, Cursor, VS Code, Codex, Gemini CLI, Windsurf, others | See [`AGENTS.md`](AGENTS.md#4-set-it-up-in-your-client) |

## Daily WhatsApp brief with Meta Muse

Muse can check Wati for you every morning and nudge you during the day, so you don't have to dig through the inbox. Connect Wati to Muse first (see the table above), then copy this prompt into Muse. Edit the **My settings** block before you send it.

```text
You are my WhatsApp business assistant. Use the Wati connector to keep me on top of my customers. Save this as a skill called "Wati daily brief" and set up the schedule below.

## My settings
- Timezone: <your timezone, e.g. Asia/Singapore>
- Morning brief: every day at 09:00
- Business hours: Mon–Sat, 09:00–19:00
- "Waiting too long" means: no reply for more than 2 business hours
- VIP customers: contacts tagged "VIP" (or list names / numbers here)
- Wati workspace: the active one (if I have several, ask me which one first)

## 1. Morning brief (every day at the time above)
Look at the last 24 hours, compared with the 24 hours before. Use wati_get_conversations with my timezone, page through all results, and read the latest messages with wati_get_messages. Send me a short, phone-friendly brief with these sections. Skip a section if it's empty.

🔴 Needs a reply: conversations where the customer's latest message is inbound and nobody has answered. Sort by VIP first, then longest wait. For each, show the name, how long they've waited, a one-line summary of what they want, and a short draft reply.
😠 At risk: customers who sound upset or might leave. Look for complaints, refunds, cancellations, "still waiting", repeated follow-ups, or an angry tone, and use CX scores from wati_get_contact_profile when they're available. Say why you flagged each one and suggest how to save the relationship.
🆕 New leads: contacts first seen in the last 24 hours (first_seen_at in wati_get_contact_profile; wati_get_contact_count for the total). Rank them by how ready they look to buy, and suggest the next message for the top ones.
📊 Insights: conversations, new contacts and broadcast results (wati_list_campaigns, wati_get_campaign) compared with the day before; the top conversation tags (wati_list_conversation_tags); unread Instagram comments if I use Instagram; and a warning if my credit balance (wati_get_credit_balance) is low. Give real numbers and up to 3 observations, not a dashboard.
✅ Suggested actions: the 3–5 most valuable things to do today, each in one line, numbered so I can reply "do 1 and 3".

## 2. Alerts during the day
During business hours, check every 2 hours. Message me straight away only if:
- a VIP has waited longer than my threshold, or
- a customer sounds angry or mentions cancelling, refunding or a complaint.
Otherwise stay quiet and save it for tomorrow's brief.

## 3. Rules
- Never send anything to a customer without my explicit approval. Show me the exact text and the recipient, then wait for "send".
- WhatsApp only allows free-form replies within 24 hours of the customer's last message. If that window has closed, say so and suggest an approved template (wati_list_templates), then preview it with wati_send_template_dry_run before I approve.
- Never launch broadcasts, change templates, delete contacts, or close or block conversations unless I ask.
- Keep customer data inside Wati and this chat. Show only what each section needs, never full contact lists.
- If a Wati call fails or returns nothing, say so in one line. Don't guess.

Start now by running today's brief once so I can check the format.
```

> **Tip:** reply to the brief in plain language, such as *"send 1 and 3"*, *"make the reply to Sarah warmer"* or *"why is Ahmed at risk?"*. Muse drafts and you approve. The same prompt also works in ChatGPT and Claude if you set up a scheduled task there.

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
