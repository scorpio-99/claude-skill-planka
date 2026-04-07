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

### CRITICAL: How to look up endpoints

1. **NEVER guess endpoints.** Always look them up first.
2. **Do NOT trust WebFetch summaries of the swagger.json.** WebFetch summarizes content and frequently returns incorrect/shortened paths (e.g. `/card-labels` instead of `/cards/{cardId}/card-labels`).
3. **Instead, fetch the raw JSON and parse it yourself:**
   ```bash
   curl -sL https://plankanban.github.io/planka/swagger-ui/swagger.json | python3 -c "
   import json, sys
   spec = json.load(sys.stdin)
   for path, methods in spec.get('paths', {}).items():
       for method in methods:
           if method in ('get','post','put','patch','delete'):
               print(f'{method.upper()} {path}')
   " | grep -i "SEARCH_TERM"
   ```
4. **On 404 errors**: Do NOT retry the same endpoint. Immediately fetch the spec and find the correct path. The server version may differ from the spec -- try path variations (e.g. `/cards/{id}/card-labels` vs `/card-labels`).

## Key Concepts

- **Hierarchy**: Project → Board → List → Card → Task List → Task
- **Responses**: `{"item": {...}}` for single, `{"items": [...]}` for collections, related data in `"included"`
- **Positioning**: Numeric, multiples of 65536. Lower = higher on the board.
- **Colors** (for lists/labels): e.g. `dark-granite`, `lagoon-blue`, `orange-peel`, `bright-moss`, `berry-red`, `light-mud`, `midnight-blue`, `light-cocoa`, `summer-sky`, `pumpkin-orange`

## How to Use

The user can talk naturally or use optional shorthand commands:

| Command | Description |
|---------|-------------|
| `/planka overview` | Show all projects and boards |
| `/planka board <name>` | Show a specific board with lists and cards |

These are shortcuts — the user can also just describe what they want freely: "create a card for Feature X", "move Bug Y to Done", "set up a new board with labels", etc.

Understand the intent, look up the right endpoints from the OpenAPI spec if needed, and execute.

**Finding boards**: Never hardcode IDs. Always list projects first, find boards from the included data, and match by name.

**Null safety**: Many Planka fields can be `None`/`null` — name, description, color, position, etc. Always use `or ""`, `or 0`, `(x or "").strip()` patterns in Python code. Never call methods on potentially null values without guarding.

**Display formatting**:
- `overview`: Show projects as headers with boards as bullet points. Include board card count.
- `board <name>`: Show each list as a section with its cards listed below. For each card show: name, labels (as colored badges), task progress (e.g. 3/5), and description preview if available. Use markdown formatting — headers for lists, bullet points for cards. Skip empty/unnamed system lists (archive, trash).

Always:

- Load env vars before API calls: `source .env 2>/dev/null; source ~/.env 2>/dev/null` — tries project `.env` first, then global `~/.env`, silently skips if missing
- Use `curl -sL` (silent + follow redirects)
- Use `python3 -c "import json; ..."` for safe JSON encoding
- Ask the user when something is ambiguous (e.g. multiple boards/projects)
- **Always confirm before executing**: describe what you're about to do (e.g. "I'll create a card 'Dark Mode' in Backlog with labels Feature, UI") and wait for the user to approve before making any API calls. This applies to all write operations — create, update, move, delete.
