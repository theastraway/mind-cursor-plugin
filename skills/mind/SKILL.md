---
name: mind
description: >
  Use this skill whenever you interact with MIND (m-i-n-d.ai) — the persistent
  memory and knowledge-graph layer for AI agents. TRIGGER at session start,
  before any non-trivial decision, before asserting something doesn't exist,
  and after completing any significant work. The `mind` MCP server is provided by this plugin;
  it needs only the MIND_API_KEY set at install time.
---

# MIND — Persistent Memory for Agents

MIND is your user's permanent knowledge graph: identity, memories, decisions,
tasks, contacts, and emotional context that survive across every session,
every tool, and every agent. This skill is the operating discipline for
using it well.

## Setup

This plugin already connects the `mind` MCP server (`https://www.m-i-n-d.ai/mcp`).
The only thing it needs is `MIND_API_KEY`, entered when the plugin is installed
(Cursor: Plugins → Configure; Grok Bot: Settings → Plugins → MIND). Create a key
at https://www.m-i-n-d.ai → Settings → Developer → API Keys (prefix `mind_`).
If every `mind_*` call fails with an authentication error, the key is missing,
revoked, or pasted with whitespace — fix it there, never by asking the user to
paste it into chat.

## Session protocol (every session)

1. **START** — call `mind_context` (sections: soul, user, rules, priorities,
   recent). This loads who the user is, their operating rules, and what's
   active. Never skip it.
2. **BEFORE deciding or asserting** — call `mind_query` on the specific topic.
   MIND is authoritative memory; do not guess or claim something doesn't
   exist without querying.
3. **AFTER non-trivial work** — call `mind_remember` with `type: "entry"`
   (PRIVATE) to log the outcome. Unlogged work is invisible to the next
   session. Tag it and set a `source`.

## Privacy rules (hard)

- `document` and `entry` are PRIVATE to the user's knowledge graph.
- `feed_post` (via `mind_remember`) and `mind_social create_thought` are
  PUBLIC — they post to the user's social feed. NEVER use them unless the
  user explicitly says "post", "share", "tweet", or "feed".
- Admin tools (`mind_admin`, `mind_agents`, `mind_tickets`) require an
  admin-scoped key; do not retry on authorization errors.

## Tool map (by app)

| Surface | Tools |
|---|---|
| Memory & knowledge | `mind_query`, `mind_remember`, `mind_folders`, `mind_folder_routes`, `mind_folder_suggest`, `mind_context`, `mind_graph` |
| Life & productivity | `mind_life` (focus/project/outcome + calendar), `mind_focuses`, `mind_tasks`, `mind_automate`, `mind_notify` |
| People | `mind_crm` (contacts/pipeline/activities), `mind_social`, `mind_social_analytics`, `mind_profile` |
| Intelligence | `mind_sense` (emotional state), `mind_insights`, `mind_research`, `mind_osint`, `mind_train` |
| Fleet & admin | `mind_agents`, `mind_personas`, `mind_tickets`, `mind_admin`, `mind_accounts` |
| Typed documents | `mind_list_templates`, `mind_get_template`, `mind_save_typed`, `mind_bootstrap_templates` |

## Usage discipline

- Classify each user request before acting: task → `mind_life`/`mind_tasks`;
  person → `mind_crm`; interaction → `mind_crm log_activity`; knowledge or
  decision → `mind_remember`.
- Log CRM activities the moment an interaction happens; complete LIFE tasks
  the moment they close — never batch to end-of-session.
- Default query mode is `hybrid`; use `graph` mode for relationship
  questions ("who is connected to X").
- Prefer storing synthesized findings over raw dumps; always tag.
