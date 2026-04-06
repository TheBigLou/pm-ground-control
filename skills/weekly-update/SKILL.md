---
name: weekly-update
description: Use when drafting the weekly stakeholder update. Run after weekly-summary is complete.
---

# Weekly Update

Draft a stakeholder-facing weekly update from the weekly summary. Format is configured in `memory/me.md`.

## Step 0 — Read inputs

1. Read `memory/me.md` for:
   - Weekly update format (Slack post / email / Notion page / other)
   - Weekly update audience (manager, team, etc.)
   - Your name and role

2. Find this week's weekly summary. Look for `{{WORKSPACE}}/daily-notes/YYYY/WW/YYYY-WW.md`. If it doesn't exist, prompt: "Weekly summary not found. Run `/weekly-summary` first, then come back."

3. Read the summary in full.

---

## Step 1 — Draft the update

Write a stakeholder-facing update from the weekly summary. This is external-facing — write for the audience, not as internal notes.

**Tone and style:**
- Concise and direct — most readers skim
- Lead with outcomes, not activity
- Use plain language — no jargon or internal acronyms without definition
- Confident, not defensive

**Structure (adapt to format):**

```
**Week of [Mon date]–[Fri date]**

**Summary**
[2-3 sentences: what the week added up to, the most important thing that happened]

**[Project Name]**
[1-3 bullets: what moved, what was decided, where it stands]

**[Project Name]**
[1-3 bullets]

**Looking ahead**
[2-3 bullets: what's on deck next week, any upcoming decisions or milestones]
```

Omit projects with no activity. Do not include the internal to-do list. Do not include "What I drove" verbatim — fold the most important items into the project summaries where they add context.

---

## Step 2 — Format for delivery

Apply the format from `memory/me.md`:

**Slack post:**
- Use Slack markdown: `*bold*`, `_italic_`, bullet points with `-`
- Keep the whole post skimmable in <2 minutes
- Open with the summary paragraph, then project bullets, then looking ahead

**Email:**
- Subject: `Weekly update — week of [Mon date]`
- Plain text or simple HTML — no fancy formatting
- Same structure as above

**Notion page:**
- Title: `Weekly Update — Week of [Mon date]`
- Use Notion heading styles (H2 for project sections)
- Add date created

**Other:** Follow whatever format the user specified in setup.

---

## Step 3 — Review and send

Present the draft to the user. Ask: "Does this look right, or would you like any changes?"

If the user approves:
- **Slack:** Offer to post via `slack_send_message` to the appropriate channel or DM (ask for confirmation of destination)
- **Email:** Offer to create a draft via `gmail_create_draft` (ask for recipient)
- **Notion:** Offer to create the page (ask for the parent page)
- **Other:** Output the final text for the user to send manually

---

## Step 4 — Confirm

> "Weekly update drafted for week of [Mon date]–[Fri date]. [Sent to / Saved as draft for / Copied to clipboard for] [destination]."
