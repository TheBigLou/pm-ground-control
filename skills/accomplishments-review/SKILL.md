---
name: accomplishments-review
description: Use when preparing for a job search, updating a resume, prepping for interviews, or doing a performance review. Triggered by "review accomplishments", "/accomplishments-review", "prepare for interviews", "update my resume", "performance review prep".
---

# Accomplishments Review

Synthesize the accomplishments log into job-search-ready outputs.

## Step 0 — Locate files and calibrate

Read `memory/me.md` for the workspace path.

- **Log directory:** `{{WORKSPACE_PATH}}/_accomplishments/` — year-based files (`2025.md`, `2026.md`, etc.)
- **Weekly summaries:** `{{WORKSPACE_PATH}}/daily-notes/` — files named `YYYY-WW.md`

Ask (or infer from context):

- **Output type:** resume bullets, interview stories, performance review, or all three?
- **Time range:** all time, last N months, or a specific date range?
- **Focus area:** all projects, or a specific theme?
- **Role level:** if not set, ask — this determines the importance bar.

**Importance bar by level:**

| Level | What clears the bar | What to drop |
|---|---|---|
| Junior / IC1-2 | Delivered on a commitment, owned a task end-to-end, proactively flagged a problem | Work done entirely under direction with no independent contribution |
| Mid / IC3-4 | Took full ownership of a project, drove a metric, navigated ambiguity, contributed across teams | Tasks completed but not owned; work that required no independent judgment |
| Senior / IC5 | Made a call that changed direction, shipped something with measurable business impact, unblocked cross-team work | Routine execution, tickets completed without broader impact |
| Staff / Principal | Set or shifted strategy, resolved contested issues with lasting impact, drove alignment at exec level, shaped how the team thinks about a problem area | Docs written, meetings attended, tickets moved, routine reviews |
| Executive | Org outcomes, hiring decisions, culture shifts, decisions that set direction for multiple teams | Individual project execution, anything below the org or strategic level |

When in doubt: "Could a more junior person have done this, and would it be forgettable in 6 months?" If yes, drop it.

---

## Step 1 — Close open loops

Scan two sources for open loops:

**Prior review files:** Glob `_accomplishments/review-*.md`. For each, look for entries that were in-progress, unresolved, or flagged.

**Prior year files:** If the time range starts mid-year or later, also read the year file(s) immediately before the range start. Scan for entries where:
- A project was described as in-progress or building toward something
- A metric was flagged as "TBD"
- Alignment or a decision was described as pending
- A proposal or initiative had not yet landed

These are cross-year open loops — work that started in one year file and may have resolved in another.

Present all open loops as a short list and ask for closure on any the user can provide. Add confirmed outcomes to the current output under a **"Closed loops"** section before proceeding.

---

## Step 2 — Gather raw material

Read two sources in parallel:

**Source A: Year files** — Glob `_accomplishments/????.md` and read only the files whose year falls within the requested time range. For each entry extract: date, project/area, action verb, impact, metrics, draft bullet.

**Source B: Weekly summaries** — Glob `daily-notes/**/YYYY-WW.md` within the time range. For each file, extract the `### What I drove this week` section only. These are already filtered for personal agency — use them as corroborating signal and as a source for entries that may not have been formally logged.

If a weekly summary item has no corresponding log entry, flag it as a logging gap and include it in the output using the weekly summary text as the draft.

---

## Step 3 — Filter by importance

Apply the calibration from Step 0. For each raw entry, assign:

- **Include** — clears the bar
- **Weak** — marginal; include only if the log is thin, otherwise drop
- **Drop** — routine execution

For Staff/Principal: be aggressive. 10 strong entries beat 25 mediocre ones.

---

## Step 4 — Group by theme

Cluster included entries into 4–6 themes based on what's actually in the log. For a PM, suggested groupings (adjust to fit):

- **Product strategy and definition** — specs, PRDs, prioritization calls, roadmap decisions
- **Cross-functional alignment** — driving agreement, unblocking stuck decisions
- **Customer and business impact** — features shipped, adoption driven, revenue-connected work
- **Data and analytics** — measurement frameworks, analytics features, data-driven decisions
- **Process and tooling** — workflows, documentation, team infrastructure
- **Platform / AI** — platform strategy, data model decisions, AI-native features

---

## Step 5 — Generate outputs

### Resume bullets

For each theme, produce 2–4 polished bullets:
- Lead with a strong action verb
- Include the most specific metric available
- One line, ~100 characters
- No internal jargon — readable by an external recruiter

```
**[Theme]**
• [Bullet]
• [Bullet]
```

Flag any metric placeholder: ⚠️ *needs metric.*

### Interview stories (STAR format)

Select 3–5 entries with the clearest impact, personal agency, and narrative arc. For each:

```
**[Story title]**

**Situation:** [2–3 sentences: what was happening, why it mattered]
**Task:** [What you were specifically responsible for]
**Action:** [3–5 sentences: your specific choices and actions]
**Result:** [Lead with the metric or most concrete outcome, then qualitative impact]
**Good for:** [Interview question types, e.g. "Tell me about a time you drove alignment"]
```

### Performance review summary (if requested)

One-page narrative:
1. **What I shipped** — bullet list of delivered work
2. **How I drove impact** — 2–3 paragraphs on business outcomes
3. **How I worked with others** — cross-functional contributions
4. **What I'm building toward** — optional forward-looking paragraph

---

## Step 6 — Flag gaps

After generating outputs, note:
- Entries with "metric TBD" worth following up on
- Themes with fewer than 2 entries (underlogged or genuinely thin?)
- Time gaps of 2+ weeks in the log (quiet period or logging missed?)
- Weekly summary items with no log entry (candidates for back-logging)

---

## Step 7 — Save

Offer to save the full output — closed loops, resume bullets, interview stories, performance review (if requested), and flags — to `_accomplishments/review-YYYY-MM-DD.md`.
