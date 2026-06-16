---
name: reminders
description: Check and act on Apple Reminders — reads Claude Inbox for pending tasks and Claude Output for results. Automatically dispatches agents for new inbox items. Use when user says /reminders, "check reminders", "what came in", or to start the watcher via /loop.
user_invocable: true
---

# Reminders Bridge

Check Apple Reminders for new tasks from your phone.

## Step 1: Run the Check Script

Run this exact command:

```bash
~/.claude/reminder-check.sh
```

This script:
- Queries "Claude Inbox" for incomplete reminders
- Filters out already-seen IDs (tracked in `~/.claude/reminder-seen.txt`)
- Marks new reminders as complete in Reminders (prevents re-processing)
- Queries "Claude Output" for flagged items (questions needing your input)
- Outputs pipe-delimited lines: `NEW|title|notes` or `FLAGGED|title|notes`
- Returns exit code 1 if nothing new (no output)

## Step 2: Handle Results

**If exit code is 1 (nothing new):** Say nothing. Return silently. Do NOT output "nothing new" or any status message — just stop.

**For each `NEW|title|notes` line:** Dispatch a background Agent to handle the task:

```
Agent tool:
  description: "Reminder: [first 3 words of title]"
  run_in_background: true
  prompt: |
    You are working autonomously on a task sent from the user's phone via Apple Reminders.

    TASK TITLE: [title from the line]
    TASK NOTES: [notes from the line]

    INSTRUCTIONS:
    1. Read ~/.claude/project-manifest.md to fuzzy-match this task to a project.
    2. If it matches an existing project, cd into that folder and read SESSION_LOG.md and CLAUDE.md.
    3. If it's a new idea, create a new project in ~/Desktop/Claude Projects/ with a kebab-case name.
    4. Do the work. Build, prototype, scaffold. Be bold.
    5. Follow existing project conventions.

    GUARDRAILS:
    - Do NOT delete files
    - Do NOT git push or deploy
    - Do NOT post to external APIs
    - Do NOT modify other projects

    WHEN DONE:
    1. Run: ~/.claude/reminder-output.sh "[project-name] Done — summary" "Details of what was built"
       Or for questions: ~/.claude/reminder-output.sh "[project-name] Question — what you need" "Context" "true"
    2. Update SESSION_LOG.md with what you did.
```

**For each `FLAGGED|title|notes` line:** Display it prominently — these are questions from agents that need your input.

## Step 3: Report

- If agents were dispatched: "Dispatched agent for: [title]" (one line per task)
- If flagged items exist: "Needs your input: [title]"
- If nothing at all: say nothing (silent return)
