---
name: spiking
description: "Use when you need to prove technical feasibility before committing to specs — building a POC or investigating an existing codebase. Also triggers on: 'spike', 'POC', 'is this feasible?', 'can we do X?', 'proof of concept', 'technical investigation'."
---

# Spiking

## Overview

Answer "can we do X?" with evidence before committing to specs.

**Core principle:** Prove it before you spec it. Evidence beats opinion.

## When to Use

- "Can library Y handle our scale?"
- "Will this API integrate with our auth?"
- "What's the performance of approach A vs B?"
- Before writing specs (phased) — technical unknowns need resolution first
- Mid-implementation (standalone) — hit a technical wall, need to validate an approach

## Step 0: Create Task List

**Before doing anything else**, create a task for each step so nothing gets lost:

**Build mode:**
1. Frame the question (one specific technical question)
2. Define success criteria (go/no-go evidence)
3. Set time budget (optional)
4. Build POC (minimal throwaway code)
5. Gather evidence (benchmarks, test output, profiles)
6. Write verdict (go, no-go, or needs more investigation)

**Investigate mode:**
1. Frame the question
2. Define success criteria
3. Map the landscape (files, modules, scope)
4. Trace code paths (dependencies, coupling, entry/exit points)
5. Measure (performance, compatibility, blast radius)
6. Assess impact
7. Write verdict

## Two Modes

| Mode | When | Evidence Source |
|------|------|----------------|
| **Build** | Greenfield / POC needed. No existing codebase constraints. | Code you wrote |
| **Investigate** | Existing codebase. "Can we do X *here*?" | Code you analyzed |

---

## Mode 1: Build (Greenfield)

### Step 1: Frame the Question

One specific technical question. Not "explore everything" — one question.

Good: "Can Postgres full-text search handle 100k documents with <200ms response?"
Bad: "Explore search options"

### Step 2: Define Success Criteria

What evidence would answer go vs no-go? Write it down before coding.

```markdown
- [ ] Response time < 200ms for 100k documents
- [ ] Supports fuzzy matching
- [ ] Works with our existing Postgres version
```

### Step 3: Optional — Declare Time Budget

"I'll spend max 2 hours on this." Not required, but prevents rabbit holes.

### Step 4: Build POC

Minimal throwaway code. Just enough to answer the question.

- No production quality needed
- No error handling unless that's what you're testing
- No tests (the POC IS the test)
- Ugly code that answers the question > clean code that doesn't

### Step 5: Gather Evidence

Run the POC. Collect hard data:
- Benchmark results
- Integration test output
- Error messages
- Performance profiles

Save raw data in `SPIKE.md` under Evidence.

### Step 6: Write Verdict

Fill in SPIKE.md verdict section: go, no-go, or needs-more-investigation. Archive POC code in `poc/` directory.

---

## Mode 2: Investigate (Existing Codebase)

### Step 1: Frame the Question

Same as build mode — one specific question.

Good: "Can we migrate from Express to Hono without breaking our 200 API tests?"
Bad: "Should we change frameworks?"

### Step 2: Define Success Criteria

Same as build mode.

### Step 3: Map the Landscape

What exists that's relevant to the question?
- Which files/modules are involved?
- What's the scope of impact?
- What external dependencies are in play?

### Step 4: Trace Code Paths

Follow the data flow through the system:
- Dependency chains — what depends on what?
- Coupling points — where are the tight integrations?
- Entry/exit points — where does data come in and leave?

### Step 5: Measure

If performance-related: profile existing code, run benchmarks.
If compatibility-related: test interfaces, check API contracts.
If migration-related: count touchpoints, estimate blast radius.

### Step 6: Assess Impact

| Dimension | Finding |
|-----------|---------|
| Files affected | {list} |
| Dependencies | {what breaks if we change this} |
| Hard constraints | {what can't move} |
| Risk level | {low / medium / high} |
| Estimated effort | {how much work is the real implementation?} |

### Step 7: Write Verdict

Same as build mode, but include the impact assessment. The verdict answers not just "can we?" but "what would it take?"

---

## Key Differences Between Modes

| | Build | Investigate |
|---|-------|-------------|
| Evidence source | Code you wrote | Code you analyzed |
| Primary artifact | POC in `poc/` | Analysis in SPIKE.md (dep maps, traces, profiles) |
| Time profile | Mostly coding | Mostly reading + measuring |
| Risk output | "It works/doesn't" | "It works/doesn't AND here's what it would take" |

## Folder Structure

**Epic-scoped:**
```
epics/{EPIC-ID}-{slug}/spikes/
├── INDEX.md                    # Spike log
└── SPIKE-001-{slug}/
    ├── SPIKE.md                # Question, criteria, evidence, verdict
    └── poc/                    # Archived POC code (build mode only)
```

**Standalone:**
```
spikes/
├── INDEX.md
└── SPIKE-001-{slug}/
    ├── SPIKE.md
    └── poc/
```

When setting up for the first time, create the `spikes/` folder structure and initialize `INDEX.md` using `references/spike-index-template.md`. Use `references/spike-template.md` for each spike.

## Key Disciplines

- **Declare what you're proving BEFORE writing code.** No aimless exploration.
- **POC code is archived, not shipped.** It's evidence, not implementation.
- **"Needs more investigation" is a valid verdict.** Document what you learned and what questions remain.
- **Never gold-plate a POC.** Ugly code that answers the question beats clean code that doesn't.
- **Each spike declares its own success criteria and optional time budget.** Flexible, not rigid.
- **One question per spike.** If you have three questions, run three spikes.

## Quick Reference

| Phase | What You Do | Output |
|-------|------------|--------|
| Frame | One specific technical question | Question documented |
| Criteria | What evidence answers go/no-go? | Success criteria checklist |
| Evidence | Build POC or investigate existing code | Raw data, benchmarks, traces |
| Verdict | Go, no-go, or needs more investigation | SPIKE.md completed |

## Red Flags

| Thought | Reality |
|---------|---------|
| "Let me just try something" | Frame the question first. Aimless exploration wastes time. |
| "I'll clean this up later" | POC code is throwaway. Don't polish it. |
| "This is taking too long" | Check your time budget. Write verdict with what you have. |
| "I need to explore everything" | One question per spike. Run multiple spikes if needed. |
| "The POC is almost production-ready" | Stop. Archive it. Write the real implementation from specs. |
| "I don't need criteria, I'll know it when I see it" | Write criteria. Vague goals produce vague results. |

## Integration

**Called by:**
- **sdd:workflow** — When user says "spike", "POC", "is this feasible?"
- **sdd:creating-epics** — Before writing specs when technical unknowns exist

**Can trigger:**
- **sdd:designing** — Spike reveals a design question ("it works, but which approach?")

**Triggered from:**
- **sdd:implementing-with-tdd** — Hit a technical wall mid-implementation
- **sdd:designing** — Design option needs technical validation before choosing

**Pairs with:**
- **sdd:designing** — Spike validates feasibility, designing picks the approach
- **sdd:writing-specs** — Spike verdicts inform spec decisions
