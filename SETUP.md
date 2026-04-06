# Setup Guide

This guide covers everything you need to configure before running `/setup` — and what to do afterward to connect your tools.

---

## 1. Meeting notes: Granola + Obsidian + Granola Sync

The recommended setup auto-exports your Granola meeting transcripts to local markdown files, organized by date. This is what the `daily-summary` skill reads.

> **Note:** If you use a different meeting notes tool, skip to the folder structure section below and configure your export path manually.

### What you need
- [Granola](https://granola.ai) — AI meeting recorder (Mac)
- [Obsidian](https://obsidian.md) — local markdown editor
- [Granola Sync community plugin](https://github.com/Quorafind/obsidian-granola) — syncs Granola transcripts to Obsidian

### Setup steps

*(Granola + Obsidian + Granola Sync walkthrough to be added — if you use this stack and want to contribute setup instructions, see CONTRIBUTING.md)*

### Expected output format

After setup, your meeting notes should appear as:

```
{{WORKSPACE}}/meeting-notes/
  2026-04-07/
    Team standup.md
    1_1 with manager.md
  2026-04-08/
    Product review.md
```

Each file should have a YAML frontmatter block with a `created` field (timestamp). The `daily-summary` skill uses this for meeting times.

---

## 2. MCP connections

Each MCP tool you connect unlocks additional signals in the daily summary.

### Slack (recommended)

The Slack MCP gives the daily summary access to your channels, DMs, and threads.

**Claude Code:**
Add to your Claude Code MCP config (`~/.claude/mcp.json` or via `claude mcp add`):

```json
{
  "slack": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-slack"],
    "env": {
      "SLACK_BOT_TOKEN": "xoxb-...",
      "SLACK_TEAM_ID": "T..."
    }
  }
}
```

See the [MCP Slack server docs](https://github.com/modelcontextprotocol/servers/tree/main/src/slack) for how to create a Slack app and get your bot token.

### Asana

Used for project-level status updates in the daily summary.

```json
{
  "asana": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-asana"],
    "env": {
      "ASANA_ACCESS_TOKEN": "..."
    }
  }
}
```

### Linear

Used for project-level status updates in the daily summary.

```json
{
  "linear": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-linear"],
    "env": {
      "LINEAR_API_KEY": "..."
    }
  }
}
```

### Jira

Uses the [official Atlassian Rovo MCP server](https://mcp.atlassian.com/v1/mcp) — cloud-hosted, OAuth-authenticated, covers Jira, Confluence, and Compass.

**Prerequisites:** Node.js v18+, an Atlassian Cloud account.

Add to your MCP config:

```json
{
  "atlassian": {
    "command": "npx",
    "args": ["-y", "mcp-remote@latest", "https://mcp.atlassian.com/v1/mcp"]
  }
}
```

On first connection, a browser window will open for OAuth authentication. After that it stays authenticated. Your existing Jira permissions apply — the MCP can only access what you can access.

### Notion

Uses the [official Notion MCP server](https://github.com/makenotion/notion-mcp-server).

Add to your MCP config:

```json
{
  "notionApi": {
    "command": "npx",
    "args": ["-y", "@notionhq/notion-mcp-server"],
    "env": {
      "NOTION_TOKEN": "ntn_..."
    }
  }
}
```

Get your token: go to [notion.so/profile/integrations](https://www.notion.so/profile/integrations), create an Internal Integration, and copy the secret (starts with `ntn_`). Then grant the integration access to the pages or databases you want it to read.

> **Note:** Notion has signaled they may sunset this local server in favor of a remote OAuth-based server. It works today, but watch for changes if you upgrade.

### GitHub CLI

The `gh` CLI is used for PR sweeps. Install via Homebrew and authenticate:

```bash
brew install gh
gh auth login
```

---

## 3. Workspace folder structure

During `/setup`, you'll choose how project folders are organized:

**Default (recommended):**
```
{{WORKSPACE}}/
  {{team}}/
    {{project}}/
      specs/          ← PRDs, technical specs, requirements
      notes/          ← meeting follow-ups, research, scratch
      gtm/            ← launch briefs, enablement docs
      prototypes/     ← prototype files, mockup exports
      external.md     ← links to Google Docs, Figma, etc.
```

**Flat:** `{{WORKSPACE}}/{{team}}/{{project}}/` with no subfolders.

**Custom:** You define the structure. Note that the `daily-summary` skill's local file sweep covers the workspace root, so all your files will still be found.

---

## 4. Updating skills

When new versions of skills are released:

```bash
cd pm-ground-control
git pull
```

Then re-run the install step. In your AI agent session with the repo open:

```
/setup
```

When prompted, skip the interview steps (or press through with your existing answers) and proceed to step 9 (Install skills) to copy the updated files.

---

## 5. Adding teams or projects after setup

You don't need to re-run `/setup` for minor changes. Edit your memory files directly:

- **New project:** Add an entry to `{{WORKSPACE}}/memory/projects.md` following the existing format. Create the project folder with the standard template. Add its Slack channel to `{{WORKSPACE}}/memory/slack-channels.md` under Tier 1.
- **New team member:** Add a row to `{{WORKSPACE}}/memory/teams.md`.
- **New Slack channel:** Add it to the appropriate tier in `{{WORKSPACE}}/memory/slack-channels.md`.

Re-run `/setup` if you want the interview flow to help you (e.g. adding a whole new team).
