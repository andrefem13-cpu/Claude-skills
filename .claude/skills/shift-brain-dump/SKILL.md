---
description: Convert a raw, unstructured verbal or typed brain dump from an EM physician into a clean, structured shift handoff note. Use this skill whenever the user says "brain dump", "shift dump", "end of shift", "sign out", "help me sign out", "handoff note", or pastes/dictates a stream of consciousness list of patients or events. Also trigger when the user lists patients in any unstructured format mid or end of shift. This is a high-frequency daily-use skill — trigger eagerly. Invoke with /dump or /signout.
---

# Shift Brain Dump

Convert an EM physician's raw end-of-shift brain dump into a structured, actionable sign-out
note — fast, accurate, ready to read aloud or paste into Epic.

## When to Use

- User says "brain dump", "sign out", "end of shift", /dump, /signout
- User pastes a list of patients in any unstructured format
- User dictates or types a stream of consciousness mid-shift update
- Proactive: if user lists 3+ patients with dispositions, offer: "Want me to format that as a sign-out?"

## Inputs Needed

| Input | Required? | How to provide |
|-------|-----------|----------------|
| Raw patient list or dump | Yes | Paste or dictate freely — no format needed |
| Output format preference | Optional | Default: structured table + narrative per patient |
| Pending items flag | Optional | Any labs, imaging, consults still outstanding |

## Steps

### 1. Parse the dump

Extract for each patient — do not ask clarifying questions first, work with what's given:

- Room / bay (if mentioned)
- Chief complaint or one-liner
- Workup status (labs sent, imaging ordered/resulted, consult placed)
- Current plan or disposition (admit, obs, discharge, boarding, still working)
- Pending items (outstanding results, callbacks, decisions)

If a patient is ambiguous, mark as [?] and flag at the bottom.

### 2. Ask format preference (one question, at the end of parse)

"Want this as a sign-out table, one-liner list, or both?"

Default to both if the user doesn't answer.

### 3. Generate sign-out table

```
| Room | Patient (MRN last 4) | CC / One-liner | Status | Pending |
|------|----------------------|----------------|--------|---------|
| 12   | Doe, J (4521)        | CP r/o ACS, trop x2 neg, EKG nl | Dispo pending cards recs | Cards callback |
| 07   | Smith, M (8833)      | Abd pain, CT with appendicitis | Surg consult in | OR eval tonight |
```

### 4. Generate one-liners (narrative)

Format each patient as a single tight sentence for verbal handoff:

> "Room 12 is a 58M with chest pain, two negative troponins and a normal EKG — waiting on cards."

### 5. Generate loose ends checklist

Pull all pending items across all patients into a single list:

```markdown
## Loose Ends
- [ ] Cards callback re: Room 12
- [ ] CT read finalized for Room 7 (prelim appendicitis)
- [ ] Pharmacy status on Room 3 vanc dose
```

## Output Format

Always deliver three blocks in order:

1. **Sign-Out Table** — paste-ready, Epic-compatible
2. **One-Liners** — verbal handoff, one per patient
3. **Loose Ends Checklist** — markdown checkboxes

## Voice + Constraints

- Never invent clinical details. If something isn't in the dump, mark as unknown — don't fill gaps.
- No jargon translation. Keep EM shorthand (trop, EKG, r/o, ACS) as-is — the receiving physician is also EM.
- Flag ambiguous patients at the bottom: "Couldn't parse: [paste original text] — clarify?"
- Speed over elegance. This is a working document, not a note. Terse is better.
- No HIPAA violations. Never include full names or full MRNs in output — last 4 only, or initials.

## Example

**Input (raw dump):**

> "ok so I've got 12, chest pain guy, two trops neg, waiting on cards. Seven is the belly pain, CT showed appy, surg is coming. Three is the sepsis lady, cultures in, started on vanco, needs renal dose check. I think 4 is still waiting for psych. That's it."

**Output:**

### Sign-Out Table

| Room | One-liner | Status | Pending |
|------|-----------|--------|---------|
| 12 | CP, r/o ACS, trop x2 neg | Awaiting cards | Cards callback |
| 07 | Abd pain, CT → appendicitis | Surg consult in | OR eval |
| 03 | Sepsis, cultures in, vanco started | Admitted, boarding | Renal dose check |
| 04 | Psych eval needed | Awaiting psych | Psych callback |

### One-Liners

- Room 12: 58M with chest pain, two negative troponins — waiting on cards consult.
- Room 7: Belly pain with CT-confirmed appendicitis — surgery is evaluating.
- Room 3: Sepsis, cultures pending, vancomycin started — needs renal dosing verified.
- Room 4: Holding for psychiatry evaluation.

### Loose Ends

- [ ] Cards callback re: Room 12
- [ ] Surgery plan re: Room 7
- [ ] Renal dose approval re: Room 3 vanco
- [ ] Psych ETA re: Room 4