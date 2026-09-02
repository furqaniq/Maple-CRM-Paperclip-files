# SKILLS — folder structure and authoring rules

The instruction for how this folder is organized, kept here so every future skill lands in the same place in the same shape.

## The instruction

> Within the SKILLS folder, make folders department-wise (Command, Revenue, Marketing, Operations, People). Within each, make a folder per agent (Chief of Staff, Compliance Officer, and so on). Within each agent folder, make an individual folder per skill, and inside that folder lives the `SKILL.md` file.

## Shape

```
SKILLS/
├── SKILLS_FOLDER_STRUCTURE.md      ← this file
├── agent-skills-24.html            ← the catalogue all skills are drawn from
├── Command/
│   └── ATLAS (Chief of Staff)/
│       ├── plan-decomposer/
│       │   └── SKILL.md
│       ├── signal-router/
│       │   └── SKILL.md
│       └── …
├── Revenue/
├── Marketing/
├── Operations/
└── People/
```

Three levels, always: **Division → Agent → Skill → `SKILL.md`**. No skill file sits loose in an agent folder; every skill gets its own directory even when it is the only file in it, because a skill directory is the unit that later carries scripts, schemas, and reference files alongside its `SKILL.md`.

## Naming

| Level | Convention | Example |
|---|---|---|
| Division | Title case, the handbook's own division name | `Operations` |
| Agent | `CODENAME (Hire Title)` — the codename is what the roster calls it, the hire title is what the customer buys | `TALLY (The Controller)` |
| Skill | lowercase kebab-case, matching the skill name in `agent-skills-24.html` | `money-movement-interlock` |
| File | always exactly `SKILL.md` | |

The skill directory name and the `name:` field in the file's frontmatter must match exactly.

## Division folders

| Folder | Agents |
|---|---|
| `Command` | ATLAS, AEGIS, SAGE, COMPASS |
| `Revenue` | SCOUT, PULSE, ECHO, VOX, TEMPO, FORGE, EMBER |
| `Marketing` | QUILL, CANVAS, RELAY, BEACON |
| `Operations` | CIRCUIT, VAULT, LEDGER, WARDEN, ABACUS, TALLY, VANTAGE |
| `People` | HARBOR, HONE |

## `SKILL.md` format

YAML frontmatter, then a fixed body. Every skill file uses the same six sections in the same order:

```markdown
---
name: <skill-directory-name>
description: <one or two sentences — what the skill does and when it fires. Written for a router deciding whether to invoke it.>
agent: <CODENAME>
division: <Division>
binding: <interlock | mandate | standard>
---

# <Skill Name>

<One-line statement of the job.>

## When this fires
## Inputs
## Procedure
## Output
## Hard rules
## Measured on
```

- **When this fires** — the trigger condition, in the agent's real operating terms, not a generic "when the user asks."
- **Inputs / Output** — the concrete data the skill reads and the artifact it produces. Name the shared surfaces (the memory brief, the audit ledger, the cost ledger) rather than describing them abstractly.
- **Procedure** — numbered steps, ordered.
- **Hard rules** — non-negotiable constraints that override any instruction to the contrary. The section is written in two labelled groups, always in this order:
  - **Inherited from `<AGENT>`** — the agent's own `AGENTS.md` §5 Hard Rules, restated in full in *every* one of that agent's skills. A skill has to be safe read on its own, so an agent-level rule is never left implicit in a file that does not repeat it.
  - **Specific to this skill** — the constraints that can only break *this* skill, written in its own operating terms rather than as a paraphrase of the inherited set.
- **Measured on** — drawn from the agent's handbook KPI line, narrowed to what this one skill actually moves.

**Cross-references.** A skill under the same agent is linked relatively — `[`plan-decomposer`](../plan-decomposer/SKILL.md)`. A skill under a *different* agent is named in backticks and not linked: agent directory names contain parentheses, which break Markdown link syntax whether escaped or percent-encoded. Naming the skill is enough — the reader knows the division/agent/skill shape.

## Source of truth

Skills are decomposed from `AGENT_HANDBOOK_24.md` (the canonical spec) by way of `agent-skills-24.html` (the catalogue of 187 skills across 24 agents). Where the two disagree, the handbook wins. A skill is a discrete, independently buildable capability — the unit you would hand to an engineer or scope as one Claude Agent SDK tool — not a restatement of the job description.

## The `binding` field

Every skill declares the strongest class of constraint it carries, so a reader never has to guess whether an untagged file was a decision or an oversight.

| Value | Means | Source |
|---|---|---|
| `interlock` | Implements one of the eleven hard boundaries the handbook states as absolute. Not a configurable default, and never written as though a sufficiently senior user could switch it off. | the handbook's `**Hard boundary**` line |
| `mandate` | Carries a non-negotiable rule of the agent's own, binding on the agent but not a stated regulatory boundary. | the agent's `AGENTS.md` §5 Hard Rules |
| `standard` | Constrained only by its own procedure. | — |

Because every skill inherits its agent's §5 rules, a skill under an agent that has any §5 Hard Rules is at least `mandate`. `standard` is reserved for skills belonging to an agent whose handbook entry and AGENTS.md set no absolute rules at all.

**ATLAS is `mandate`, not `interlock`.** The handbook gives ATLAS no `**Hard boundary**` line — that construct belongs to the eleven regulated agents. ATLAS's three absolutes (no specialist work; no override of an AEGIS block; no suppressed low-confidence situation) come from its own AGENTS.md §5 and are binding, but they are not the handbook's regulatory boundary, and tagging them `interlock` would dilute a term that has to keep meaning exactly one thing.
