---
name: context-handoff
description: >
  Generate a compressed, structured context snapshot for handing off a session to a fresh Claude instance.
  Use this skill whenever the user types /handoff, says things like "summarize our context", "start a new session",
  "pick this up later", "new chat context", "hand this off", or when the conversation is running long and continuity
  is at risk. Also proactively suggest a handoff when the conversation has covered many distinct topics, produced
  multiple artifacts, or the user mentions continuing elsewhere. The output is a dense, resume-ready brief —
  not a summary for reading, but a cold-start package for the next Claude instance.
---

# Context Handoff Skill

Produce a compressed, token-efficient session snapshot that a fresh Claude instance can ingest and immediately
resume from — no re-explanation needed.

---

## Step 1: Ask Output Format

Before generating, ask the user which format they want:

> "Quick format check — do you want this as:
> **(A)** Inline markdown block to copy-paste into a new chat
> **(B)** A downloadable `.md` file
> **(C)** Obsidian-ready note (with YAML frontmatter + tags)
> **(D)** All three"

Wait for their answer. Then generate accordingly.

---

## Step 2: Extract + Compress

Scan the full conversation and extract only signal-dense content. **Do not narrate or editorialize.**
Every line in the output should be load-bearing. Strip filler, pleasantries, dead-end threads.

### The 6 Required Sections

#### 1. `## RESUME HERE` ← Always first
One sentence: the exact task in flight, its current status, and where it stalled or what comes next.
This is the cold-start hook. Make it scannable in 3 seconds.

#### 2. `## Context`
Bullet list: the essential facts (user goals, project scope, constraints, decisions made).
Assume zero context — spell everything.

#### 3. `## Artifacts + Output So Far`
Links and brief descriptions of:
- Code written
- Files created or modified
- Documents generated
- Decisions logged

#### 4. `## Open Questions / Next Decisions`
What still needs resolving or deciding before proceeding.

#### 5. `## Key Assumptions`
Anything the conversation took for granted that a fresh Claude instance needs to know upfront.

#### 6. `## Code/Context Snippets (if needed)`
If the new Claude will need to reference specific code or detailed context, paste the most relevant snippet here with filename and line numbers.

---

## Step 3: Format Output

**If inline:** Deliver as a markdown code block ready for copy-paste into a new chat.

**If file:** Save to `/mnt/user-data/outputs/context-handoff-[project]-[date].md` and use `present_files`.

**If Obsidian:** Wrap in YAML frontmatter:
```yaml
---
date: YYYY-MM-DD
tags: [context-handoff, project-name]
project: [project name]
status: active
source: claude-session
---
```

---

## Step 4: Delivery Message

Always include:

> "Paste this into a new chat and lead with: 'I'm continuing work on [X] from a previous session. Use the context below.'"

Or if saving to file:

> "Save and load this in your next session for instant continuity."

---

## Constraints

- **No editorializing.** Just facts.
- **Chronology over narrative.** List what happened in order of relevance, not order of occurrence.
- **Code snippets only if essential** — don't paste entire files unless the new Claude absolutely needs it.
- **Assume amnesia.** Every detail that seems obvious should be in there.
- **Token efficiency.** Use shorthand where possible (links, filenames, commit SHAs) to save space.

---

## Example

**Input:** "We built a custom Obsidian plugin for EM shift tracking. Session got long. I want to hand off to a fresh Claude to finish the UI layer."

**Output:**

```markdown
## RESUME HERE
Building Obsidian plugin for EM shift tracking. UI framework set up (React + TypeScript). Next: component library + state management. Stalled on modal positioning.

## Context
- User: Andre, EM doc + developer
- Project: obsidian-shift-tracker (private GitHub repo)
- Goal: Plugin that captures patient handoffs in structured format, syncs to Obsidian vault
- Tech stack: TypeScript, React, Obsidian API, TailwindCSS
- Current branch: feature/ui-components
- Constraint: Plugin must load <500ms, no external dependencies beyond Obsidian API

## Artifacts + Output So Far
- `manifest.json` updated with required permissions (vault access, settings tab)
- `main.ts` with plugin lifecycle hooks (onload, onunload)
- `ui/Modal.tsx` — partial (modal opens but positioning broken)
- `types/shift.ts` — Shift interface defined
- GitHub: https://github.com/andrefem13-cpu/obsidian-shift-tracker/tree/feature/ui-components

## Open Questions
- Should modal be floating or dock-based? (Andre leans floating)
- Confirm timezone handling for shift dates — local vs UTC?
- Performance: how many shift records can vault handle before lag?

## Key Assumptions
- Using Obsidian API v1.4.15
- Shift data stored as YAML frontmatter in individual .md files
- User has basic TS/React knowledge but new to Obsidian plugin architecture

## Code Snippet

File: `ui/Modal.tsx` (lines 1-40)
```typescript
export class ShiftModal extends Modal {
  result: string;

  constructor(app: App) {
    super(app);
  }

  onOpen() {
    const {contentEl} = this;
    contentEl.createEl('h2', {text: 'Shift Handoff'});
    
    // TODO: Fix positioning — currently off-screen
    const form = contentEl.createEl('form');
    // form setup continues...
  }
}
```
```

---

## When to Trigger Proactively

Suggest a handoff when:
- Conversation has covered 5+ distinct topics
- Multiple artifacts created (3+ files, scripts, configs)
- Chat is 30+ messages deep and user mentions "later" or "continuing"
- User is switching contexts (e.g., "I need to hand this off to a colleague")

Offer with:

> "This conversation has a lot of context. Want me to package a handoff brief so you can pick this up seamlessly later or pass to someone else?"