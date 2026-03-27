---
name: planka
description: Manage Planka kanban boards — create, organize, and track project tasks
---

Interact with a self-hosted Planka instance. Planka is a Trello-like kanban board.

> See [README.md](README.md) for setup instructions, usage examples, and advanced use cases.

## Setup

Required environment variables:

- `PLANKA_URL` — Base URL (e.g. `https://planka.example.com`)
- `PLANKA_API_KEY` — API key (generated in Planka UI → User Settings)

These can be set in:
1. The current project's `.env` file (most common)
2. `~/.env` (global, for cross-project use)
3. Shell environment (e.g. `~/.bashrc` or `~/.zshrc`)

## API

Base path: `$PLANKA_URL/api/`

Auth header: `X-Api-Key: $PLANKA_API_KEY`

**Full OpenAPI spec**: https://plankanban.github.io/planka/swagger-ui/swagger.json
Fetch this via WebFetch when you need endpoint details, request/response schemas, or field names. This is the authoritative source — do not guess endpoints.

## Key Concepts

- **Hierarchy**: Project → Board → List → Card → Task List → Task
- **Responses**: `{"item": {...}}` for single, `{"items": [...]}` for collections, related data in `"included"`
- **Positioning**: Numeric, multiples of 65536. Lower = higher on the board.
- **Colors** (for lists/labels): `dark-granite`, `lagune-blue`, `orange-peel`, `bright-moss`, `berry-red`, `light-mud`, `midnight-blue`, `light-cocoa`, `summer-sky`, `pumpkin-orange`

## How to Use

The user will tell you what they want in natural language — e.g. "create a card for Feature X", "show me the board", "move Bug Y to Done", "create a new board".

Understand the intent, look up the right endpoints from the OpenAPI spec if needed, and execute. Always:

- Load env vars before API calls: `source .env 2>/dev/null; source ~/.env 2>/dev/null` — tries project `.env` first, then global `~/.env`, silently skips if missing
- Use `curl -sL` (silent + follow redirects)
- Use `python3 -c "import json; ..."` for safe JSON encoding
- Ask the user when something is ambiguous (e.g. multiple boards/projects)
- Confirm before destructive actions (delete board/project)
