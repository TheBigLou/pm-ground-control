---
name: add-source
description: Use when adding a new data source to the daily summary — MCP tools, CLI commands, local file paths, or HTTP endpoints. Triggered by "add source", "/add-source", "add to my daily summary", or "connect [tool] to my daily summary".
---

# Add Source

Add a new data source to your daily summary. The source will be checked automatically each time `/daily-summary` runs.

## Step 0 — Locate custom-sources.md

Read `memory/me.md` for the workspace path. The file lives at `{{WORKSPACE_PATH}}/memory/custom-sources.md`. If it doesn't exist, create it from the template structure (see the format below).

---

## Step 1 — Name and type

Ask: "What would you like to call this source? (e.g. 'Confluence team space', 'On-call incidents', 'Inbox folder')"

Ask: "What type of source is it?
1. MCP tool — a connected tool like Confluence, PagerDuty, Intercom, etc.
2. CLI command — a terminal command like `gh`, `jira`, `kubectl`, a custom script
3. Local files — a folder or file pattern on your computer
4. HTTP endpoint — a URL you can query directly"

---

## Step 2 — Access details

Based on type, ask:

**MCP tool:**
> "What's the tool name and the call you'd use to query it? For example: `confluence_search space=\"TEAM\" query=\"updated:today\"`. If you're not sure of the exact syntax, describe what you want and I'll help construct it."

**CLI command:**
> "What's the command? I'll run it now to confirm it works:
> ```bash
> {{COMMAND}}
> ```
> Does the output look right?"

Run the command via Bash to verify it executes and returns something useful. If it errors, help the user fix it before proceeding.

**Local files:**
> "What's the path or glob pattern? For example: `~/Documents/Inbox/*.md` or `/path/to/folder/`. I'll check what's there now."

Glob the path to confirm files exist and show a sample.

**HTTP endpoint:**
> "What's the URL and method (GET/POST)? Any headers or auth needed?"

---

## Step 3 — What to extract

Ask: "What should the daily summary pull out from this source? Be specific — for example: 'pages updated today with title and author', 'open incidents with severity and assigned team', 'files modified today with a one-line summary of content'."

---

## Step 4 — Filter

Ask: "How often should this be checked?
1. Today only — only content from today
2. Since last working day — today + yesterday (or Friday if today is Monday)
3. Always — no date filter (good for things like open incident queues)"

---

## Step 5 — Relevance

Ask: "Which projects or contexts is this relevant to? This helps the daily summary know where to include it.
1. All projects
2. Specific projects (list them)"

---

## Step 6 — Preview and confirm

Show the complete entry that will be written:

```
## Source: {{NAME}}

- **Type:** {{TYPE}}
- **Access:** {{ACCESS}}
- **Extract:** {{EXTRACT}}
- **Filter:** {{FILTER}}
- **Relevant to:** {{RELEVANT_TO}}
```

Ask: "Does this look right? (yes / adjust)"

---

## Step 7 — Write to custom-sources.md

Append the confirmed entry to `memory/custom-sources.md` above the closing comment line.

---

## Step 8 — Test (optional)

Ask: "Want me to test this source now and show you what it would surface today? (yes / no)"

If yes, execute the source query and show sample output formatted as it would appear in the daily summary.

---

## Step 9 — Confirm

> "Added. '{{NAME}}' will be checked in every daily summary{{, scoped to: {{RELEVANT_TO}} if not 'all'}}. Run `/daily-summary` to see it in action."
