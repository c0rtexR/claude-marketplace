# Design: sdd:spiking and sdd:designing Skills

**Date:** 2026-02-19
**Status:** Approved
**Plugin:** sdd v2.4.0

---

## Problem

SDD assumes you know what to build: epic → specs → implement. But two types of uncertainty have no home:

1. **Technical uncertainty** — "Can we do X?" Requires evidence before committing to specs.
2. **Design uncertainty** — "How should we build X?" Requires exploring options before committing to an approach.

Without these skills, teams either guess in specs or do ad-hoc exploration that doesn't feed back into the process.

---

## Skill 1: sdd:spiking — Technical Feasibility

### Purpose

Answer "can we do X?" with evidence before committing to specs.

### Trigger Phrases

"Can we do X?", "is this feasible?", "spike", "POC", "proof of concept", "technical investigation"

### Two Modes

| Mode | When | Evidence Source |
|------|------|----------------|
| **Build** | Greenfield / POC needed | Code you wrote |
| **Investigate** | Existing codebase | Code you analyzed |

### Mode 1: Build (Greenfield)

1. **Frame the question** — one specific technical question
2. **Define success criteria** — what evidence proves go/no-go?
3. **Optional: declare time budget**
4. **Build POC** — minimal throwaway code, just enough to answer the question
5. **Gather evidence** — benchmarks, test results, integration output
6. **Write verdict** — go/no-go with evidence, archive POC

### Mode 2: Investigate (Existing Codebase)

1. **Frame the question** — one specific technical question
2. **Define success criteria** — what evidence proves go/no-go?
3. **Map the landscape** — what exists, what's relevant, scope of impact
4. **Trace code paths** — dependency chains, coupling points, data flow
5. **Measure** — profile existing code, test interfaces, check compatibility
6. **Assess impact** — what files/systems change? What are the hard constraints?
7. **Write verdict** — go/no-go with evidence, dependency map, risk assessment

### Key Differences Between Modes

| | Build | Investigate |
|---|-------|-------------|
| Evidence source | Code you wrote | Code you analyzed |
| Primary artifact | POC in `poc/` | Analysis in SPIKE.md (dep maps, traces, profiles) |
| Time profile | Mostly coding | Mostly reading + measuring |
| Risk output | "It works/doesn't" | "It works/doesn't AND here's what it would take" |

### Folder Structure

```
epics/{EPIC-ID}/spikes/
├── INDEX.md
└── SPIKE-001-{slug}/
    ├── SPIKE.md           # Question, criteria, evidence, verdict
    └── poc/               # Archived POC code (build mode only)
```

Standalone spikes (no epic): `spikes/SPIKE-001-{slug}/`

### Key Disciplines

- Declare what you're proving BEFORE writing any code
- POC code is archived, not shipped — evidence, not implementation
- "Needs more investigation" is a valid verdict (with what was learned)
- Never gold-plate a POC — ugly code that answers the question beats clean code that doesn't
- Each spike declares its own success criteria and optional time budget

---

## Skill 2: sdd:designing — Design Exploration

### Purpose

Explore design options when the right approach isn't obvious. Produces visual reasoning, trade-off analysis, and a formalized decision.

### Trigger Phrases

"How should we build X?", "design options", "explore", "architecture options", "what's the right approach?"

### Steps

1. **Frame the design question** — what decision needs to be made? What are the constraints?
2. **Map the design space** — what are the moving parts? What's fixed vs flexible?
3. **Sketch 2-3 options** — each option gets:
   - ASCII diagram showing component relationships / data flow
   - Pros and cons
   - What it optimizes for (simplicity? performance? extensibility?)
4. **Trade-off matrix** — compare options across dimensions that matter for this specific context
5. **Recommend + discuss** — present recommendation with reasoning, get user input
6. **Formalize** — write the chosen approach as an ADR using existing `references/adr-template.md`

### Output Flow

```
Informal options doc (exploratory)
    │  with ASCII diagrams
    ▼
Trade-off matrix (evaluative)
    │
    ▼
ADR (formal decision record)
```

### Folder Structure

```
epics/{EPIC-ID}/decisions/
├── INDEX.md                      # Existing decision log
├── ADR-NNN-{slug}.md             # Formalized decision (existing format)
└── explorations/
    └── EXP-001-{slug}.md         # Options doc with diagrams + trade-offs
```

The `explorations/` subfolder holds the full reasoning. The ADR is the concise verdict that links back to it.

### Key Disciplines

- Always explore at least 2 options. Never commit to the first idea.
- Every option gets a diagram. If you can't draw it, you don't understand it.
- The trade-off matrix must use dimensions relevant to THIS decision, not generic ones.
- "No decision yet" is a valid outcome — if none of the options work, say why.
- Explorations are permanent artifacts — future developers should understand WHY.

---

## Workflow Integration

### Updated SDD Flow

```
creating-epics
    │
    │ Technical unknowns? → sdd:spiking (before specs)
    │ Design unclear? → sdd:designing (before/during specs)
    │
    ▼
writing-specs (informed by spike verdicts + design decisions)
    │
    ▼
(rest unchanged)
```

### Skill Chain Rules

- `spiking` → can trigger `designing` (spike reveals a design question)
- `designing` → can trigger `spiking` (design option needs technical validation)
- `designing` → feeds into `writing-specs` (chosen design becomes architecture doc)
- `implementing-with-tdd` → can trigger either (hit a wall mid-implementation)

### Route Table Additions (workflow/SKILL.md)

| User Says | Invoke | Why |
|-----------|--------|-----|
| "Can we do X?", "is this feasible?", "spike", "POC" | **sdd:spiking** | Technical feasibility |
| "How should we build X?", "design options", "explore" | **sdd:designing** | Design exploration |

---

## Files to Create/Modify

| File | Action |
|------|--------|
| `skills/spiking/SKILL.md` | **CREATE** — new skill |
| `skills/spiking/references/spike-template.md` | **CREATE** — SPIKE.md template |
| `skills/spiking/references/spike-index-template.md` | **CREATE** — INDEX.md template |
| `skills/designing/SKILL.md` | **CREATE** — new skill |
| `skills/designing/references/exploration-template.md` | **CREATE** — options doc template |
| `skills/creating-epics/SKILL.md` | **MODIFY** — add `spikes/` and `explorations/` to folder structure |
| `skills/workflow/SKILL.md` | **MODIFY** — update to 8 skills, add route table entries, add to skill chain |
| `.claude-plugin/plugin.json` | **MODIFY** — version 2.3.0 → 2.4.0, update description |

---

## Version

Plugin version: **2.3.0 → 2.4.0**
