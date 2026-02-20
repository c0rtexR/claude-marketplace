---
name: designing
description: "Use when the right design approach isn't obvious — exploring architecture options, comparing patterns, and making trade-off decisions with diagrams. Also triggers on: 'design options', 'explore', 'how should we build X?', 'architecture options', 'what's the right approach?'."
---

# Designing

## Overview

Explore design options when the right approach isn't obvious. Produces visual reasoning, trade-off analysis, and a formalized decision.

**Core principle:** Never commit to the first idea. Explore at least two options with diagrams and trade-offs.

## When to Use

- "How should these components interact?"
- "What's the right API shape for this?"
- "Should we use pattern A or B?"
- During spec writing (phased) — the design approach is unclear
- Standalone — facing an architectural fork at any point

## Steps

### Step 1: Frame the Design Question

What decision needs to be made? What are the constraints?

Good: "Should the WebSocket handler live in the API layer or as a separate service?"
Bad: "Design the architecture"

Document the question and constraints in an exploration doc.

### Step 2: Map the Design Space

Identify what's in play:

- **Moving parts** — what components/systems are involved?
- **Fixed constraints** — what can't change? (tech stack, existing contracts, performance requirements)
- **Interfaces** — what needs to connect to what?

### Step 3: Sketch 2-3 Options

**Minimum 2 options. Always.** Even if you think you know the answer.

Each option gets:

1. **ASCII diagram** showing component relationships and data flow
2. **Pros and cons** table
3. **What it optimizes for** — simplicity? performance? extensibility? maintainability?

```
Option A: Embedded WebSocket

┌──────────────────────┐
│ API Server           │
│  ├── REST routes     │
│  └── WS handler ◄───┤── Same process, shared state
└──────────────────────┘

Option B: Separate WS Service

┌──────────────┐    ┌──────────────┐
│ API Server   │◄──►│ WS Service   │
│ (REST only)  │    │ (WS only)    │
└──────────────┘    └──────────────┘
       ▲                   ▲
       └───── Redis ───────┘
```

### Step 4: Trade-Off Matrix

Compare options across dimensions **relevant to THIS decision**. Don't use generic dimensions — pick what actually matters.

| Dimension | Option A | Option B |
|-----------|----------|----------|
| Deployment simplicity | Single process | Two services to manage |
| State sharing | Direct memory access | Redis overhead |
| Scaling | Coupled scaling | Independent scaling |
| Failure isolation | WS crash kills API | Isolated failures |

### Step 5: Recommend + Discuss

Present your recommendation with reasoning:
- Which option you'd choose
- What trade-offs you're accepting and why
- What risks remain

Get user input. Be prepared to revise.

### Step 6: Formalize as ADR

Write the chosen approach as an Architecture Decision Record:
1. Use the ADR template from `writing-specs/references/adr-template.md`
2. Link back to the exploration doc for full reasoning
3. Record in `decisions/INDEX.md`

---

## Output Flow

```
Informal options doc (exploratory)
    │  with ASCII diagrams
    ▼
Trade-off matrix (evaluative)
    │
    ▼
ADR (formal decision record)
```

The exploration doc captures the full reasoning. The ADR is the concise verdict.

## Folder Structure

**Epic-scoped:**
```
epics/{EPIC-ID}-{slug}/decisions/
├── INDEX.md                      # Existing decision log
├── ADR-NNN-{slug}.md             # Formalized decision
└── explorations/
    └── EXP-001-{slug}.md         # Options doc with diagrams + trade-offs
```

**Standalone:**
```
decisions/
├── INDEX.md
├── ADR-NNN-{slug}.md
└── explorations/
    └── EXP-001-{slug}.md
```

When setting up for the first time, create the `explorations/` subfolder under existing `decisions/`. Use `references/exploration-template.md` for each exploration.

## Key Disciplines

- **Always explore at least 2 options.** Never commit to the first idea.
- **Every option gets a diagram.** If you can't draw it, you don't understand it.
- **Trade-off matrix uses dimensions relevant to THIS decision.** Not generic ones like "scalability" unless scalability actually matters here.
- **"No decision yet" is a valid outcome.** If none of the options work, document why and what's missing.
- **Explorations are permanent artifacts.** Future developers should understand WHY you chose this approach, not just WHAT you chose.

## Quick Reference

| Phase | What You Do | Output |
|-------|------------|--------|
| Frame | One specific design question + constraints | Question documented |
| Map | Moving parts, fixed constraints, interfaces | Design space mapped |
| Sketch | 2-3 options with ASCII diagrams + pros/cons | Options doc |
| Evaluate | Trade-off matrix across relevant dimensions | Comparison table |
| Recommend | Present choice with reasoning, discuss with user | Agreed approach |
| Formalize | Write ADR, link to exploration doc | Decision recorded |

## Red Flags

| Thought | Reality |
|---------|---------|
| "Obviously we should use X" | That's your first idea. What's the second? |
| "This diagram is too complex" | Simplify the design, not the diagram. Complexity in the diagram = complexity in the system. |
| "The trade-offs are all the same" | You're not using the right dimensions. Find what actually differentiates the options. |
| "Let's just pick one and iterate" | Explore first, iterate after. Changing architecture is expensive. |
| "We don't need an ADR for this" | If you explored options, the decision is worth recording. Future you will thank you. |
| "I already know the best approach" | You might. But documenting alternatives shows you considered them. |

## Integration

**Called by:**
- **sdd:workflow** — When user says "design options", "explore", "how should we build X?"
- **sdd:writing-specs** — When design approach is unclear during spec writing

**Can trigger:**
- **sdd:spiking** — Design option needs technical validation before you can evaluate it
- **sdd:ui-designing** — Architecture decision made, feature involves UI, visualization approach unclear

**UI gate:**
- After architecture decision, check context: does this feature involve a UI?
- If YES → suggest `sdd:ui-designing` for visualization exploration
- If NO → suggest `sdd:writing-specs` directly
- If UNCLEAR → ask the user: "Does this feature involve a user interface?"

**Triggered from:**
- **sdd:spiking** — Spike reveals a design question ("it works, but which approach?")

**Feeds into:**
- **sdd:writing-specs** — Chosen design becomes the architecture doc for the feature

**Pairs with:**
- **sdd:spiking** — Spike validates feasibility, designing picks the approach
- **sdd:creating-epics** — Explorations live in the epic's decisions folder
