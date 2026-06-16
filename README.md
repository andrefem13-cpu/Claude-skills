# Claude Skills

A collection of Claude Code skills (slash commands) for Andre Freire (@BloodSweatxED) — EM physician, educator, and clinician-developer.

## What's Here

Skills live at `.claude/skills/[skill-name]/SKILL.md`. Each skill is a markdown file that Claude Code reads to define behavior for a slash command.

### Custom Skills (Andre-built)

| Skill | Command | Description |
|-------|---------|-------------|
| `shift-brain-dump` | `/dump` or `/signout` | Converts raw EM shift notes into a structured sign-out table, one-liners, and loose-ends checklist |
| `em-ai-idea-lab` | `/lab` | Screens AI/tech ideas for EM through a fucks-or-sucks pipeline with Obsidian note output |
| `context-handoff` | `/handoff` | Packages the current session into a compressed snapshot for handing off to a fresh Claude instance |
| `grill-me` | `/grill-me` | Relentlessly interviews you about a plan or design using AskUserQuestion until every decision branch is resolved |
| `reminders` | `/reminders` | Bridges Apple Reminders → Claude: reads the Claude Inbox list and dispatches background agents for new tasks |
| `skill-builder` | `/skill-builder` | Interactive wizard for creating new Claude skills and writing valid SKILL.md files |

### Printing Press (Third-Party — mvanhorn)

CLI generator toolkit from [github.com/mvanhorn/cli-printing-press](https://github.com/mvanhorn/cli-printing-press). Generates production-ready Go CLIs for any API.

| Skill | Command | Description |
|-------|---------|-------------|
| `printing-press` | `/printing-press` | Full CLI generation pipeline: research → absorb → generate → verify → ship |
| `printing-press-polish` | `/printing-press-polish` | Second-pass improvement on an existing generated CLI |
| `printing-press-score` | `/printing-press-score` | Score a CLI against the Steinberger quality bar |
| `printing-press-publish` | `/printing-press-publish` | Publish a CLI to the printing-press library |
| `printing-press-retro` | `/printing-press-retro` | Post-run retrospective to improve the press itself |
| `printing-press-reprint` | `/printing-press-reprint` | Regenerate an existing CLI under the current press version |
| `printing-press-import` | `/printing-press-import` | Import a CLI from the public library into your local setup |
| `printing-press-output-review` | (internal) | Plausibility review subagent invoked by printing-press and polish |
| `printing-press-catalog` | `/printing-press-catalog` | Browse and install pre-built CLIs (deprecated) |

## Installation

To use these skills in Claude Code, clone this repo and symlink or copy the `.claude/skills/` directory into your home `.claude/`:

```bash
# Option A: copy skills into your existing .claude/skills
cp -r .claude/skills/* ~/.claude/skills/

# Option B: run from the repo root (Claude Code picks up .claude/skills automatically)
cd ~/path/to/Claude-skills
claude
```

Restart Claude Code or open a new session after adding skills.

## Adding a Skill

1. Create `.claude/skills/[skill-name]/SKILL.md`
2. Add YAML frontmatter: `name`, `description` (include trigger phrases in the description — the runtime uses it for auto-detection)
3. Write the instructions in markdown — steps, output format, constraints
4. Restart Claude Code

Or run `/skill-builder` and let Claude walk you through it.
