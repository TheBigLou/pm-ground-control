---
name: setup
description: Use when setting up pm-ground-control for the first time. Creates the workspace folder structure, populates memory files through an interview, and installs skills.
---

# pm-ground-control Setup

Welcome to pm-ground-control. This skill will set up your workspace through a short interview. It will:
1. Ask where you want your workspace
2. Learn about your tools
3. Learn about your teams and projects
4. Create your workspace folder structure
5. Populate your memory files
6. Install the skills into your AI agent

This will take about 10-15 minutes. Let's start.

---

## Step 0 — Locate the repo

Determine the path to the pm-ground-control repo (the directory containing this skill). This is where the templates live. Derive it from this file's path — the repo root is two levels up from this file.

---

## Step 1 — Workspace path

Ask the user:

> "Where would you like your workspace folder? This is where your daily notes, meeting notes, and project files will live. Example: `~/Documents/WorkNotes/`"

Wait for their response. Expand `~` to the full home path. Create the directory if it doesn't exist:

```bash
mkdir -p {{WORKSPACE_PATH}}
```

---

## Step 2 — Tools interview

Ask the following questions one at a time:

**Meeting notes tool:**
> "What tool do you use for meeting notes?
> 1. Granola + Obsidian + Granola Sync (recommended — auto-exports transcripts to local markdown)
> 2. Granola (manual export)
> 3. Otter.ai
> 4. Fireflies
> 5. I write notes manually in markdown
> 6. Other"

**Daily notes tool:**
> "What tool do you use for daily notes?
> 1. Obsidian (recommended)
> 2. Notion
> 3. Plain markdown files
> 4. Other"

**Project tracker:**
> "Which project tracker does your team use?
> 1. Asana
> 2. Linear
> 3. Jira
> 4. Notion
> 5. Multiple (you can specify per project later)
> 6. None"

**Slack:**
> "Do you use Slack for team communication? (yes/no)"

Record all answers — they go into `memory/me.md`.

---

## Step 3 — Identity interview

Ask one at a time:

1. "What's your name?"
2. "What's your role? (e.g. Product Manager, Senior PM, Principal PM)"
3. "What company do you work at?"
4. If Slack: "What's your Slack user ID? You can find this in Slack under your profile → More → Copy member ID. It looks like `U08B13F3YCX`."
5. "Who is your manager? (name)"
6. "What format do you prefer for your weekly update? (Slack post / email / Notion page / other)"
7. "Who is the audience for your weekly update? (e.g. my manager, my team — or skip)"

---

## Step 4 — Teams interview

> "Now let's set up your teams. Tell me about the first team you manage or work most closely with."

For each team, ask one at a time:
1. "What's the team name?"
2. "Who is the engineering lead? (name)"
3. "What does this team own? (brief description)"
4. "What are the team's Slack channels? (list them)"
5. For each channel named: "What's the channel ID for #{{channel}}? Right-click the channel in Slack → View channel details → scroll to bottom to find it."
6. "Who are the key people on this team? List names and roles, one per line."

After each team: "Add another team? (yes/no)"

---

## Step 5 — Projects interview

> "Now let's set up your active projects."

For each project, ask one at a time:
1. "What's the project name?"
2. "Which team does it belong to?"
3. "Does it have a dedicated Slack channel? If yes, what's the name and ID?"
4. "Do you track this in Asana, Linear, Jira, or Notion? If yes, paste the project URL."
5. "Any notes about this project? (optional, press enter to skip)"

After each project: "Add another project? (yes/no)"

---

## Step 6 — Slack channels

> "Let's set up your Slack channel list. Tier 1 (full read) is automatically set from your project and team channels. Now let's add Tier 2 — broader channels you participate in that sometimes have useful signal."

Ask: "List any Tier 2 channels (name and ID if you have it). These might be cross-functional channels, broad product or engineering channels, or customer-facing channels."

Ask: "Any Tier 3 channels? These are company-wide announcement channels you only need to scan for major news."

---

## Step 7 — Folder structure

> "How would you like your project folders organized?
> 1. Default template — each project folder gets: specs/, notes/, gtm/, prototypes/, external.md (recommended)
> 2. Flat — project folders only, no subfolders
> 3. Custom — I'll create project folders and you organize them yourself"

---

## Step 7b — Optional top-level directories

> "A few optional top-level directories can be useful alongside your project folders. Tell me which you'd like included:
>
> - `_accomplishments/` — a running log of meaningful wins, decisions, and shipped work; used by the accomplishments skills for perf reviews and interview prep
> - `_explorations/` — research and side investigations not tied to a specific project
> - `_reference/` — reference materials, downloaded docs, third-party context
> - `_strategy/` — strategic thinking, vision docs, and planning artifacts
> - `_templates/` — reusable document templates (PRDs, briefs, specs)
>
> Which would you like? (list them, or say 'all' or 'none')"

Record the user's choices — they get created in Step 8.

---

## Step 8 — Create workspace

Using all interview responses, create the workspace.

**Base folders:**

```bash
mkdir -p {{WORKSPACE_PATH}}/daily-notes
mkdir -p {{WORKSPACE_PATH}}/meeting-notes
mkdir -p {{WORKSPACE_PATH}}/memory
```

**Optional directories** — create each one the user selected in Step 7b:

```bash
mkdir -p {{WORKSPACE_PATH}}/_accomplishments   # if selected
mkdir -p {{WORKSPACE_PATH}}/_explorations      # if selected
mkdir -p {{WORKSPACE_PATH}}/_reference         # if selected
mkdir -p {{WORKSPACE_PATH}}/_strategy          # if selected
mkdir -p {{WORKSPACE_PATH}}/_templates         # if selected
```

**Project folders** — for each team and project (using the chosen template):

Default:
```bash
mkdir -p {{WORKSPACE_PATH}}/{{TEAM_FOLDER}}/{{PROJECT_FOLDER}}/specs
mkdir -p {{WORKSPACE_PATH}}/{{TEAM_FOLDER}}/{{PROJECT_FOLDER}}/notes
mkdir -p {{WORKSPACE_PATH}}/{{TEAM_FOLDER}}/{{PROJECT_FOLDER}}/gtm
mkdir -p {{WORKSPACE_PATH}}/{{TEAM_FOLDER}}/{{PROJECT_FOLDER}}/prototypes
cp {{REPO_PATH}}/templates/workspace/project-template/external.md \
   {{WORKSPACE_PATH}}/{{TEAM_FOLDER}}/{{PROJECT_FOLDER}}/external.md
```

Flat: `mkdir -p {{WORKSPACE_PATH}}/{{TEAM_FOLDER}}/{{PROJECT_FOLDER}}`

Custom: `mkdir -p {{WORKSPACE_PATH}}/{{TEAM_FOLDER}}/{{PROJECT_FOLDER}}`

**Memory files** — copy each template and populate with interview responses:

```bash
cp {{REPO_PATH}}/templates/memory/me.md {{WORKSPACE_PATH}}/memory/me.md
cp {{REPO_PATH}}/templates/memory/projects.md {{WORKSPACE_PATH}}/memory/projects.md
cp {{REPO_PATH}}/templates/memory/teams.md {{WORKSPACE_PATH}}/memory/teams.md
cp {{REPO_PATH}}/templates/memory/slack-channels.md {{WORKSPACE_PATH}}/memory/slack-channels.md
cp {{REPO_PATH}}/templates/memory/people-signal.md {{WORKSPACE_PATH}}/memory/people-signal.md
```

After copying, edit each file to replace all `{{placeholders}}` with actual values from the interview. Do not leave any placeholders — if the user didn't provide a value, use `—`.

**MEMORY.md index:**

Create `{{WORKSPACE_PATH}}/memory/MEMORY.md`:

```markdown
# Memory

- [User identity](me.md) — name, role, tools, workspace paths
- [Projects](projects.md) — active/archived projects with team, channels, tracker links
- [Teams reference](teams.md) — people, roles, ownership for all teams
- [Slack channels](slack-channels.md) — three-tier channel list for daily summary sweeps
- [High-signal Slack people](people-signal.md) — prioritized in daily summaries; updated each run
```

---

## Step 9 — Install skills

Ask: "Which AI agent are you using?
1. Claude Code (`~/.claude/skills/`)
2. Codex (`~/.agents/skills/`)
3. Other (specify the skills directory path)"

Resolve the skills directory, then copy:

```bash
cp -r {{REPO_PATH}}/skills/daily-summary {{SKILLS_DIR}}/
cp -r {{REPO_PATH}}/skills/weekly-summary {{SKILLS_DIR}}/
cp -r {{REPO_PATH}}/skills/weekly-update {{SKILLS_DIR}}/
```

Note: the setup skill itself lives in the repo and is not copied — users re-run it by returning to the repo.

---

## Step 10 — Confirm

Output a summary:

> "Setup complete! Here's what was created:
>
> **Workspace:** `{{WORKSPACE_PATH}}`
> **Teams:** {{TEAM_LIST}}
> **Projects:** {{PROJECT_LIST}}
> **Optional directories:** {{OPTIONAL_DIR_LIST or "none"}}
> **Skills installed:** daily-summary, weekly-summary, weekly-update → `{{SKILLS_DIR}}`
>
> **Next steps:**
> 1. Connect your MCP tools (see SETUP.md for instructions): Slack{{, PROJECT_TRACKER_NAME if applicable}}
> 2. Configure {{MEETING_NOTES_TOOL}} to export transcripts to `{{WORKSPACE_PATH}}/meeting-notes/YYYY-MM-DD/`
> 3. Configure {{DAILY_NOTES_TOOL}} to save daily notes to `{{WORKSPACE_PATH}}/daily-notes/YYYY/MM/YYYY-MM-DD.md`
> 4. Run `/daily-summary` at the end of your next working day
>
> You can re-run `/setup` at any time to add teams or projects."
