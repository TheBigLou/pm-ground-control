# pm-ground-control — Design Spec

**Date:** 2026-04-06
**Author:** Louis Hellman

---

## Overview

`pm-ground-control` is a standalone, open-source AI skill package for product managers. It provides a daily operating system: automated end-of-day summaries, weekly synthesis, and stakeholder updates — all driven by a structured memory system that the user builds once and maintains over time.

The package is tool-agnostic and works with any AI agent that supports skills (Claude Code, Codex, Gemini CLI, etc.). It ships as a GitHub repo; users clone it, run `/setup`, and their workspace is ready.

**Tagline:** AI-powered daily operating system for product managers.

**Repo name:** `pm-ground-control` (GitHub, Louis's personal account)

---

## Architecture

### Separation of concerns

The repo contains only the skills, memory templates, and setup tooling. The user's workspace — meeting notes, daily notes, project files, memory — lives entirely outside the repo at a path they specify during setup. This keeps personal data private and makes skill updates simple: pull the repo, re-run install.

```
[pm-ground-control repo]          [user workspace, e.g. ~/Documents/WorkNotes/]
  skills/              ──copy──>   ~/.claude/skills/pm-ground-control/
  templates/memory/    ──seed──>   workspace/memory/
  templates/workspace/ ──seed──>   workspace/ (folder structure)
  README.md
  SETUP.md
  CONTRIBUTING.md
```

---

## Repo Structure

```
pm-ground-control/
  skills/
    setup/
      SKILL.md
    daily-summary/
      SKILL.md
    weekly-summary/
      SKILL.md
    weekly-update/
      SKILL.md
  templates/
    memory/
      me.md              ← user identity: name, role, Slack ID, workspace path, tools
      projects.md        ← active/archived projects with team, channel, tracker links
      teams.md           ← team members, roles, ownership scope
      slack-channels.md  ← three-tier channel list (Tier 1 full read, 2 signal filter, 3 ambient)
      people-signal.md   ← high-signal people for daily summary prioritization
    workspace/
      project-template/  ← default subfolder structure (see Workspace Structure below)
  README.md
  SETUP.md
  CONTRIBUTING.md
```

---

## The `/setup` Skill

The onboarding entry point. Fully automated — the user provides a parent folder path and the skill handles everything else.

### Flow

1. **Workspace path:** Ask for the parent folder (e.g. `~/Documents/WorkNotes/`). Create it if it doesn't exist.

2. **Tools interview:**
   - Meeting notes solution — recommend Granola + Obsidian + Granola Sync community plugin; any folder of dated markdown files works
   - Daily notes tool — recommend Obsidian; any markdown editor works
   - Project tracker — Asana, Linear, Jira, Notion, or none
   - Team communication — Slack (required for channel sweep features)
   Records choices in `memory/me.md`

3. **Identity interview:**
   - Name, role, company
   - Slack/Teams user ID (if applicable)
   - Manager and interim manager (if applicable)

4. **Teams interview:** One team at a time —
   - Team name, eng lead, key people (name + role)
   - Team Slack/Teams channels
   - Ownership scope (what does this team own?)
   - "Add another team?" escape hatch

5. **Projects interview:** One project at a time —
   - Project name, which team it belongs to
   - Home Slack/Teams channel + ID
   - Project tracker link (Asana/Linear/Jira/Notion)
   - "Add another project?" escape hatch

6. **Slack/Teams channels:** Tier 1 auto-derived from project channels + team channels. Asks explicitly for Tier 2 (broader participation) and Tier 3 (company-wide/ambient) channels.

7. **Workspace folder structure:** Presents three options:
   - **Default template:** `<team>/<project>/specs/`, `notes/`, `gtm/`, `prototypes/`, `external.md`
   - **Custom:** User provides their own structure; skill notes they'll need to update path references manually
   - **Flat:** Project folders with no prescribed subfolders

8. **Creates workspace:** Folder structure, memory files populated from interview.

9. **Installs skills:** Copies skills from repo into user's AI agent skills directory (e.g. `~/.claude/skills/`).

10. **Confirms:** Prints summary of what was created, what MCP connections to set up, and how to run the first daily summary.

---

## Core Skills

### `daily-summary`

Synthesizes the day's activity into a structured note appended to the user's daily note file.

**Two-phase architecture:**
- **Phase 1 (grounding):** Reads today's meeting transcripts, last 5 days of carry-forward to-dos, and memory files. Produces a context document.
- **Phase 2 (parallel signals):** Three concurrent subagents — Slack/Teams sweep, PR sweep, local file activity — each receive Phase 1 context for filtering.

**Output sections:**
- To-dos (top of note, always)
- Meetings list (wikilinks to transcripts)
- What happened today (2-4 sentence narrative)
- Per-project sections (one per active project, derived from `memory/projects.md`)
- What I drove today (1-3 plain bullets, high bar)

**Tool-conditional behavior:**
- Slack MCP: full channel sweep per `memory/slack-channels.md` (v1 only; Teams in v2)
- Asana/Linear/Jira/Notion MCP: project status update sweep
- GitHub CLI: PR sweep
- Graceful degradation if any tool is unavailable

### `weekly-summary`

Synthesizes the week's daily notes into a single weekly document. Reads from the same memory files. Identifies themes across the week, carries forward open to-dos, and surfaces what moved.

### `weekly-update`

Drafts a stakeholder-facing weekly update from the weekly summary. Format configured in `memory/me.md`:
- Slack post (default)
- Email
- Notion page
- Other (user-specified)

---

## Memory Files

All personal configuration lives in the workspace, not the repo. Populated by `/setup`, maintained over time.

| File | Purpose |
|---|---|
| `me.md` | Identity, workspace paths, tools, update preferences |
| `projects.md` | Active/proposed/archived projects with team, channels, tracker links, local folders |
| `teams.md` | People, roles, ownership scope for all teams + broader org |
| `slack-channels.md` | Three-tier channel list with IDs |
| `people-signal.md` | High-signal people for daily summary prioritization, updated dynamically |

---

## Workspace Structure

### Default template (per project folder)
```
<team>/<project>/
  specs/
  notes/
  gtm/
  prototypes/
  external.md    ← Google Docs / external links registry
```

### Setup options
1. **Accept default** — standard template applied to all projects
2. **Custom structure** — user defines their own; skill notes they'll need to update path references
3. **Flat** — project folders only, no prescribed subfolders

---

## MCP Integrations

| Tool | What it unlocks | Status |
|---|---|---|
| Slack | Full channel sweep, DMs, thread following | v1 |
| Asana | Project status updates in daily summary | v1 |
| Linear | Project status updates in daily summary | v1 |
| Jira | Project status updates in daily summary | v1 |
| Notion | Project status updates; weekly update output | v1 |
| GitHub CLI | PR sweep in daily summary | v1 |
| Microsoft Teams | Channel sweep | v2 |

All integrations degrade gracefully — if an MCP isn't connected, that data source is skipped and noted in the summary output.

---

## Documentation

### `README.md`
- What it is and why (2-3 sentences)
- What you get (skill list)
- Prerequisites
- Recommended stack: Granola + Obsidian + Granola Sync, Slack MCP, project tracker MCP
- Quick start (clone → `/setup` → done)
- MCP integration table
- Link to `SETUP.md`

### `SETUP.md`
- Granola + Obsidian + Granola Sync setup walkthrough
- MCP connection instructions per tool
- Workspace folder structure explanation
- How to update skills when new versions ship

### `CONTRIBUTING.md`
- How to add new integrations
- How to submit skill improvements
- Testing guidance

---

## Release Scope

### v1
- `/setup` skill
- `daily-summary`
- `weekly-summary`
- `weekly-update`
- All MCP integrations listed above
- Full documentation

### v2 (planned)
- `log-accomplishment`
- `accomplishments-review`
- Additional skills TBD based on community feedback

---

## Open Questions

- Jira and Notion integrations ship in v1 but are not validated against Louis's own setup — README should note these as community-contributed paths pending real-world testing.
- Granola Sync plugin setup details to be provided by Louis for SETUP.md.
