# Planka Skill for Claude Code

A Claude Code skill for managing [Planka](https://github.com/plankanban/planka) kanban boards through natural language.

## Setup

1. Clone this repo into your Claude Code skills directory:
   ```bash
   git clone <repo-url> ~/.claude/skills/claude-skill-planka
   ```
   Or copy the folder manually to `~/.claude/skills/`.

2. Set environment variables (in `.env` or shell):
   ```bash
   PLANKA_URL=https://planka.example.com
   PLANKA_API_KEY=your-api-key
   ```

3. Generate an API key in Planka: User Settings → API Key

## Usage

Shorthand commands:

```
/planka overview                    # show all projects and boards
/planka board "My Project"          # show a specific board with lists and cards
```

Or just talk naturally:

```
/planka create a card "Fix login bug" in the ToDo list with label Bug
/planka move "Fix login bug" to In Progress
/planka add tasks to "Fix login bug": check auth flow, test redirect, update tests
/planka mark "Fix login bug" as done
/planka set up a board with lists ToDo, In Progress, Done and labels Bug, Feature, UI
/planka look at board X and create the same structure for a new board
/planka read the git log and create cards for all completed features
/planka check all cards for typos and fix them
```

The skill fetches the [Planka OpenAPI spec](https://plankanban.github.io/planka/swagger-ui/swagger.json) dynamically, so it stays up to date with the latest API.

## Requirements

- Planka instance with API key support
- `curl` and `python3` available in shell
