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

**Output of Phase 1:** Meeting summaries with times and action items, to-do carry-forward candidates (checked/unchecked), and a compact project/team context summary. Passed as context to all Phase 2 agents.

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

Return: notable signals grouped by source (channel name or "DM with X"), with sender and timestamp.

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

Read `memory/projects.md` for active projects and their tracker links (Asana, Linear, Jira, Notion). For each project with a tracking link, look for project-level status updates posted today or since the last working day. Do not enumerate individual tickets.

**Asana:** For each project with an Asana link, use `get_status_overview`. Only include updates from today or the previous working day.

**Linear:** For each project with a Linear link, use `get_status_updates` with `type: "project"` and `createdAt` set to the previous working day.

**Jira:** For each project with a Jira link, query for status changes or comments on epics/initiatives (not individual tickets) from today.

**Notion:** For each project with a Notion link, check for updates to the project page from today.

Skip any tracker that is not connected or returns an error — degrade gracefully.

Return: status updates grouped by project, with author, date, and 1-3 sentence summary. Flag discrepancies with Phase 1 meeting context.

---

### Subagent D — Local file activity

Read `memory/me.md` for the workspace path.

1. Find files modified today under the workspace folder, excluding daily-notes/, meeting-notes/, and any archive directories:
   ```bash
   find {{WORKSPACE_PATH}} -type f -newermt "YYYY-MM-DD 00:00:00" \
     -not -path "*/daily-notes/*" \
     -not -path "*/meeting-notes/*"
   ```
   (Substitute today's date for YYYY-MM-DD.)
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

3. **Ownership filter:** Only your own tasks. Exclude delegated work, other teams' blockers (unless they gate your work directly), same-day completions.

Output: verified to-do list with KEEP / DROP / MERGE dispositions and one-line rationale. This feeds directly into Step 2 — do not re-litigate dispositions there.

---

## Step 2 — Synthesize

**What happened today:** 2-4 sentences on the day's 2-3 main themes. Not a list of meetings — what actually mattered and why.

**Project sections:** Read `memory/projects.md` for the active project list. Structure:
- Your team projects: always present. Write "No activity today." if quiet.
- Cross-functional projects: only if active today.
- Other: true overflow only, for items that don't fit any named project.

Each section:
1. **Opening sentence:** Where things stand today. The headline.
2. **Top-level bullets:** Decisions made, blockers surfaced, milestones reached, major open questions. High bar — if it doesn't clear this, it's a sub-bullet or dropped.
3. **Sub-bullets:** Supporting detail, minor flags, specifics.

Formatting:
- No em-dashes as separators. Use colons or plain sentences.
- Bold section headers (`### Project Name`), not bold inline labels.
- Merge signals from meetings, Slack, PRs, tracker updates, and local files into the same section if they touch the same project.

**To-dos:** Merge KEEP items, new AI-derived items, and manual items from today's note. Apply 5-day dedup pass against completed items. Use ownership filter before including any item.

```
#### Date-anchored
- [ ] [task] — [deadline or trigger] 🤖

#### Quick actions
- [ ] [task]

#### Deep work
- [ ] [task] 🤖

#### To-be-triaged
- [ ] [task]
```

- **Date-anchored:** known due date or trigger event
- **Quick actions:** completable in <30 min
- **Deep work:** requires sustained focus or collaboration
- **To-be-triaged:** no clear category yet — use sparingly
- **🤖:** AI could meaningfully help

**What I drove today:** 1-3 plain bullets. High bar: decisions made, documents produced, alignment achieved, PRs shipped, problems solved. Omit section entirely if nothing clears the bar.

---

## Step 3 — Write to daily note

Order:
1. **To-dos block** at the very top, above everything else
2. **Meetings section:** `# Meetings` with wikilinks sorted by time ascending
3. **Daily Summary** with project sections and "What I drove today"

Format:
```
### To-dos

#### Date-anchored
...

#### Quick actions
...

#### Deep work
...

#### To-be-triaged
...

# Meetings
- HH:MM - [[meeting-notes/YYYY-MM-DD/FILENAME_WITHOUT_EXTENSION|Display Name]]

## Daily Summary

### What happened today
[2-4 sentence narrative]

### [Project Name]
[One sentence: where things stand.]

- [Top-level bullet]
  - [Sub-bullet]

### What I drove today
- [plain bullet]

### Other
- [Short items that don't fit]
```

Handling existing content:
- Replace existing `### To-dos` block entirely
- Leave existing `# Meetings` section as-is — do not overwrite
- Remove `## My To-dos` section if present (items now in To-dos block)
- Never overwrite an existing `## Daily Summary` — only append if none exists

---

## Step 4 — Consistency check

1. Every meaningful signal from Phase 1 and Phase 2 appears somewhere in the note
2. Every claim traces to a source
3. Every KEEP item from Step 1 appears in the to-do list. No DROP items appear.
4. Meetings section matches Phase 1 output — every meeting file has a wikilink with correct time and display name

Fix any gaps before proceeding.

---

## Step 5 — Review people and channels

**People:** Check today's Slack interactions against `memory/people-signal.md`. Add anyone you engaged with substantively 3+ times recently who isn't listed. Resolve Slack handle via `slack_search_users` if needed. Do not remove existing entries.

**Channels:** If you participated in a channel not in `memory/slack-channels.md`, flag it and ask whether to add it permanently.

---

## Step 6 — Confirm

> "Daily summary written to `[path]`. [N] to-dos ([M] carried forward, [K] new). [Any notable flags from consistency check or channels review.]"
