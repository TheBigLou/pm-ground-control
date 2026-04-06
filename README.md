# pm-ground-control

AI-powered daily operating system for product managers.

Clone this repo, run `/setup`, and your workspace is ready. At the end of each day, run `/daily-summary` to get a structured synthesis of your meetings, Slack, and project activity — written directly to your daily note.

---

## What you get

| Skill | What it does |
|---|---|
| `/setup` | One-time onboarding: creates your workspace, populates memory files, installs skills |
| `/daily-summary` | End-of-day synthesis: meetings, Slack, PRs, project tracker updates, local files → structured daily note |
| `/weekly-summary` | Synthesizes the week's daily notes into a single weekly summary |
| `/weekly-update` | Drafts your stakeholder update from the weekly summary |

---

## Prerequisites

- An AI agent that supports skills: [Claude Code](https://claude.ai/code), Codex, or Gemini CLI
- [Slack MCP](https://github.com/modelcontextprotocol/servers) connected to your agent (required for Slack sweep)
- Optionally: Asana, Linear, Jira, or Notion MCP for project tracker updates

**Recommended stack:**
- **Meeting notes:** [Granola](https://granola.ai) + [Obsidian](https://obsidian.md) + [Granola Sync community plugin](https://github.com/Quorafind/obsidian-granola) — auto-exports transcripts to local markdown. See [SETUP.md](SETUP.md) for walkthrough.
- **Daily notes:** Obsidian
- **Project tracker:** Asana or Linear (both supported out of the box)

---

## Quick start

```bash
git clone https://github.com/TheBigLou/pm-ground-control.git
cd pm-ground-control
```

Then, in your AI agent session with this directory open:

```
/setup
```

Follow the interview (10-15 minutes). When it's done, your workspace is ready and skills are installed.

At the end of your next working day:

```
/daily-summary
```

---

## MCP integrations

| Tool | What it unlocks | Notes |
|---|---|---|
| Slack | Full channel sweep, DMs, thread following | Required for Slack features |
| Asana | Project-level status updates in daily summary | Optional |
| Linear | Project-level status updates in daily summary | Optional |
| Jira | Project-level status updates in daily summary | Optional; community-contributed path |
| Notion | Project updates; weekly update output | Optional; community-contributed path |
| GitHub CLI | PR sweep in daily summary | Optional |

All integrations degrade gracefully — if an MCP isn't connected, that source is skipped and noted in the output.

See [SETUP.md](SETUP.md) for connection instructions.

---

## How it works

Your **workspace** (notes, project files, memory) lives outside the repo at a path you choose during setup. This keeps your personal data private and makes skill updates simple: pull the repo, re-run install.

**Memory files** in your workspace hold your configuration: who you are, what projects you own, your team, your Slack channels. Skills read these files at runtime — nothing is hardcoded.

To update your skills when new versions ship:

```bash
git pull
/setup  # Re-run the install step (step 9) to copy updated skills
```

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
