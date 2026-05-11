# CLAUDE.md
These rules apply to every task in this repo unless explicitly overridden.
Bias: caution over speed on non-trivial work. Use judgment on trivial tasks.

## Rule 1 — Think Before Coding
State assumptions explicitly. If uncertain, ask rather than guess.
Present multiple interpretations when ambiguity exists.
Push back when a simpler approach exists.
Stop when confused. Name what's unclear.

## Rule 2 — Simplicity First
Minimum code that solves the problem. Nothing speculative.
No features beyond what was asked. No abstractions for single-use code.
Test: would a senior engineer say this is overcomplicated? If yes, simplify.

## Rule 3 — Surgical Changes
Touch only what you must. Clean up only your own mess.
Don't "improve" adjacent code, comments, or formatting.
Don't refactor what isn't broken. Match existing style.

## Rule 4 — Goal-Driven Execution
Define success criteria. Loop until verified.
Don't follow steps. Define success and iterate.
Strong success criteria let you loop independently.

## Rule 5 — Use the model only for judgment calls
Use Claude API for: classification, drafting, extraction, scoring.
Do NOT use it for: routing, retries, deterministic transforms.
If code can answer, code answers.

## Rule 6 — Token budgets are not advisory
Per-task: 4,000 tokens. Per-session: 30,000 tokens.
If approaching budget, summarize and start fresh.
Surface the breach. Do not silently overrun.

## Rule 7 — Checkpoint after every significant step
Summarize what was done, what's verified, what's left.
Don't continue from a state you can't describe back.
If you lose track, stop and restate.

## Rule 8 — Fail loud
"Completed" is wrong if anything was skipped silently.
Default to surfacing uncertainty, not hiding it.

---

## Project conventions
- Stack: React (JSX), Tailwind, Anthropic API (claude-sonnet-4-20250514)
- Skills live at: .claude/skills/[skill-name]/SKILL.md
- Palette: dark-navy / teal (dc-rx standard)
- No localStorage in artifacts — in-memory state only
- No form tags — use onClick/onChange handlers
- Mobile-first authoring (iPhone); keep file operations flat and single-step
- Max CLAUDE.md length: 200 lines
