---
name: planka
description: Manage Planka kanban boards — create, organize, and track project tasks
argument-hint: overview | board <name>
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

The user can talk naturally or use optional shorthand commands:

| Command | Description |
|---------|-------------|
| `/planka overview` | Show all projects and boards |
| `/planka board <name>` | Show a specific board with lists and cards |

These are shortcuts — the user can also just describe what they want freely: "create a card for Feature X", "move Bug Y to Done", "set up a new board with labels", etc.

Understand the intent, look up the right endpoints from the OpenAPI spec if needed, and execute.

**Finding boards**: Never hardcode IDs. Always list projects first, find boards from the included data, and match by name. Handle `None` values in position/color fields gracefully when sorting or displaying.

**Display formatting**:
- `overview`: Show projects as headers with boards as bullet points. Include board card count.
- `board <name>`: Show each list as a section with its cards listed below. For each card show: name, labels (as colored badges), task progress (e.g. 3/5), and description preview if available. Use markdown formatting — headers for lists, bullet points for cards. Skip empty/unnamed system lists (archive, trash).

Always:

- Load env vars before API calls: `source .env 2>/dev/null; source ~/.env 2>/dev/null` — tries project `.env` first, then global `~/.env`, silently skips if missing
- Use `curl -sL` (silent + follow redirects)
- Use `python3 -c "import json; ..."` for safe JSON encoding
- Ask the user when something is ambiguous (e.g. multiple boards/projects)
- Confirm before destructive actions (delete board/project)
