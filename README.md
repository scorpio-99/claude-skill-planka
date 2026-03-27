# Planka Skill for Claude Code

A Claude Code skill for managing [Planka](https://github.com/plankanban/planka) kanban boards through natural language. No rigid CLI syntax — just describe what you want.

## Setup

1. Clone this repo into your Claude Code skills directory:
   ```bash
   git clone <repo-url> ~/.claude/skills/claude-skill-planka
   ```
   Or copy the folder manually to `~/.claude/skills/`.

2. Set environment variables (in project `.env`, `~/.env`, or shell):
   ```bash
   PLANKA_URL=https://planka.example.com
   PLANKA_API_KEY=your-api-key
   ```

3. Generate an API key in Planka: User Settings → API Key

## Quick Commands

```
/planka overview              # all projects and boards at a glance
/planka board "My Project"    # detailed board view with cards and progress
```

## What You Can Do

Talk naturally — the skill understands intent and figures out the API calls.

### Browse & Inspect
```
/planka show me all my boards
/planka what's in the ToDo list?
/planka show me card "Login Bug" with all its tasks
```

### Create & Organize
```
/planka create a card "Dark Mode" in Backlog with fitting labels
/planka set up a new board "Backend" with lists ToDo, In Progress, Done, Blocked
/planka add labels Bug (red), Feature (blue), Urgent (red) to the "My Project" board
/planka add tasks to "Dark Mode": update theme provider, test contrast, update docs
```

### Move & Update
```
/planka move "Dark Mode" to In Progress
/planka mark "Dark Mode" as done
/planka rename "Dark Mode" to "Dark Mode v2"
/planka add a comment to "Dark Mode": waiting for design review
```

### Bulk Operations
```
/planka move everything from In Progress to Done
/planka read the git log and create cards for all completed features
/planka look at board X and create the same structure for a new board
/planka check all cards for typos and fix them
```

### Project Sync
```
/planka sync the board with our recent git commits
/planka create cards for all open issues
/planka what changed on the board since last week?
```

The skill fetches the [Planka OpenAPI spec](https://plankanban.github.io/planka/swagger-ui/swagger.json) dynamically, so it stays up to date with the latest API.

## Requirements

- Planka instance with API key support
- `curl` and `python3` available in shell
