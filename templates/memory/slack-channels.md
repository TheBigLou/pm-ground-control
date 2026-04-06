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
