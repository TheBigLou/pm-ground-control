# pm-ground-control v1a Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create the `pm-ground-control` GitHub repo with the core foundation: repo structure, memory templates, generalized daily-summary skill, and the /setup skill.

**Architecture:** A skills-only repo that users clone and run `/setup` to configure their personal workspace elsewhere. Skills are markdown files following the agentskills.io specification. All personal configuration lives in memory files outside the repo, seeded from templates during setup.

**Tech Stack:** Markdown (skill files), YAML frontmatter, Bash (for setup file operations), GitHub

---

## File Structure

**New files to create:**

```
pm-ground-control/
  .github/
    ISSUE_TEMPLATE.md
  skills/
    setup/
      SKILL.md              ← onboarding interview + workspace creation
    daily-summary/
      SKILL.md              ← generalized from Louis's version
  templates/
    memory/
      me.md                 ← user identity, workspace path, tools
      projects.md           ← project registry template
      teams.md              ← team reference template
      slack-channels.md     ← three-tier channel list template
      people-signal.md      ← high-signal people template
    workspace/
      project-template/
        specs/.gitkeep
        notes/.gitkeep
        gtm/.gitkeep
        prototypes/.gitkeep
        external.md         ← Google Docs registry template
  README.md                 ← placeholder for Plan B
  SETUP.md                  ← placeholder for Plan B
  CONTRIBUTING.md           ← placeholder for Plan B
```

---

## Task 1: Initialize GitHub repo

**Files:**
- Create: `pm-ground-control/` (local directory)
- Create: `pm-ground-control/.gitignore`
- Create: `pm-ground-control/README.md` (placeholder)

- [ ] **Step 1: Create local repo**

```bash
mkdir -p ~/Documents/pm-ground-control
cd ~/Documents/pm-ground-control
git init
```

- [ ] **Step 2: Create .gitignore**

Create `.gitignore`:
```
.DS_Store
*.swp
*.swo
```

- [ ] **Step 3: Create placeholder README**

Create `README.md`:
```markdown
# pm-ground-control

AI-powered daily operating system for product managers.

> Full documentation coming soon.
```

- [ ] **Step 4: Initial commit**

```bash
git add .
git commit -m "chore: initialize pm-ground-control repo"
```

- [ ] **Step 5: Create GitHub repo and push**

```bash
gh repo create pm-ground-control --public --description "AI-powered daily operating system for product managers" --push --source .
```

Expected: repo created at `https://github.com/<your-username>/pm-ground-control`

---

## Task 2: Create repo skeleton

**Files:**
- Create: `skills/setup/`, `skills/daily-summary/`
- Create: `templates/memory/`, `templates/workspace/project-template/`

- [ ] **Step 1: Create directory structure**

```bash
mkdir -p skills/setup
mkdir -p skills/daily-summary
mkdir -p templates/memory
mkdir -p templates/workspace/project-template/specs
mkdir -p templates/workspace/project-template/notes
mkdir -p templates/workspace/project-template/gtm
mkdir -p templates/workspace/project-template/prototypes
touch templates/workspace/project-template/specs/.gitkeep
touch templates/workspace/project-template/notes/.gitkeep
touch templates/workspace/project-template/gtm/.gitkeep
touch templates/workspace/project-template/prototypes/.gitkeep
```

- [ ] **Step 2: Create project-template external.md**

Create `templates/workspace/project-template/external.md`:
```markdown
# External Links

Links to Google Docs, Figma files, or other external resources for this project.

| Link | What it is | Last reviewed |
|---|---|---|
| [Doc title](url) | Description | YYYY-MM-DD |
```

- [ ] **Step 3: Commit skeleton**

```bash
git add .
git commit -m "chore: add repo directory skeleton"
```

---

## Task 3: Create memory templates

**Files:**
- Create: `templates/memory/me.md`
- Create: `templates/memory/projects.md`
- Create: `templates/memory/teams.md`
- Create: `templates/memory/slack-channels.md`
- Create: `templates/memory/people-signal.md`

These are blank templates with structure and inline comments. The `/setup` skill populates them during onboarding.

- [ ] **Step 1: Create me.md template**

Create `templates/memory/me.md`:
```markdown
---
name: User identity
description: Personal identity, workspace configuration, tools, and preferences — read by all skills
type: reference
---

## Identity

- **Name:** {{YOUR_NAME}}
- **Role:** {{YOUR_ROLE}}
- **Company:** {{YOUR_COMPANY}}

## Workspace

- **Parent folder:** {{WORKSPACE_PATH}}
  - Daily notes: {{DAILY_NOTES_PATH}}
  - Meeting notes: {{MEETING_NOTES_PATH}}

## Tools

- **Meeting notes:** {{MEETING_NOTES_TOOL}} (e.g. Granola, Otter.ai, manual)
- **Daily notes:** {{DAILY_NOTES_TOOL}} (e.g. Obsidian, Notion, plain markdown)
- **Project tracker:** {{PROJECT_TRACKER}} (e.g. Asana, Linear, Jira, Notion, none)
- **Team communication:** Slack

## Slack

- **User ID:** {{SLACK_USER_ID}}

## Weekly update format

- **Format:** {{WEEKLY_UPDATE_FORMAT}} (e.g. Slack post, email, Notion page)
- **Audience:** {{WEEKLY_UPDATE_AUDIENCE}} (e.g. my manager, my team)

## Reporting structure

- **Manager:** {{MANAGER_NAME}}
```

- [ ] **Step 2: Create projects.md template**

Create `templates/memory/projects.md`:
```markdown
---
name: Projects
description: All active and archived projects with team, Slack channel, tracker links, and local folder — source of truth for daily summary sections
type: project
---

## {{TEAM_1_NAME}}

### {{PROJECT_NAME}}
- **Status:** Active
- **Slack:** #{{CHANNEL_NAME}} ({{CHANNEL_ID}})
- **Asana:** {{ASANA_URL or —}}
- **Linear:** {{LINEAR_URL or —}}
- **Jira:** {{JIRA_URL or —}}
- **Notion:** {{NOTION_URL or —}}
- **Local folder:** `{{WORKSPACE_PATH}}/{{TEAM}}/{{PROJECT}}/`
- **Notes:** {{ANY_NOTES}}

---

## Cross-functional

> Projects you contribute to but don't own. Add as needed.

---

## Archive

> Completed or deprioritized projects.

*(empty)*

---

## How to apply

- **Daily summary:** One section per active project. Write "No activity today." if nothing to report.
- **Adding a project:** Fill in all fields; create local folder with standard template.
- **Completing a project:** Move to Archive with completion date.
```

- [ ] **Step 3: Create teams.md template**

Create `templates/memory/teams.md`:
```markdown
---
name: Teams reference
description: People, roles, and ownership scope for all teams and the broader org — authoritative reference for all contexts
type: reference
---

## {{TEAM_NAME}}

**Mission/ownership:** {{WHAT_THIS_TEAM_OWNS}}
**Eng lead:** {{ENG_LEAD_NAME}} ({{SLACK_ID}})
**Team channels:** #{{TEAM_CHANNEL}}, #{{TEAM_LEADS_CHANNEL}}

### People

| Name | Slack ID | Role | Notes |
|---|---|---|---|
| {{NAME}} | {{SLACK_ID}} | {{ROLE}} | {{NOTES}} |

---

## Org

| Name | Slack ID | Role |
|---|---|---|
| {{NAME}} | {{SLACK_ID}} | {{ROLE}} |
```

- [ ] **Step 4: Create slack-channels.md template**

Create `templates/memory/slack-channels.md`:
```markdown
---
name: Slack channels
description: Three-tier channel list for daily summary Slack sweeps — Tier 1 full read, Tier 2 signal filter, Tier 3 ambient
type: reference
---

## Tier 1: Full read

Derived from projects.md and teams.md. Read all human messages. Follow every thread.

### Project channels

| Channel | ID | Project |
|---|---|---|
| #{{CHANNEL_NAME}} | {{CHANNEL_ID}} | {{PROJECT_NAME}} |

### Team channels

| Channel | ID | Team |
|---|---|---|
| #{{CHANNEL_NAME}} | {{CHANNEL_ID}} | {{TEAM_NAME}} |

---

## Tier 2: Signal filter

Include only: your messages and replies, threads you're in, decisions/announcements, topically relevant content.

| Channel | ID | Why monitored |
|---|---|---|
| #{{CHANNEL_NAME}} | {{CHANNEL_ID}} | {{REASON}} |

---

## Tier 3: Ambient / company-wide

Include only if you participated or it's a company-wide announcement.

| Channel | ID | Notes |
|---|---|---|
| #{{CHANNEL_NAME}} | {{CHANNEL_ID}} | {{NOTES}} |

---

## Notes

- IDs marked `—` should be resolved via `slack_search_channels` on first use.
- Tier 1 is auto-derived from projects.md and teams.md. Add new project channels here when adding projects.
- If you participate in an unlisted channel, flag it in the daily summary step 5 review.
```

- [ ] **Step 5: Create people-signal.md template**

Create `templates/memory/people-signal.md`:
```markdown
---
name: High-signal Slack people
description: People whose Slack messages are prioritized in the daily summary — updated dynamically based on interactions
type: reference
---

## High-signal people

| Name | Slack ID | Role | Why high-signal |
|---|---|---|---|
| {{YOUR_NAME}} | {{YOUR_SLACK_ID}} | {{YOUR_ROLE}} | Self — always include |
| {{NAME}} | {{SLACK_ID}} | {{ROLE}} | {{REASON}} |

## Update rules

When daily-summary runs:
- If you replied to or DM'd someone not on this list 3+ times recently, add them
- Do not remove existing entries — annotate with last-active date if engagement drops
```

- [ ] **Step 6: Commit memory templates**

```bash
git add templates/
git commit -m "feat: add memory file templates"
```

---

## Task 4: Create generalized daily-summary skill

**Files:**
- Create: `skills/daily-summary/SKILL.md`

This is the most important skill. It is generalized from the author's personal setup — all personal details (Slack user ID, workspace paths, channel IDs) are read from memory files rather than hardcoded.

- [ ] **Step 1: Create the skill file**

Create `skills/daily-summary/SKILL.md`:

```markdown
---
name: daily-summary
description: Use when drafting the end-of-day summary. Reads today's meetings, Slack, and memory files to append a structured summary to today's daily note.
---

# Daily Summary

Synthesize today's activity into a structured summary and append it to today's daily note.

## Step 0 — Determine paths

Read `memory/me.md` to get:
- Workspace parent folder
- Daily notes path (e.g. `{{WORKSPACE}}/daily-notes/YYYY/MM/YYYY-MM-DD.md`)
- Meeting notes path (e.g. `{{WORKSPACE}}/meeting-notes/YYYY-MM-DD/`)
- Memory path

Use `currentDate` from context if available, otherwise infer from file timestamps.

Previous working day: step back one calendar day, skipping weekends.

---

## Phase 1 — Grounding (run first, before anything else)

Run a single grounding pass. Do not start Phase 2 until complete.

**Tasks:**

1. **Meeting notes:** Glob `{{MEETING_NOTES_PATH}}/YYYY-MM-DD/*.md`. If the folder doesn't exist, note that and continue. For each file:
   - Read in full (use `offset`/`limit` of 150 lines if needed)
   - Extract: key decisions, open questions, your explicit action items or commitments
   - Extract from YAML frontmatter: `created` field `HH:MM` — check `memory/me.md` for timezone; if using Granola, timestamps are UTC and must be converted to local time
   - Note filename without `.md` (for wikilink) and derive display name: replace ` _ ` → ` / `, then `_` before space or end-of-string → `: `

2. **Carry-forward to-dos:** Read the last 5 working days' daily notes. For each, find `### To-dos` inside `## Daily Summary` and extract:
   - Unchecked `- [ ]` items
   - Checked `- [x]` items (to avoid regenerating completed work)

3. **Context:** Read `memory/projects.md`, `memory/teams.md`, `memory/people-signal.md`, and skim other memory files. Note active projects, key people, today's meeting themes, and initial to-do carry-forward candidates.

**Output of Phase 1:** Meeting summaries with times and action items, to-do carry-forward candidates, project/team context. Passed as context to all Phase 2 agents.

---

## Phase 2 — Signals (run in parallel, after Phase 1 completes)

Dispatch subagents simultaneously. Each receives Phase 1 output as context. All return findings only — no file writes.

---

### Subagent A — Slack

Read `memory/me.md` for your Slack user ID. Read `memory/slack-channels.md` for the full channel list with IDs and tiers. Do not use hardcoded values.

**Tier 1 channels (full read):**

Use a two-pass approach for each channel:
- **Pass 1:** `slack_read_channel` with `oldest` set to today's midnight local time (compute Unix timestamp at runtime — do not hardcode). Follow every thread with replies via `slack_read_thread`.
- **Pass 2:** `slack_search_public_and_private` with `in:#channel-name after:YYYY-MM-DD`. For any result from a prior-day thread root, call `slack_read_thread` for full context.

Deduplicate across passes.

**Your messages (all channels including DMs):**
- `slack_search_public_and_private` with `from:<@YOUR_SLACK_ID> after:YYYY-MM-DD`
- For each result, call `slack_read_thread`

**DMs:**
- `slack_search_public_and_private` with `to:<@YOUR_SLACK_ID> after:YYYY-MM-DD`
- For each high-signal person in `memory/people-signal.md`, also read DM history via `slack_read_channel` with their user ID as `channel_id`

**Tier 2 channels (signal filter):**
Use `slack_search_public_and_private` with `in:#channel-name after:YYYY-MM-DD`. Include only:
- Your messages and replies
- Threads you participated in
- Decisions, announcements, deadlines, or questions directed at you
- Content topically relevant to your active projects (use Phase 1 context)

**Tier 3 channels:** Include only if you participated or it's a company-wide announcement.

**Discard all bot/automated messages.** Only include messages from real humans.

**Use Phase 1 context to filter:** If a meeting already surfaced an action item, don't regenerate it. Flag as "confirmed in Slack" instead.

Return: notable signals grouped by source, with sender and timestamp.

---

### Subagent B — Pull requests

Read `memory/me.md` — if GitHub CLI is not configured, skip this subagent and return nothing.

1. Run both in parallel (substitute today's date):
   - `gh search prs --author "@me" --created ">=YYYY-MM-DD" --json number,title,repository,state,url,createdAt --limit 20`
   - `gh search prs --author "@me" --merged ">=YYYY-MM-DD" --json number,title,repository,state,url,mergedAt --limit 20`
2. Deduplicate. For each PR: repo, number, title, state, one-line description.

Return: PRs created or merged today. If none, return nothing.

---

### Subagent C — Project tracker updates

Read `memory/projects.md` for active projects and their tracker links (Asana, Linear, Jira, Notion). For each project with a tracking link, look for project-level status updates posted today or since the last working day.

**Asana:** For each project with an Asana link, use `get_status_overview`. Only include updates from today or the previous working day.

**Linear:** For each project with a Linear link, use `get_status_updates` with `type: "project"` and `createdAt` set to the previous working day.

**Jira:** For each project with a Jira link, query for status changes or comments on epics/initiatives (not individual tickets) from today.

**Notion:** For each project with a Notion link, check for updates to the project page from today.

Skip any tracker that is not connected or returns an error.

Return: status updates grouped by project, with author, date, and 1-3 sentence summary. Flag discrepancies with Phase 1 meeting context.

---

### Subagent D — Local file activity

Read `memory/me.md` for the workspace path.

1. Find files modified today under the workspace folder, excluding daily-notes/, meeting-notes/, and any archive directories. Use:
   ```bash
   find {{WORKSPACE_PATH}} -type f -newermt "YYYY-MM-DD 00:00:00" \
     -not -path "*/daily-notes/*" \
     -not -path "*/meeting-notes/*"
   ```
2. For each file: read in full (150-line chunks if needed). Identify: document type, project, state, 1-2 sentence summary.

Return: files modified today with path, type, project, and summary. If none, return nothing.

---

## Step 1 — To-do verification pass

For every candidate to-do item, assign one of:
- **KEEP** — still open, no evidence of completion
- **DROP** — completed today or previously
- **MERGE** — variant of another item; combine

1. **Carry-forwards:** For each unchecked item, check Phase 2 signals for completion evidence. If it references a specific doc, read that file directly.

2. **New items from meetings and Slack:** Check whether completed same-day before including:
   - "Send X a message" → check Subagent A
   - "Update the spec" → read the file directly
   - "Open a PR" → check Subagent B
   - Decision made in the meeting itself → done, do not carry forward

3. **Ownership filter:** Only your own tasks. Exclude delegated work, other teams' blockers (unless they gate your work), same-day completions.

---

## Step 2 — Synthesize

**What happened today:** 2-4 sentences on the day's 2-3 main themes.

**Project sections:** Read `memory/projects.md` for the active project list. Structure:
- Your team projects: always present, "No activity today." if quiet
- Cross-functional projects: only if active today
- Other: true overflow only

Each section:
1. Opening sentence: the headline
2. Top-level bullets: decisions, blockers, milestones, major questions
3. Sub-bullets: supporting detail

Formatting:
- No em-dashes as separators
- Bold section headers, not bold inline labels
- Merge signals from all sources into the same project section

**To-dos:** Merge KEEP items, new AI-derived items, and manual items. 5-day dedup pass against completed items.

```
#### Date-anchored
- [ ] [task] — [deadline] 🤖

#### Quick actions
- [ ] [task]

#### Deep work
- [ ] [task] 🤖

#### To-be-triaged
- [ ] [task]
```

🤖 = AI could meaningfully help

**What I drove today:** 1-3 plain bullets. High bar: decisions made, documents produced, alignment achieved, PRs shipped, problems solved. Omit if nothing clears the bar.

---

## Step 3 — Write to daily note

Order:
1. **To-dos block** at the very top
2. **Meetings section:** `# Meetings` with wikilinks sorted by time ascending
3. **Daily Summary** with project sections and "What I drove today"

Handling existing content:
- Replace existing `### To-dos` block
- Leave existing `# Meetings` as-is
- Remove `## My To-dos` if present
- Never overwrite an existing `## Daily Summary`

---

## Step 4 — Consistency check

1. Every meaningful signal appears somewhere in the note
2. Every claim traces to a source
3. Every KEEP item is in the to-do list
4. Meetings section matches Phase 1 output

Fix any gaps before proceeding.

---

## Step 5 — Review people and channels

**People:** Check today's interactions against `memory/people-signal.md`. Add recurring new contacts (3+ interactions). Resolve Slack handle via `slack_search_users` if needed.

**Channels:** Flag any channel you participated in that isn't in `memory/slack-channels.md`. Ask whether to add it permanently.

---

## Step 6 — Confirm

> "Daily summary written to `[path]`. [N] to-dos ([M] carried forward, [K] new). [Any notable flags.]"
```

- [ ] **Step 2: Verify the skill has no hardcoded personal values**

Check that the skill contains no hardcoded:
- Slack user IDs (should reference `memory/me.md`)
- File paths (should reference `memory/me.md`)
- Channel IDs (should reference `memory/slack-channels.md`)
- Project names (should reference `memory/projects.md`)

- [ ] **Step 3: Commit**

```bash
git add skills/daily-summary/
git commit -m "feat: add generalized daily-summary skill"
```

---

## Task 5: Create the /setup skill

**Files:**
- Create: `skills/setup/SKILL.md`

This is the most complex skill — it interviews the user and creates their workspace from scratch.

- [ ] **Step 1: Create the skill file**

Create `skills/setup/SKILL.md`:

```markdown
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

Determine the path to the pm-ground-control repo (the directory containing this skill). This is where the templates live.

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
> 1. Granola + Obsidian + Granola Sync (recommended)
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
> 5. Multiple (you can specify later)
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
7. "Who is the audience for your weekly update? (e.g. my manager, my team, skip)"

---

## Step 4 — Teams interview

> "Now let's set up your teams. Tell me about the first team you manage or work most closely with."

For each team, ask one at a time:
1. "What's the team name?"
2. "Who is the engineering lead? (name)"
3. "What does this team own? (brief description)"
4. "What are the team's Slack channels? (list them, I'll ask for IDs next)"
5. For each channel named: "What's the channel ID for #{{channel}}? You can find this by right-clicking the channel in Slack → View channel details → scroll to bottom."
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
5. "Any notes about this project? (optional)"

After each project: "Add another project? (yes/no)"

---

## Step 6 — Slack channels

> "Let's set up your Slack channel list. Tier 1 (full read) is automatically set from your project and team channels. Now let's add Tier 2 — broader channels you're in that sometimes have useful signal."

Ask: "List any Tier 2 channels (name and ID if you have it). These are channels you participate in but don't own — company-wide discussions, cross-functional channels, etc."

Ask: "Any Tier 3 channels? These are company-wide announcement channels you only need to check for major news."

---

## Step 7 — Folder structure

> "How would you like your project folders organized?
> 1. Default template — each project folder gets: specs/, notes/, gtm/, prototypes/, external.md (recommended)
> 2. Flat — project folders only, no subfolders
> 3. Custom — I'll create project folders and you can organize them yourself"

---

## Step 8 — Create workspace

Using all interview responses, create the workspace:

**Folder structure:**

```bash
mkdir -p {{WORKSPACE_PATH}}/daily-notes
mkdir -p {{WORKSPACE_PATH}}/meeting-notes
mkdir -p {{WORKSPACE_PATH}}/memory
```

For each team and project (using the chosen template):
```bash
mkdir -p {{WORKSPACE_PATH}}/{{TEAM_FOLDER}}/{{PROJECT_FOLDER}}/specs
mkdir -p {{WORKSPACE_PATH}}/{{TEAM_FOLDER}}/{{PROJECT_FOLDER}}/notes
mkdir -p {{WORKSPACE_PATH}}/{{TEAM_FOLDER}}/{{PROJECT_FOLDER}}/gtm
mkdir -p {{WORKSPACE_PATH}}/{{TEAM_FOLDER}}/{{PROJECT_FOLDER}}/prototypes
```

(Adjust based on chosen template option — omit subfolders for flat, create folders only for custom.)

**Memory files:**

Copy each template from `{{REPO_PATH}}/templates/memory/` to `{{WORKSPACE_PATH}}/memory/` and populate with interview responses:

- `me.md` — name, role, company, Slack ID, workspace paths, tools, weekly update format
- `projects.md` — one entry per project with team, channel, tracker links, local folder
- `teams.md` — one section per team with people and channels
- `slack-channels.md` — Tier 1 auto-derived from projects + teams; Tier 2 and 3 from interview
- `people-signal.md` — user themselves plus eng leads and manager as initial entries

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

Copy skills from the repo to the user's AI agent skills directory.

Ask: "Which AI agent are you using?
1. Claude Code (`~/.claude/skills/`)
2. Codex (`~/.agents/skills/`)
3. Other (specify path)"

Copy skills:
```bash
cp -r {{REPO_PATH}}/skills/daily-summary {{SKILLS_DIR}}/
cp -r {{REPO_PATH}}/skills/weekly-summary {{SKILLS_DIR}}/
cp -r {{REPO_PATH}}/skills/weekly-update {{SKILLS_DIR}}/
# Note: setup skill is intentionally not copied — it lives in the repo
```

---

## Step 10 — Confirm

Output a summary:

> "Setup complete! Here's what was created:
>
> **Workspace:** `{{WORKSPACE_PATH}}`
> **Teams:** {{TEAM_LIST}}
> **Projects:** {{PROJECT_LIST}}
> **Skills installed:** daily-summary, weekly-summary, weekly-update → `{{SKILLS_DIR}}`
>
> **Next steps:**
> 1. Connect your MCP tools (see SETUP.md for instructions): Slack{{, TRACKER_NAME}}
> 2. Set up {{MEETING_NOTES_TOOL}} to export transcripts to `{{WORKSPACE_PATH}}/meeting-notes/YYYY-MM-DD/`
> 3. Set up {{DAILY_NOTES_TOOL}} to save daily notes to `{{WORKSPACE_PATH}}/daily-notes/YYYY/MM/YYYY-MM-DD.md`
> 4. Run `/daily-summary` at the end of your next working day
>
> You can re-run `/setup` at any time to add teams or projects."
```

- [ ] **Step 2: Verify the skill covers all interview paths**

Check:
- Slack user asks for user ID
- Non-Slack path skips channel questions gracefully
- All three folder structure options are handled
- Memory files are populated (not left as templates with `{{placeholders}}`)

- [ ] **Step 3: Commit**

```bash
git add skills/setup/
git commit -m "feat: add /setup onboarding skill"
```

---

## Task 6: Add placeholder skills for Plan B

**Files:**
- Create: `skills/weekly-summary/SKILL.md`
- Create: `skills/weekly-update/SKILL.md`

These are stubs that will be fully written in Plan B.

- [ ] **Step 1: Create weekly-summary stub**

Create `skills/weekly-summary/SKILL.md`:
```markdown
---
name: weekly-summary
description: Use when synthesizing the week's work into a local summary document.
---

# Weekly Summary

> Coming in v1 — this skill is not yet implemented.
```

- [ ] **Step 2: Create weekly-update stub**

Create `skills/weekly-update/SKILL.md`:
```markdown
---
name: weekly-update
description: Use when drafting the weekly stakeholder update.
---

# Weekly Update

> Coming in v1 — this skill is not yet implemented.
```

- [ ] **Step 3: Commit**

```bash
git add skills/weekly-summary/ skills/weekly-update/
git commit -m "chore: add weekly skill stubs for Plan B"
```

---

## Task 7: Push and verify

- [ ] **Step 1: Push all commits**

```bash
git push origin main
```

- [ ] **Step 2: Verify repo on GitHub**

Open `https://github.com/<your-username>/pm-ground-control` and confirm:
- All directories are present
- `skills/daily-summary/SKILL.md` is readable and contains no hardcoded personal values
- `skills/setup/SKILL.md` is readable and covers all interview paths
- `templates/memory/` contains all 5 template files

- [ ] **Step 3: Test /setup skill end-to-end**

Run `/setup` in a fresh AI agent session pointed at the repo. Use a test workspace path (e.g. `~/Documents/TestWorkspace/`). Verify:
- All interview questions appear in correct order
- Workspace folder structure is created
- Memory files are populated with interview responses (no `{{placeholders}}` remaining)
- Skills are copied to the correct directory
- Confirm message is accurate

Clean up test workspace after verification:
```bash
rm -rf ~/Documents/TestWorkspace/
```

- [ ] **Step 4: Test daily-summary skill**

Run `/daily-summary` with the workspace configured. Verify:
- Reads workspace path from `memory/me.md`
- Reads channels from `memory/slack-channels.md`
- Reads projects from `memory/projects.md`
- Phase 1 completes before Phase 2 starts
- Output is written to the correct daily note path

---

## Self-Review Notes

- The `/setup` skill uses `{{placeholders}}` in the skill body as interview prompts, not as unreplaced template values — this is intentional.
- The daily-summary skill references `memory/me.md` for the workspace path but doesn't define the exact path format — the setup skill establishes this convention, which is acceptable since setup runs first.
- Weekly-summary and weekly-update are stubs — explicitly noted as "Coming in v1" to set expectations without blocking the commit.
- Jira and Notion are included in the project tracker interview and the daily-summary Subagent C, but MCP call specifics for those tools should be validated against actual MCP documentation before shipping.
