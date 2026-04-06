---
name: log-accomplishment
description: Use when capturing a specific accomplishment for job searching, performance reviews, or interview prep. Triggered by "log accomplishment", "/log-accomplishment", "add to my accomplishments", or "remember this for my resume".
---

# Log Accomplishment

Capture a career-relevant accomplishment in the persistent log.

## Step 0 — Locate the log

Read `memory/me.md` for the workspace path.

- **Log directory:** `{{WORKSPACE_PATH}}/_accomplishments/`
- **Active log file:** `YYYY.md` where YYYY is the current year. Create the file if it doesn't exist.

---

## Step 1 — Get the raw material

If the user described what happened, use it. If they just invoked the skill with no context, ask:

> "What happened? Give me a quick description and I'll help frame it."

---

## Step 2 — Extract five elements

Probe for anything missing. Do not write the entry until you have all five:

1. **What you specifically did** — Action verb + object. Not "we shipped X" but "I designed/led/resolved/shipped X."
2. **Situation** — Why was this needed? What was the problem, gap, or opportunity?
3. **Impact** — What changed? What was unblocked, delivered, or improved?
4. **Scale or metrics** — Any numbers? Users affected, time saved, error rate reduced, adoption rate, revenue influenced. Even rough estimates are valuable ("~50 customers", "cut review time in half").
5. **Date** — Today unless specified otherwise.

If the user says they don't know a metric, write "metric TBD — [description of what to look up]" and flag it in the entry.

---

## Step 3 — Write the entry

Append to `_accomplishments/YYYY.md` (current year):

```
## YYYY-MM-DD — [Project/Area]

**What I did:** [specific action with strong verb]

**Context:** [1-2 sentences: what the situation was and why it mattered]

**Impact:** [what changed or was enabled; include metrics if available]

**Draft bullet:** "[Action verb] [what] resulting in [outcome]"

**STAR notes:**
- **S:** [Situation — the context]
- **T:** [Task — what you were responsible for]
- **A:** [Action — what you specifically did]
- **R:** [Result — outcome, ideally with metric]

---
```

---

## Action verb reference

| Category | Verbs |
|---|---|
| Strategy / decisions | Defined, Designed, Prioritized, Scoped, Recommended, Proposed, Established |
| Alignment | Aligned, Negotiated, Facilitated, Resolved, Advocated, Drove consensus |
| Delivery | Shipped, Launched, Deployed, Completed, Delivered, Produced |
| Analysis | Diagnosed, Identified, Investigated, Synthesized, Surfaced |
| Improvement | Reduced, Increased, Streamlined, Eliminated, Automated, Optimized |
| Leadership | Led, Drove, Initiated, Coordinated, Influenced, Unblocked |

---

## Step 4 — Confirm

Reply with:

> "Logged. Draft bullet: [the bullet]"

If a metric is missing and worth tracking down, flag it: "Worth finding the [X] number to strengthen this one."
