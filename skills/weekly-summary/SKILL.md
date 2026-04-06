---
name: weekly-summary
description: Use when synthesizing the week's daily notes into a weekly summary document. Run at end of week.
---

# Weekly Summary

Synthesize the week's daily notes into a single weekly summary document and save it to the workspace.

## Step 0 — Determine paths

Read `memory/me.md` for workspace path. Use `currentDate` from context to identify the current week.

- Week span: Monday through today (or Friday if run on Monday)
- Working days: Monday–Friday, skipping any days where no daily note exists
- Weekly summary output: `{{WORKSPACE}}/daily-notes/YYYY/WW/YYYY-WW.md` (or alongside daily notes — use whatever convention exists, create if new)

---

## Step 1 — Read the week's daily notes

For each working day this week (Monday through today):
1. Read the daily note at `{{WORKSPACE}}/daily-notes/YYYY/MM/YYYY-MM-DD.md`
2. If the note doesn't exist, skip that day
3. From each note, extract:
   - `### What happened today` narrative
   - All project sections with their content
   - `### What I drove today` bullets (if present)
   - `### To-dos` block — all unchecked `- [ ]` items

Read `memory/projects.md` and `memory/me.md` for context.

---

## Step 2 — Synthesize

**What moved this week:** 3-5 sentences. Identify the 2-4 themes that ran across the week — decisions made, problems solved, things shipped, things stuck. Not a day-by-day recap — what the week added up to.

**Project sections:** One section per active project (from `memory/projects.md`). For each:

1. **Opening sentence:** Where this project stands at the end of the week.
2. **What moved:** Top-level bullets for decisions made, milestones reached, blockers surfaced or resolved this week.
3. **Open questions:** Any unresolved questions that need attention next week.

Write "No activity this week." if a project had nothing.

**Open to-dos:** Collect all unchecked `- [ ]` items from across the week's daily notes. Deduplicate — if the same item appears across multiple days, keep the most recent version. Apply the same ownership filter as daily-summary: only your own tasks.

Format the same as daily-summary to-dos:
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

**What I drove this week:** Collect all "What I drove today" bullets from the week. Deduplicate and consolidate — if the same project or decision appears across multiple days, merge into one bullet. Apply the same high bar: decisions made, documents produced, alignment achieved, PRs shipped, problems solved. Write as 3-7 plain bullets. Omit if nothing clears the bar.

---

## Step 3 — Write weekly summary

Create or overwrite `{{WORKSPACE}}/daily-notes/YYYY/WW/YYYY-WW.md`:

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

## Weekly Summary

### What moved this week
[3-5 sentence narrative]

### [Project Name]
[One sentence: where things stand at end of week.]

- [What moved]
  - [Detail]
- Open questions:
  - [Question]

### What I drove this week
- [plain bullet]
```

---

## Step 4 — Confirm

> "Weekly summary written to `[path]`. Week of [Mon date]–[Fri date]. [N] open to-dos. [Any notable flags.]"
