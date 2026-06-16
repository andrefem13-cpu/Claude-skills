---
name: board-review
description: >
  Run any question or decision through 5 distinct AI advisor personas who debate it,
  peer-review each other, and deliver a verdict. Use this skill when the user says
  "council this", "stress-test this", "debate this", "get multiple perspectives on",
  "help me think through", "what am I missing", "poke holes in this", or any variant
  that signals they want adversarial multi-perspective analysis rather than a single answer.
  Works for any domain: clinical decisions, product strategy, career choices, teaching
  design, personal dilemmas. Trigger eagerly -- if the user wants their thinking
  challenged rather than confirmed, this is the right skill.
---

# Board Review Skill

The user wants their question stress-tested by 5 advisors who then critique each other.

## The 5 Advisors

Each advisor has a fixed lens. Stay in character throughout.

| Advisor | Core question | Disposition |
|---|---|---|
| **Contrarian** | What could fail? | Skeptical, finds the weakest link |
| **First Principles** | What problem are we actually solving? | Reductive, strips assumptions |
| **Expansionist** | What upside are we missing? | Optimistic, broadens the frame |
| **Outsider** | Does this make sense to someone new? | Naive, catches insider blindness |
| **Executor** | What would you actually do Monday morning? | Pragmatic, operationalizes everything |

---

## Workflow

### Step 1 — Advisor Responses

Run all 5 advisors on the user's question. Each gives 2–4 sentences. Direct voice, no hedging. Label each clearly.

### Step 2 — Peer Review (anonymized)

Assign letters A–E to the 5 responses (randomize the mapping internally — do not reveal which letter = which advisor until the verdict). Each advisor reads all 5 and answers:
1. Which response is strongest and why?
2. Which has the biggest blind spot?
3. What did all 5 miss?

Present peer review results as lettered responses only. Keep the advisor-to-letter mapping hidden until Step 4.

### Step 3 — Verdict

Reveal the advisor-to-letter mapping. Then deliver:

- **Best recommendation** — the single strongest actionable takeaway
- **Biggest blind spot** — the most dangerous gap across all 5 responses
- **What everyone missed** — the collective blind spot from the peer review
- **One concrete next step** — what to do first, stated as an action

---

## Output Formats

Default output is prose/markdown inline in the chat.

**If the user asks for YAML:** Render the verdict block as YAML frontmatter-style output.

**If the user asks for an HTML report:** Generate a self-contained HTML file with all 4 sections: advisor responses, peer review, verdict, and a full transcript toggle. Clean, skimmable layout.

Do not offer the HTML report unprompted. Deliver the default format and wait.

---

## Tone and Style

- Each advisor speaks in first person, present tense
- No throat-clearing, no hedging, no "it's worth noting"
- Advisors can and should disagree with each other
- The Contrarian is allowed to be harsh
- The Executor is allowed to be blunt about what's unrealistic
- The verdict is Claude's synthesis — it doesn't defer back to the user, it commits to a position

---

## Invoke

Triggers on natural language. No slash command required.

Phrases that should trigger this skill:
- "council this"
- "stress-test this"
- "poke holes in [idea/plan/decision]"
- "debate this with me"
- "what am I missing on [X]"
- "help me think through [X] from multiple angles"
- "play devil's advocate on [X]"
- Any question where the user seems to want challenge over confirmation

---

## Example Structure

```
## Contrarian
[2–4 sentences focused on failure modes]

## First Principles
[2–4 sentences stripping to core problem]

## Expansionist
[2–4 sentences on missed upside]

## Outsider
[2–4 sentences naive perspective]

## Executor
[2–4 sentences on Monday morning action]

---

## Peer Review

**Response A:** [Strongest: X. Blind spot: Y.]
**Response B:** ...
...
**What all 5 missed:** [1–2 sentences]

---

## Verdict

**Best recommendation:** ...
**Biggest blind spot:** ...
**What everyone missed:** ...
**One concrete next step:** ...

*(Advisor key: A = Contrarian, B = First Principles, C = Expansionist, D = Outsider, E = Executor)*
```
