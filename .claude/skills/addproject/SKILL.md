---
name: addproject
description: >
  Convert a conversation into a structured, forward-looking project brief — and optionally set up scheduled tasks.
  Use this skill whenever the user says "turn this into a project", "save this as a project", "create a project from this",
  "addproject", or when a conversation has produced a working system/integration and the user wants to capture
  the ongoing work (not the setup history). Also trigger when the user says "what's left to do on this" in the
  context of a multi-session effort, or wants to define recurring/automated tasks from work already done.
  This is NOT a session snapshot — it is a living project brief that strips completed one-time setup and focuses
  on active tasks, scheduled automation, and forward momentum.
---

# Add Project Skill

Convert the current conversation into a structured project brief. The goal is a **forward-facing document** —
not a history of what happened, but a clear picture of what this project is and what needs to happen next.

---

## Core Distinction

This skill produces something different from a context handoff:

| context-handoff | addproject |
|---|---|
| Session snapshot | Persistent project brief |
| "What happened + resume here" | "What this project IS + what's next" |
| One-time use | Updated as project evolves |
| Includes setup steps | Strips completed setup entirely |
| No scheduled task integration | Defines scheduled/recurring tasks |

---

## Step 1: Extract Project Signal

Scan the conversation and identify:

1. **What is the project?** — The ongoing system, integration, or automation being built. One sentence.
2. **What's been completed and can be safely ignored?** — Setup, auth, installation, config. These are DONE. Do not carry them forward.
3. **What is the active work?** — Tasks currently in flight or about to start.
4. **Are there scheduled or recurring tasks?** — Anything the user wants to run automatically on a schedule.
5. **What tools/integrations are confirmed working?** — MCPs, APIs, credentials. Just the names + status, no setup detail.
6. **What are the user's prefs and constraints?** — Tone, workflow rules, anything stated as "always do this."

---

## Step 2: Generate the Project Brief

Use this exact structure. Every section should be tight — this is a working document, not a narrative.

```
# [Project Name]

## What This Is
[One sentence. What does this project do / automate / produce?]

## Active Tasks
- [ ] [Task 1 — specific, actionable]
- [ ] [Task 2]
...

## Scheduled Tasks
[List any recurring/automated tasks. If none yet defined, note "TBD — use schedule skill to define."]
| Task | Trigger | Status |
|------|---------|--------|
| ... | ... | ... |

## Tools + Integrations
| Tool | Status |
|------|--------|
| ... | ✅ Connected / ⚠️ Needs attention |

## Constraints + Prefs
- [User tone/style rules]
- [Workflow constraints]

## Done (don't revisit)
- [Brief list of completed one-time steps — setup, auth, config]
  These are closed. Don't re-explain or carry them forward.
```

---

## Step 3: What's Next

After producing the brief, offer two short, tailored observations — not generic advice, but things actually grounded in this project:

1. **What would make this project stronger** — Is anything missing or vague? Are there gaps in the Active Tasks, unclear owners, integrations that still need attention, or a task that's too broad to act on? If everything looks solid, say so briefly.

2. **A direction to consider next** — Based on what the project actually is, what's the highest-leverage move? This might be:
   - Setting up a scheduled task (only suggest this if the project clearly involves recurring automation)
   - Producing a first deliverable
   - Defining the workflow in more detail
   - Nothing specific — "project is well-defined, ready to execute"

Keep both points brief (1–2 sentences each). Don't pad. If there's nothing useful to say on either front, skip it.

If the project does involve obvious recurring automation, offer to invoke the `schedule` skill:
> "This looks like a good candidate for a scheduled task — want me to set that up now?"

---

## Step 4: Deliver the Brief

Save the project brief to `/mnt/user-data/outputs/project-[name]-[YYYY-MM-DD].md`.

---

## Updating an Existing Project Brief

If the user says "update the project" or "mark X as done" or similar:
1. Read the existing project brief file
2. Apply the update (check off tasks, add new ones, update tool status, etc.)
3. Save back to the same file
4. Confirm what changed

---

## Quality Check

Before delivering, verify:
- [ ] No setup/installation steps in "Active Tasks" — those belong in "Done"
- [ ] Every active task is specific and actionable (not vague like "figure out messaging")
- [ ] Scheduled tasks table is present even if empty (signals the project is automation-aware)
- [ ] "Done" section is short — it's a tombstone, not a recap
- [ ] The brief could be handed to a fresh Claude and immediately acted on
