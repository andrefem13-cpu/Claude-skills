---
name: skill-builder
description: Build a new Claude skill from scratch. Guides through naming, trigger phrases, step-by-step instructions, and writes a valid SKILL.md to .claude/skills/. Use when the user says "build a skill", "create a skill", "new skill", "make a /command", or "I want a repeatable workflow for X."
---

# Skill Builder

Guide the user through building a reusable Claude skill (slash command) and write the SKILL.md file.

## Step 1: Gather Requirements

Ask one question at a time using `AskUserQuestion`. Collect:

1. **What does this skill do?** — One-sentence description of the behavior it automates.
2. **What triggers it?** — What phrases or slash command should invoke it? (e.g., `/dump`, "brain dump", "end of shift")
3. **What are the steps?** — Walk through the workflow the skill should execute. Ask the user to describe it as if explaining to a colleague.
4. **What tools does it need?** — Does it need Bash, Read/Write files, web search, agents?
5. **What should the output look like?** — What does "done" look like for this skill?

## Step 2: Name the Skill

Derive a kebab-case skill name from the description:
- Short and memorable (2–3 words max)
- Verb-first when possible: `shift-brain-dump`, `grill-me`, `context-handoff`
- No spaces, no special characters

Confirm with the user before proceeding.

## Step 3: Draft the SKILL.md

Write a complete SKILL.md using this structure:

```markdown
---
name: [skill-name]
description: [One-sentence description. Include all trigger phrases here so the runtime auto-fires it. End with: "Invoke with /[name]."]
---

# /[skill-name]

[One-line summary of what this does.]

## When to Use

- [Trigger phrase 1]
- [Trigger phrase 2]
- [Proactive trigger if applicable]

## Steps

### 1. [First step]
[Instructions]

### 2. [Second step]
[Instructions]

## Output Format

[What the output looks like]

## Constraints

- [Any rules or guardrails]
```

## Step 4: Write the File

1. Check if `~/.claude/skills/[skill-name]/` exists. If not, create it.
2. Write the SKILL.md:

```bash
mkdir -p ~/.claude/skills/[skill-name]
```

Then use the Write tool to create `~/.claude/skills/[skill-name]/SKILL.md`.

## Step 5: Confirm

Tell the user:
> "Skill `/[name]` is ready at `~/.claude/skills/[skill-name]/SKILL.md`. Restart Claude Code or open a new session to activate it. Test it by typing `/[name]`."

Offer to refine the trigger phrases or adjust the steps based on their feedback.
