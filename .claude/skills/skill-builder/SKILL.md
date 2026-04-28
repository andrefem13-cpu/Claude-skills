#Claude-skills
-----

## name: skill-builder
description: >
Build a new Claude skill from scratch using Andre’s PRN framework — interviewing the user about
their workflow, pain point, or use case and outputting a ready-to-use SKILL.md file.
Use this skill whenever the user says “build me a skill”, “new skill”, “turn this into a skill”,
“make this a skill”, “create a skill for”, or types “/skill”. Also trigger when the user describes
a repetitive workflow or frustration and it’s clear that a reusable skill would solve it — even
if they don’t explicitly ask. If a conversation has produced a useful output pattern more than once,
proactively suggest: “Want me to turn this into a skill?”

# Skill Builder (Andre’s PRN Framework)

Interviews the user about a workflow or pain point, then outputs a production-ready SKILL.md
using Andre’s PRN template — optimized for clinical, educational, and brand workflows.

-----

## Step 1: Identify the Pain Point

Start with one question. Do NOT ask multiple questions at once.

> “What’s the workflow — or the thing you keep rebuilding from scratch?”

If the current conversation already contains a repeated pattern or workflow, extract it directly
and confirm with the user before proceeding:

> “Looks like you’ve been doing [X] a few times. Want to turn that into a skill?”

-----

## Step 2: Interview (PRN Questions)

Ask these one at a time, only as needed. Skip any that are already obvious from context.

1. **Trigger** — “What would you say to Claude to kick this off? Any slash command you’d want?”
1. **Input** — “What does Claude need to do this? (paste, upload, just a prompt?)”
1. **Output** — “What does done look like? (format, length, destination — Obsidian, Epic, tweet, file?)”
1. **Voice/constraints** — “Anything Claude always gets wrong that this skill should fix?”
1. **Example** — “Can you give me one example of the input and what perfect output looks like?”

Stop interviewing when you have enough to write the skill. Two or three answers is usually enough.

-----

## Step 3: Write the SKILL.md

Use this exact template. Fill every section — do not leave placeholders.

```markdown
---
name: [kebab-case-name]
description: >
  [2-3 sentences. Start with what it does. Then list specific trigger phrases.
  Be slightly pushy — undertriggering is the main failure mode.
  Include slash command if defined.]
---

# [Skill Title]

[One sentence: what this does and why it exists.]

---

## When to Use

- [Trigger phrase or context 1]
- [Trigger phrase or context 2]
- [Proactive trigger: when to suggest this without being asked]

---

## Inputs Needed

| Input | Required? | How to provide |
|-------|-----------|----------------|
| [input 1] | Yes/Optional | [paste / upload / prompt] |
| [input 2] | Yes/Optional | [paste / upload / prompt] |

---

## Steps

1. [Step 1 — what Claude does first]
2. [Step 2]
3. [Step 3]
4. [Output step — format and destination]

---

## Output Format

- **Format:** [markdown / React / plain text / Epic SmartPhrase / tweet / .md file]
- **Length:** [target length or range]
- **Destination:** [inline / Obsidian / outputs folder / copy-paste]
- **Voice:** [Andre's coaching voice / clinical / neutral / brand]

---

## Example

**Input:**
[Concrete example input]

**Output:**
[What perfect output looks like — abbreviated if long]

---

## Quality Check

Before delivering output, ask:
- [ ] Does this match the voice and format defined above?
- [ ] Is every required input present? If not, ask before proceeding.
- [ ] Is the output going to the right destination?
```

-----

## Step 4: Deliver Output

Always deliver in both formats:

### Inline

Show the complete SKILL.md in a fenced code block so the user can copy directly into their repo:

```
```markdown
[full SKILL.md here]
```
```

### File

Save to `/mnt/user-data/outputs/[skill-name]/SKILL.md` and use `present_files` to deliver.

Tell the user:

> “Drop the `[skill-name]/` folder into your `.claude/skills/` directory in your repo and push. It’s live.”

-----

## Step 5: Repo Placement Reminder

After delivering, always include this one-liner:

```
Repo path: your-repo/.claude/skills/[skill-name]/SKILL.md
```

If the user doesn’t have a repo set up yet, offer:

> “Want me to generate a README and folder structure for your full skills repo?”

-----

## Andre’s Skill Taxonomy (for suggestions)

When helping identify what skill to build, reference these domains:

**Clinical:** shift-handoff, bedside-explainer, clinical-reasoning-coach, literature-distiller
**Education:** student-feedback, osce-case-builder, advising-prep, ms3-teaching-note
**QI/Admin:** qi-case-formatter, epic-smartphrase-builder, billing-mdm-support
**Brand:** bloodsweatxed-post, opmed-draft, thread-builder
**Meta/Workflow:** obsidian-capture, context-handoff, skill-builder (this one)

If the user’s pain point maps to one of these, name it and offer to build it directly.