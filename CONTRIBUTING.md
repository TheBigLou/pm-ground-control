# Contributing

pm-ground-control is an open-source project and contributions are welcome.

---

## What makes a good contribution

**Skills and skill improvements** are the highest-value contributions:
- Bug fixes in existing skills (wrong behavior, edge cases, bad output)
- Improvements to the daily-summary skill based on your real usage
- Fully implemented weekly-summary and weekly-update skills
- New skills (accomplishment logging, retrospective synthesis, etc.)

**Integration support:**
- Jira MCP integration in the daily-summary skill (currently a stub)
- Notion MCP integration in the daily-summary skill (currently a stub)
- Granola + Obsidian + Granola Sync setup walkthrough for SETUP.md

**Documentation:**
- Setup walkthrough for your meeting notes tool
- MCP connection instructions for tools not yet covered

---

## Skill authoring

Skills follow the [agentskills.io specification](https://agentskills.io/specification). Each skill is a directory with a `SKILL.md` file:

```
skills/
  your-skill-name/
    SKILL.md
```

`SKILL.md` requires YAML frontmatter with `name` and `description`:

```markdown
---
name: your-skill-name
description: Use when [specific triggering condition]. [What it does.]
---

# Your Skill Name

...
```

**Key principles for skills in this repo:**
- No hardcoded personal values (Slack IDs, file paths, project names). Read everything from memory files.
- Degrade gracefully when tools aren't connected — skip, note it, continue.
- Follow the daily-summary skill's Phase 1 → Phase 2 pattern for any skill that gathers signals from multiple sources.

---

## How to submit

1. Fork the repo
2. Create a branch: `git checkout -b feat/your-improvement`
3. Make your changes
4. Test the skill in a real AI agent session — describe what you tested in your PR
5. Open a pull request with a description of what you changed and why

For significant new skills or architectural changes, open an issue first to discuss the approach.

---

## Reporting bugs

Open an issue with:
- Which skill and which step failed
- What you expected vs. what happened
- Your AI agent (Claude Code, Codex, Gemini CLI, etc.)
- Which MCP tools you have connected
