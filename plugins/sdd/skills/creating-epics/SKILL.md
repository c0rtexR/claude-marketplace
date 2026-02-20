---
name: creating-epics
description: "Use when starting a new project, creating an epic, checking what to work on next, or closing completed work. Also triggers on: 'new project', 'start feature', 'create epic', 'what's next', 'close epic'."
---

# Creating Epics

## Overview

Epics are the working area for all design, planning, and implementation. Create the structure first, fill it in, then work within it.

**Core principle:** The folder structure IS the coordination mechanism. Every artifact has a home.

## When to Use

- Starting a new project or major feature
- User says "let's build X" or "new epic"
- Resuming work ("what's next")
- Wrapping up completed work ("close epic")

## Step 0: Create Task List

**Before doing anything else**, create a task for each step so nothing gets lost:

**Creating a new epic:**
1. Interview the user
2. Create folder structure
3. Fill in core documents (CLAUDE.md, EPIC.md, REQUIREMENTS.md, CHECKLIST.md, stories)
4. Identify features and invoke writing-specs

**Closing an epic:**
1. Verify all phases and checklist complete
2. Create feature documentation in services/
3. Sync ADRs to service decisions
4. Run retrospective (invoke reflecting)
5. Update EPIC.md and INDEX.md

## Creating a New Epic

### Step 1: Interview the User

Ask these questions (one at a time):
- What are we building? (the problem, not the solution)
- Who is it for?
- What are the hard constraints? (tech stack, timeline, integrations)
- What does "done" look like?

### Step 2: Create the Folder Structure

```
epics/
├── INDEX.md                        # Epic catalog (status table)
└── {EPIC-ID}-{slug}/
    ├── EPIC.md                     # Overview, phases, success criteria
    ├── CHECKLIST.md                # Lifecycle checklist
    ├── REQUIREMENTS.md             # Hard requirements
    ├── decisions/
    │   ├── INDEX.md                # Decision log
    │   ├── ADR-001-{slug}.md       # Architecture Decision Records
    │   └── explorations/           # Design explorations (see sdd:designing)
    │       └── EXP-001-{slug}.md
    ├── spikes/
    │   ├── INDEX.md                # Spike log (see sdd:spiking)
    │   └── SPIKE-001-{slug}/
    │       ├── SPIKE.md
    │       └── poc/
    ├── stories/
    │   └── {STORY-ID}.md
    ├── bugs/
    │   └── {BUG-ID}/
    │       ├── INVESTIGATION.md
    │       ├── SOLUTION.md
    │       ├── CHECKLIST.md
    │       └── logs/
    └── features/
        ├── INDEX.md                # Feature catalog + dependency map
        └── {feature-name}/
            ├── architecture.md
            ├── specs/
            │   ├── api.md
            │   ├── database.md
            │   ├── tests.md
            │   ├── transformations.md
            │   └── ui/
            │       ├── INDEX.md
            │       ├── pages/
            │       ├── flows.md
            │       └── state.md
            └── phases/
                └── phase{N}.md
```

Also create at project root (if not already present):
```
project-root/
├── CLAUDE.md                       # Points agents to SDD skills
├── services/
│   ├── INDEX.md
│   └── {service}/
│       ├── PROJECT.md              # Tech stack, key paths, conventions
│       └── features/
├── bugs/                           # Standalone bugs (no epic context)
└── docs/
    ├── coding-standards.md         # Rules derived from real bugs (see template)
    ├── agent-protocol.md           # Agent verification contract (see template)
    └── learnings/                  # Self-improvement system (see sdd:reflecting)
        ├── INDEX.md                # Dashboard: patterns, unreviewed, metrics
        ├── entries/                # Individual learning entries
        └── reviews/               # Retrospective review summaries
```

**Key rule:** `epics/` is the working area. `services/` is source of truth — only update after code merges.

### Step 3: Fill In Core Documents

Use templates from `references/` alongside this skill:

| Document | Template | Purpose |
|----------|----------|---------|
| CLAUDE.md | `references/claude-md-template.md` | Project entrypoint for agents |
| EPIC.md | `references/epic-template.md` | Overview, phases, success criteria |
| REQUIREMENTS.md | `references/requirements-template.md` | Functional & non-functional requirements |
| CHECKLIST.md | `references/checklist-template.md` | Lifecycle progress tracking |
| stories/{id}.md | `references/story-template.md` | User stories with acceptance criteria |
| docs/coding-standards.md | `references/coding-standards-template.md` | Bug-derived rules (numbered, with examples) |
| docs/agent-protocol.md | `references/agent-protocol-template.md` | Agent verification contract (Definition of Done) |

### Step 4: Identify Features

Each deliverable gets a folder under `features/`. Create the feature folders with empty spec directories. Update `features/INDEX.md`:

**REQUIRED SUB-SKILL:** After identifying features, invoke `sdd:writing-specs` to write specs for each feature. Do not skip this step.

```markdown
| Feature | Service | Depends on | Architecture | Specs | Phases | Status |
|---------|---------|-----------|-------------|-------|--------|--------|
| {name} | {service} | — | [arch](...) | — | — | planning |
```

## Resuming Work ("What's Next")

1. Read `epics/INDEX.md` for in-progress epics
2. Find phases with status `in-progress` or next `planning` phase
3. Check CHECKLIST.md for uncompleted items
4. Present options to user

## Closing an Epic

1. Verify all phases complete (check each `phases/phaseN.md`)
2. Verify all CHECKLIST.md items are done or explicitly skipped
3. Create feature documentation in `services/{service}/features/` using `references/feature-readme-template.md`
4. Create/update `services/{service}/PROJECT.md` using `references/project-template.md`
5. Sync ADRs from epic `decisions/` to service decisions folder
6. Run retrospective — invoke `sdd:reflecting` in review mode
7. Verify all learning entries reviewed and actions applied
8. Update EPIC.md status → `complete`
9. Update `epics/INDEX.md`

## Checklist Skip Protocol

Every checklist item must be DONE or explicitly SKIPPED. Never leave a box unchecked:
```
- [x] ~~Item~~ — **SKIPPED**: {reason} (approved by: {name})
```

## Automatic Decision Capture

Create an ADR immediately when you see:
- A choice between multiple approaches
- A trade-off being made
- A deviation from existing patterns
- Something a future developer would ask "why?"

Use `references/adr-template.md`. Record in `decisions/INDEX.md`. Inform user: "Recorded as ADR-{NNN}."

## Writing Style for All SDD Documents

- **Tables over prose** — structured data in tables, not paragraphs
- **Frontmatter for metadata** — YAML frontmatter for status, IDs, dates
- **ASCII diagrams** — portable, diff-friendly
- **Short sentences** — direct language, no filler
- **Link, don't duplicate** — reference other files
- **Consistent naming** — lowercase-kebab-case for folders and files

## Integration

**Called by:**
- **sdd:workflow** — When user says "new project", "let's build X", "what's next", "close epic"

**Required after:**
- **sdd:writing-specs** — REQUIRED after Step 4 (features identified). Write specs for each feature before any code.

**Pairs with:**
- **sdd:investigating-bugs** — For bugs found during or after implementation
- **sdd:coordinating-agent-teams** — For complex epics requiring multi-agent teams
- **sdd:reflecting** — REQUIRED during epic closure (Step 6). Run retrospective before marking complete.
- **sdd:spiking** — For technical feasibility spikes before writing specs
- **sdd:designing** — For design exploration when approach is unclear
