# Spiking & Designing Skills Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add two new SDD skills — `sdd:spiking` (technical feasibility) and `sdd:designing` (design exploration) — and integrate them into the existing workflow.

**Architecture:** Two new skill directories under `plugins/sdd/skills/` following the same pattern as existing skills (SKILL.md + references/). Modifications to workflow, creating-epics, and plugin.json to integrate.

**Tech Stack:** Markdown skill files, YAML frontmatter, ASCII diagrams.

**Working directory:** `/Users/patrykolejniczakorlowski/Development/claude-marketplace/plugins/sdd/`

**Design doc:** `docs/plans/2026-02-19-spiking-and-designing-skills-design.md`

---

### Task 1: Create spiking skill directory and templates

**Files:**
- Create: `skills/spiking/references/spike-template.md`
- Create: `skills/spiking/references/spike-index-template.md`

**Step 1: Create directory structure**

Run: `mkdir -p plugins/sdd/skills/spiking/references`

**Step 2: Create spike-template.md**

Write `skills/spiking/references/spike-template.md`:

```markdown
---
id: SPIKE-NNN
date: YYYY-MM-DD
question: "{The specific technical question}"
mode: # build | investigate
status: # in-progress | go | no-go | needs-more-investigation
time_budget: # optional, e.g. "2 hours"
epic: # epic ID if applicable, or "standalone"
---

# SPIKE-NNN: {Short Title}

## Question

{One specific technical question. Not "explore everything" — one question.}

## Success Criteria

{What evidence would answer go vs no-go? Be specific.}
- [ ] {Criterion 1 — e.g. "Response time < 200ms under 1000 concurrent users"}
- [ ] {Criterion 2}

## Evidence

### Build Mode
{POC implementation notes, code in `poc/` directory}

### Investigate Mode
{Landscape map, code path traces, measurements, impact assessment}

### Raw Data
{Benchmark results, log snippets, test output, profiling data}

## Verdict

**Status:** {go | no-go | needs-more-investigation}

**Summary:** {One paragraph — what did we learn?}

**If go:** {What approach to take, key constraints discovered}

**If no-go:** {Why it doesn't work, what alternatives exist}

**If needs-more-investigation:** {What questions remain, what to try next}

## Impact Assessment (Investigate Mode)

| Dimension | Finding |
|-----------|---------|
| Files affected | {list} |
| Dependencies | {list} |
| Hard constraints | {what can't move} |
| Risk level | {low / medium / high} |
| Estimated effort | {if go, how much work is the real implementation?} |
```

**Step 3: Create spike-index-template.md**

Write `skills/spiking/references/spike-index-template.md`:

```markdown
# Spike Log

| ID | Question | Mode | Status | Verdict | Date |
|----|----------|------|--------|---------|------|
```

**Step 4: Commit**

```bash
git add plugins/sdd/skills/spiking/references/
git commit -m "feat(sdd): add spike templates"
```

---

### Task 2: Create spiking SKILL.md

**Files:**
- Create: `skills/spiking/SKILL.md`

**Step 1: Write SKILL.md**

Write `skills/spiking/SKILL.md` with:

- Frontmatter: name `spiking`, description with trigger phrases: "spike", "POC", "is this feasible?", "can we do X?", "proof of concept", "technical investigation"
- Overview: Answer "can we do X?" with evidence before committing to specs
- Core principle: "Prove it before you spec it. Evidence beats opinion."
- When to Use section
- Two Modes table (build vs investigate) with when/evidence source
- Mode 1: Build — steps 1-6 (frame question, define criteria, optional time budget, build POC, gather evidence, write verdict)
- Mode 2: Investigate — steps 1-7 (frame question, define criteria, map landscape, trace code paths, measure, assess impact, write verdict)
- Key differences table (evidence source, primary artifact, time profile, risk output)
- Folder structure (epics/{EPIC-ID}/spikes/ with INDEX.md and SPIKE-NNN-{slug}/ containing SPIKE.md and poc/)
- Standalone spikes: `spikes/SPIKE-NNN-{slug}/`
- First-time setup: create folder structure and initialize INDEX.md from template
- Key Disciplines section:
  - Declare what you're proving BEFORE writing code
  - POC code is archived, not shipped
  - "Needs more investigation" is a valid verdict
  - Never gold-plate a POC
  - Each spike declares own success criteria and optional time budget
- Quick Reference table (phase → what you do → output)
- Red Flags table:
  - "Let me just try something" → Frame the question first
  - "I'll clean this up later" → POC code is throwaway, don't polish it
  - "This is taking too long" → Check your time budget, write verdict with what you have
  - "I need to explore everything" → One question per spike
  - "The POC is almost production-ready" → Stop. Archive it. Write the real implementation from specs.
- Integration section:
  - Called by: sdd:workflow, sdd:creating-epics (before specs when technical unknowns exist)
  - Can trigger: sdd:designing (spike reveals a design question)
  - Triggered from: sdd:implementing-with-tdd (hit a technical wall mid-implementation)
  - Pairs with: sdd:designing (spike validates feasibility, designing picks the approach), sdd:writing-specs (spike verdicts inform specs)

**Step 2: Commit**

```bash
git add plugins/sdd/skills/spiking/SKILL.md
git commit -m "feat(sdd): add spiking skill"
```

---

### Task 3: Create designing skill directory and template

**Files:**
- Create: `skills/designing/references/exploration-template.md`

**Step 1: Create directory structure**

Run: `mkdir -p plugins/sdd/skills/designing/references`

**Step 2: Create exploration-template.md**

Write `skills/designing/references/exploration-template.md`:

```markdown
---
id: EXP-NNN
date: YYYY-MM-DD
question: "{The design question}"
status: # in-progress | decided | no-decision
decision: # ADR-NNN reference if decided
epic: # epic ID if applicable, or "standalone"
---

# EXP-NNN: {Short Title}

## Design Question

{What decision needs to be made? What are the constraints?}

## Design Space

### Moving Parts
{What components/systems are involved?}

### Fixed Constraints
{What can't change? Tech stack, existing contracts, performance requirements, etc.}

## Options

### Option A: {Name}

```
{ASCII diagram — component relationships, data flow}
```

**Optimizes for:** {what this approach prioritizes}

| Pros | Cons |
|------|------|
| {pro} | {con} |

### Option B: {Name}

```
{ASCII diagram}
```

**Optimizes for:** {what this approach prioritizes}

| Pros | Cons |
|------|------|
| {pro} | {con} |

### Option C: {Name} (if applicable)

```
{ASCII diagram}
```

**Optimizes for:** {what this approach prioritizes}

| Pros | Cons |
|------|------|
| {pro} | {con} |

## Trade-Off Matrix

| Dimension | Option A | Option B | Option C |
|-----------|----------|----------|----------|
| {relevant dimension 1} | {rating} | {rating} | {rating} |
| {relevant dimension 2} | {rating} | {rating} | {rating} |
| {relevant dimension 3} | {rating} | {rating} | {rating} |

## Recommendation

**Chosen:** {Option X}

**Why:** {Reasoning — what trade-offs are we accepting and why}

**Decision:** Formalized as {ADR-NNN} in `decisions/ADR-NNN-{slug}.md`
```

**Step 3: Commit**

```bash
git add plugins/sdd/skills/designing/references/
git commit -m "feat(sdd): add exploration template"
```

---

### Task 4: Create designing SKILL.md

**Files:**
- Create: `skills/designing/SKILL.md`

**Step 1: Write SKILL.md**

Write `skills/designing/SKILL.md` with:

- Frontmatter: name `designing`, description with trigger phrases: "design options", "explore", "how should we build X?", "architecture options", "what's the right approach?"
- Overview: Explore design options when the right approach isn't obvious
- Core principle: "Never commit to the first idea. Explore at least two options with diagrams and trade-offs."
- When to Use section
- Steps 1-6:
  1. Frame the design question — what decision, what constraints
  2. Map the design space — moving parts, fixed vs flexible
  3. Sketch 2-3 options — each gets ASCII diagram, pros/cons, what it optimizes for
  4. Trade-off matrix — compare across dimensions relevant to THIS decision
  5. Recommend + discuss — present recommendation, get user input
  6. Formalize — write chosen approach as ADR using `writing-specs/references/adr-template.md`
- Output Flow diagram (informal options doc → trade-off matrix → ADR)
- Folder structure:
  ```
  epics/{EPIC-ID}/decisions/
  ├── INDEX.md
  ├── ADR-NNN-{slug}.md
  └── explorations/
      └── EXP-001-{slug}.md
  ```
  Standalone: `decisions/explorations/EXP-001-{slug}.md`
- First-time setup: create `explorations/` subfolder under existing `decisions/`
- Key Disciplines:
  - Always explore at least 2 options
  - Every option gets a diagram — if you can't draw it, you don't understand it
  - Trade-off matrix uses dimensions relevant to THIS decision, not generic ones
  - "No decision yet" is valid — document why
  - Explorations are permanent artifacts
- Quick Reference table
- Red Flags table:
  - "Obviously we should use X" → That's your first idea. What's the second?
  - "This diagram is too complex" → Simplify the design, not the diagram
  - "The trade-offs are all the same" → You're not using the right dimensions
  - "Let's just pick one and iterate" → Explore first, iterate after
  - "We don't need an ADR for this" → If you explored options, the decision is worth recording
- Integration section:
  - Called by: sdd:workflow, sdd:writing-specs (when design approach is unclear during spec writing)
  - Can trigger: sdd:spiking (design option needs technical validation)
  - Triggered from: sdd:spiking (spike reveals a design question)
  - Feeds into: sdd:writing-specs (chosen design becomes architecture doc)
  - Pairs with: sdd:spiking (spike validates feasibility, designing picks the approach)

**Step 2: Commit**

```bash
git add plugins/sdd/skills/designing/SKILL.md
git commit -m "feat(sdd): add designing skill"
```

---

### Task 5: Modify creating-epics to include spikes and explorations

**Files:**
- Modify: `skills/creating-epics/SKILL.md`

**Step 1: Add spikes/ to epic folder structure**

In the epic folder structure (Step 2), add `spikes/` directory after `bugs/`:

```
    ├── spikes/
    │   ├── INDEX.md                # Spike log (see sdd:spiking)
    │   └── SPIKE-001-{slug}/
    │       ├── SPIKE.md
    │       └── poc/
```

**Step 2: Add explorations/ to decisions/ folder**

In the same folder structure, add `explorations/` under `decisions/`:

```
    ├── decisions/
    │   ├── INDEX.md                # Decision log
    │   ├── ADR-001-{slug}.md       # Architecture Decision Records
    │   └── explorations/           # Design explorations (see sdd:designing)
    │       └── EXP-001-{slug}.md
```

**Step 3: Add spiking and designing to Integration section**

Add to "Pairs with:":
```
- **sdd:spiking** — For technical feasibility spikes before writing specs
- **sdd:designing** — For design exploration when approach is unclear
```

**Step 4: Commit**

```bash
git add plugins/sdd/skills/creating-epics/SKILL.md
git commit -m "feat(sdd): add spikes and explorations to epic structure"
```

---

### Task 6: Update workflow skill for 8 skills

**Files:**
- Modify: `skills/workflow/SKILL.md`

**Step 1: Update skill count**

Change "6 specialized skills" to "8 specialized skills" in the EXTREMELY-IMPORTANT block.

**Step 2: Add to workflow diagram**

Add spiking and designing between creating-epics and writing-specs in the ASCII diagram:

```
         │ Technical unknowns?
         ▼
┌─────────────────┐
│ sdd:spiking      │ ◄── Prove feasibility before specs
│                  │     Build POC or investigate existing code
└────────┬────────┘
         │ Design approach unclear?
         ▼
┌─────────────────┐
│ sdd:designing    │ ◄── Explore options, trade-offs, ASCII diagrams
│                  │     → ADR formalizing the decision
└────────┬────────┘
```

**Step 3: Add route table entries**

Add these rows to the route table (after "Close epic" row, before "Retrospective" row):

```
| "Can we do X?", "is this feasible?", "spike", "POC" | **sdd:spiking** | Technical feasibility |
| "How should we build X?", "design options", "explore" | **sdd:designing** | Design exploration |
```

**Step 4: Add to invocation list**

Add to the Skill tool invocation block:
```
Skill: sdd:spiking
Skill: sdd:designing
```

**Step 5: Add to skill chain**

Add these entries:
```
- `spiking` → spike reveals design question → **can invoke** `sdd:designing`
- `designing` → option needs technical validation → **can invoke** `sdd:spiking`
- `designing` → decision made → feeds into `sdd:writing-specs`
```

**Step 6: Update description frontmatter**

Add spiking and designing trigger phrases to the description field.

**Step 7: Commit**

```bash
git add plugins/sdd/skills/workflow/SKILL.md
git commit -m "feat(sdd): integrate spiking and designing into workflow"
```

---

### Task 7: Bump plugin version to 2.4.0

**Files:**
- Modify: `.claude-plugin/plugin.json`

**Step 1: Update version and description**

Change version from `"2.3.0"` to `"2.4.0"`.

Update description to: `"Skills-Driven Development — a spec-first, TDD methodology with 8 practical skills: creating-epics, writing-specs, implementing-with-tdd, investigating-bugs, coordinating-agent-teams, reflecting, spiking, and designing. Pick the skill that matches what you're doing."`

**Step 2: Commit**

```bash
git add plugins/sdd/.claude-plugin/plugin.json
git commit -m "feat(sdd): bump version to 2.4.0"
```

---

### Task 8: Verify all files and cross-references

**Step 1: Verify new files exist**

Run: `ls -la plugins/sdd/skills/spiking/ plugins/sdd/skills/spiking/references/ plugins/sdd/skills/designing/ plugins/sdd/skills/designing/references/`

Expected: SKILL.md in each skill dir, templates in each references dir.

**Step 2: Verify template references in skills**

Read `skills/spiking/SKILL.md` — confirm it references `references/spike-template.md` and `references/spike-index-template.md`.

Read `skills/designing/SKILL.md` — confirm it references `references/exploration-template.md` and `writing-specs/references/adr-template.md`.

**Step 3: Verify workflow integration**

Read `skills/workflow/SKILL.md` — confirm:
- 8 skills count
- spiking and designing in diagram, route table, invocation list, skill chain
- Trigger phrases present

**Step 4: Verify creating-epics integration**

Read `skills/creating-epics/SKILL.md` — confirm:
- `spikes/` in epic folder structure
- `explorations/` under `decisions/`
- spiking and designing in Integration section

**Step 5: Verify plugin.json**

Read `.claude-plugin/plugin.json` — confirm version `2.4.0` and 8 skills in description.

**Step 6: Final commit (if any fixes needed)**

If verification reveals issues, fix and commit.
