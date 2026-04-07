# pm-ground-control

Your meeting transcripts, Slack threads, and project activity — synthesized into structured daily notes, weekly summaries, and a running career log. Automatically, every day.

`pm-ground-control` is a set of AI skills for product managers. Connect it to your meeting transcription tool and Slack, and at the end of each day your AI agent reads everything that happened, writes a structured summary to your daily note, carries forward open action items, and logs anything worth remembering for your next performance review.

---

## What it does

At the end of each day, run `/daily-summary`. Your agent:

1. Reads every meeting transcript from today
2. Sweeps your Slack channels and DMs for decisions, action items, and signals relevant to your projects
3. Checks your project tracker for status updates
4. Scans any docs or specs you edited
5. Writes a structured daily note — decisions made, open questions, to-dos, and a narrative of what actually mattered

At the end of each week, `/weekly-summary` synthesizes the daily notes into a single weekly view. `/weekly-update` drafts your stakeholder update from that. `/log-accomplishment` captures a win to your career log in STAR format. `/accomplishments-review` combs through your weekly summaries and accomplishments log to create resume bullets and performance review narratives when you need them.

---

## What gets created on your computer

Setup creates a workspace folder at a path you choose — separate from the repo, so your notes stay private. Here's what it looks like:

```
~/Documents/WorkNotes/
├── daily-notes/
│   └── 2026/
│       ├── 04/
│       │   └── 2026-04-03.md       ← written by /daily-summary each day
│       └── W14/
│           └── 2026-W14.md         ← written by /weekly-summary each week
├── meeting-notes/
│   └── 2026-04-03/
│       ├── Onboarding v2 Kickoff.md
│       ├── Product _ Eng Sync.md   ← auto-exported by Granola
│       └── API Rate Limiting RFC Review.md
├── memory/
│   ├── me.md                       ← your identity, tools, and preferences
│   ├── projects.md                 ← active projects with Slack + tracker links
│   ├── teams.md                    ← people, roles, and ownership
│   ├── slack-channels.md           ← channel list with tier weights
│   ├── people-signal.md            ← high-signal people prioritized in sweeps
│   └── custom-sources.md           ← additional sources for daily summary
├── _accomplishments/
│   └── 2026.md                     ← running career log, updated by /log-accomplishment
├── _explorations/                  ← research and investigations
├── _reference/                     ← downloaded docs, third-party context
├── _strategy/                      ← vision docs and planning artifacts
├── growth/
│   └── onboarding-v2/
│       ├── specs/
│       ├── notes/
│       ├── gtm/
│       ├── prototypes/
│       └── external.md             ← customer quotes, competitor notes
└── platform/
    └── data-pipeline-migration/
        ├── specs/
        ├── notes/
        ├── gtm/
        └── prototypes/
```

Project folders are organized by team, with a consistent structure for specs, notes, GTM docs, and prototypes. `/setup` creates all of this from an interview — no manual configuration.

<img width="787.5" height="695.5" alt="daily-note" src="https://github.com/user-attachments/assets/117c83e6-d28b-408a-afae-87025e4989e2" />
<img width="787.5" height="695.5" alt="weekly-note" src="https://github.com/user-attachments/assets/4f80521b-076e-499b-9f20-af4331949803" />
<img width="787.5" height="506.5" alt="accomplishment-log" src="https://github.com/user-attachments/assets/cac79241-2257-42e6-a72d-32b6bbefb508" />


---

## Skills

| Skill | When to run | What it does |
|---|---|---|
| `/setup` | Once | Creates your workspace, populates memory files, installs skills, configures MCP connections |
| `/daily-summary` | End of each day | Reads transcripts, Slack, PRs, and tracker updates → structured daily note with to-dos |
| `/weekly-summary` | End of each week | Synthesizes daily notes into a weekly view |
| `/weekly-update` | Weekly | Drafts your stakeholder update from the weekly summary |
| `/log-accomplishment` | As wins happen | Captures a career-relevant win to your log in STAR format |
| `/accomplishments-review` | Review season | Synthesizes your log into resume bullets and review narratives |
| `/add-source` | As needed | Adds a new data source (MCP, CLI, file path, HTTP endpoint) to your daily summary |

---

## Prerequisites

- An AI agent that supports skills: [Claude Code](https://claude.ai/code), [Codex](https://openai.com/codex/), or [Gemini CLI](https://geminicli.com/)
- [Slack MCP](https://github.com/modelcontextprotocol/servers) connected to your agent (required for Slack sweep)
- Optionally: Asana, Linear, Jira, or Notion MCP for project tracker updates

**Recommended stack:**
- **Meeting transcription:** [Granola](https://granola.ai) + [Obsidian](https://obsidian.md) + [Granola Sync community plugin](https://github.com/Quorafind/obsidian-granola) = auto-exports transcripts to local markdown after every meeting, no manual steps
- **Daily notes:** Obsidian
- **Project tracker:** Asana or Linear (both supported out of the box)

---

## Quick start

### If you use a desktop app (Claude Code, Cursor, etc.)

1. Click **Code → Download ZIP** at the top of this page
2. Unzip it somewhere permanent — e.g. `~/Documents/pm-ground-control/`
3. Open your AI agent and open the `pm-ground-control` folder as your working directory
4. Type `/setup` and follow the interview

`/setup` handles everything else: workspace creation, memory files, MCP configuration, and skill installation. It takes about 10–15 minutes.

### If you're comfortable with the terminal

```bash
git clone https://github.com/TheBigLou/pm-ground-control.git
cd pm-ground-control
```

Then open the folder in your AI agent and run `/setup`.

---

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
| Jira | Project-level status updates in daily summary | Optional; official Atlassian Rovo MCP |
| Notion | Project updates; weekly update output | Optional; official Notion MCP |
| GitHub CLI | PR sweep in daily summary | Optional |

All integrations degrade gracefully — if an MCP isn't connected, that source is skipped and noted in the output.

See [SETUP.md](SETUP.md) for connection instructions.

---

## How it works

Your **workspace** (notes, project files, memory) lives outside the repo at a path you choose during setup. This keeps your personal data private and makes skill updates simple: pull the repo, re-run install.

**Memory files** in your workspace hold your configuration: who you are, what projects you own, your team, your Slack channels. Skills read these files at runtime. Nothing is hardcoded.

To update your skills when new versions ship:

```bash
git pull
/setup  # Re-run the install step (step 9) to copy updated skills
```

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
