# MIND plugin for Cursor and Grok Bot

<p align="center"><img src="assets/logo.png" alt="MIND" width="128" height="128"></p>

Official [Cursor](https://cursor.com) / Grok Bot plugin for [MIND](https://www.m-i-n-d.ai) — the persistent memory and knowledge-graph layer for AI agents.

It connects to MIND's hosted Streamable HTTP MCP server at `https://www.m-i-n-d.ai/mcp`, so your agent can query and write memories, documents, folders, the LIFE board, calendar, CRM, tasks, MINDsense, insights, research, automations, and more — 37 tools in total.

## Prerequisites

A MIND account and an API key: sign in at [www.m-i-n-d.ai](https://www.m-i-n-d.ai) → **Settings → Developer → API Keys**. Keys start with `mind_`.

## Install

**Grok Bot:** sidebar account → **Settings → Plugins** → search **MIND** → Add → paste your key into **MIND API key**.

**Cursor:** Customize → Marketplace → search **MIND** → Add → **Plugins → Configure** → set `MIND_API_KEY`.

That is the whole setup. No OAuth round-trip, no sign-in link: the key is sent as `Authorization: Bearer mind_…` on every request, exactly like a GitHub personal access token.

Until the listing is approved, the same connection works as a custom connector:

- **Grok Bot:** Settings → Plugins → custom connector → Server URL `https://www.m-i-n-d.ai/mcp`, auth header `Authorization: Bearer mind_…`
- **Cursor** (`~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "mind": {
      "url": "https://www.m-i-n-d.ai/mcp",
      "headers": { "Authorization": "Bearer mind_YOUR_KEY" }
    }
  }
}
```

## What you get

- **`mind` MCP server** — 37 tools: `mind_context`, `mind_query`, `mind_remember`, `mind_life`, `mind_crm`, `mind_tasks`, `mind_sense`, `mind_research`, `mind_graph`, and the rest of the MIND surface.
- **`mind` skill** — the session discipline that makes memory actually stick: load context at session start, query before asserting, log outcomes after work, never post to the public feed unprompted.

## Layout

- `.cursor-plugin/plugin.json` — manifest and the `MIND_API_KEY` install variable
- `mcp.json` — remote MIND MCP (`https://www.m-i-n-d.ai/mcp`, bearer header)
- `skills/mind/SKILL.md` — when and how to use MIND
- `.grok-plugin/plugin.json`, `.mcp.json` — the same plugin for Grok Build

No secrets live in this repo. Users set `MIND_API_KEY` at install time.

## Links

- Product: https://www.m-i-n-d.ai
- Connect guide: https://www.m-i-n-d.ai/grok
- npm MCP server (stdio, for local clients): [`@astramindapp/mcp-server`](https://www.npmjs.com/package/@astramindapp/mcp-server)
- REST API: `https://m-i-n-d.ai/developer/v1` with header `X-API-Key`

## License

MIT © Astra AI
