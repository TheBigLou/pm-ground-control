# pm-ground-control

This is the pm-ground-control repo — a set of AI skills for product managers. The user has opened this folder to run setup or update their skills.

## Repo structure

```
skills/          # Skills installed into the user's agent during setup
  setup/         # One-time onboarding skill — the entry point
  daily-summary/
  weekly-summary/
  weekly-update/
  log-accomplishment/
  accomplishments-review/
  add-source/
templates/       # Memory file templates populated during setup
  memory/        # me.md, projects.md, teams.md, slack-channels.md, etc.
  workspace/     # Project folder templates (external.md)
examples/        # Sample output files for reference (gitignored)
```

## Important

- The user's **workspace** (notes, project files, memory) lives outside this repo at a path they choose during setup. Do not write personal files into this repo.
- Skills are installed by copying them from `skills/` into the agent's skills directory (`~/.claude/skills/` for Claude Code).
- Memory templates are copied from `templates/memory/` into the user's workspace and populated with their information.

## Entry point

If the user is here for the first time, the skill to run is `/setup`.

If they are returning to update skills after a `git pull`, direct them to re-run step 9 of `/setup` (the install step).
