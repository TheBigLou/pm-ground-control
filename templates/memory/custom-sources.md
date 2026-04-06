---
name: Custom sources
description: Additional data sources included in the daily summary — MCP tools, CLI commands, local file paths, or HTTP endpoints defined by the user
type: reference
---

# Custom Sources

Each entry below is checked by the daily summary. Add entries with `/add-source` or edit this file directly.

## Format

```
## Source: [Name]

- **Type:** mcp | cli | local-files | http
- **Access:** [tool call, shell command, glob pattern, or URL]
- **Extract:** [what to pull out — decisions, status updates, action items, etc.]
- **Filter:** today | since-last-working-day | always
- **Relevant to:** all | [comma-separated project names or keywords]
- **Notes:** [optional context for the subagent]
```

---

<!-- Add your sources below this line. Examples:

## Source: Confluence team space

- **Type:** mcp
- **Access:** `confluence_search space="TEAM" query="updated:today"`
- **Extract:** Pages updated today — title, author, summary of changes
- **Filter:** today
- **Relevant to:** all
- **Notes:** Engineering team's internal wiki

## Source: On-call incidents

- **Type:** cli
- **Access:** `pagerduty incidents list --status triggered,acknowledged --limit 10`
- **Extract:** Active incidents — title, severity, assigned team
- **Filter:** always
- **Relevant to:** all
- **Notes:** Check even on quiet days — any P1/P2 is worth surfacing

## Source: Inbox folder

- **Type:** local-files
- **Access:** `~/Documents/Inbox/*.md`
- **Extract:** File name, first 5 lines, apparent topic
- **Filter:** today
- **Relevant to:** all
- **Notes:** Capture anything dropped here during the day

-->
