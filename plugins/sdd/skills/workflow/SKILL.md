---
name: workflow
description: "Skills-Driven Development — a complete spec-first, TDD methodology for AI agent teams. Use this skill whenever the user wants to create an epic, plan features, implement with TDD, investigate bugs, or coordinate agent teams for software development. Also triggers on: 'new project', 'start feature', 'plan epic', 'implement phase', 'investigate bug', 'what's next', 'create PR', 'close epic'."
---

# Skills-Driven Development

A complete software development methodology for AI agent teams. This skill teaches the orchestrator (Opus) how to set up project structure, coordinate agent teammates, and deliver software through a spec-driven, test-first workflow.

## How This Works

1. User says what they want to build
2. Orchestrator reads this skill, sets up the project structure
3. Orchestrator spawns agents to do planning, spec writing, TDD, implementation
4. Agents work within the structure organically — the folder structure IS the coordination mechanism
5. Everything is tracked in markdown files that agents read/write

---

## Project Structure

When starting a new project or epic, create this structure:

```
project-root/
├── CLAUDE.md                     # Entrypoint — points agents to this skill
├── epics/
│   ├── INDEX.md                  # Epic catalog (status table)
│   └── {EPIC-ID}-{slug}/
│       ├── EPIC.md               # Overview, phases, success criteria
│       ├── CHECKLIST.md          # Lifecycle checklist (track progress)
│       ├── REQUIREMENTS.md       # Hard requirements (cannot skip)
│       ├── decisions/
│       │   ├── INDEX.md          # Decision log table
│       │   └── ADR-001-{slug}.md # Architecture Decision Records
│       ├── stories/
│       │   └── {STORY-ID}.md
│       ├── bugs/
│       │   └── {BUG-ID}/
│       │       ├── INVESTIGATION.md
│       │       ├── SOLUTION.md
│       │       └── CHECKLIST.md
│       └── features/
│           ├── INDEX.md          # Feature catalog + service mapping
│           └── {feature-name}/
│               ├── specs/
│               │   ├── api.md            # API contracts
│               │   ├── database.md       # Database schemas
│               │   ├── tests.md          # Test plan (TDD source)
│               │   ├── transformations.md # Data mappings (if needed)
│               │   └── ui/
│               │       ├── INDEX.md      # UI spec overview
│               │       └── {page}.md     # Per-page specs
│               └── phases/
│                   ├── architecture.md   # Design decisions
│                   └── phase{N}.md       # Phase execution plans
├── services/                     # Source of truth (update after merge only)
│   ├── INDEX.md
│   └── {service}/
│       ├── PROJECT.md            # Tech stack, key paths, conventions
│       └── features/
├── bugs/                         # Standalone bugs (no epic context)
│   └── {BUG-ID}/
└── docs/
    └── workflow.md
```

### Key Principle: Epics Are the Working Area

All design, specs, and planning go in the **epic folder**. The `services/` folder is the source of truth and only gets updated after code is merged. Agents work in `epics/` — never edit `services/` during planning.

---

## Non-Negotiable Principles

### 1. TDD — Tests Before Code, Always

```
BEFORE writing ANY implementation code:
1. Ensure features/{feature}/specs/tests.md exists
   → If not: write the test plan first
2. Write test code from the test plan
   → Tests MUST FAIL (RED) — code doesn't exist yet
3. ONLY THEN write implementation to make tests pass (GREEN)
4. Refactor while keeping tests green

❌ NEVER write code first, add tests after
✅ ALWAYS write tests first, then code to pass
```

The RED → GREEN → REFACTOR cycle:
- **RED**: Write a failing test that describes desired behavior
- **GREEN**: Write the minimum code to make it pass
- **REFACTOR**: Clean up while keeping all tests green
- Commit at every GREEN state

### 2. Spec-Driven Development — Document First, Code Second

Document the UI first → then design the API to serve it → then the database to store it → then the test plan to verify it. This ensures everything fits together before a single line of code is written.

Order: **UI specs → API specs → Database specs → Test plan → Implementation**

### 3. Closed-Loop Verification

Agents MUST run tests themselves and verify they pass before claiming work is done. Never say "done and working" without having executed tests and confirmed output. If tests fail, fix and re-run until green.

### 4. Checklist Skip Protocol

Checklist items are either DONE or explicitly SKIPPED with human approval. Never leave a box unchecked. To skip:
```
- [x] ~~Item~~ — **SKIPPED**: {reason} (approved by: {name})
```

### 5. Automatic Decision Capture

Proactively detect and record Architecture Decision Records when you see:
- A choice between multiple approaches
- A trade-off being made
- A deviation from existing patterns
- An irreversible or costly-to-reverse choice
- Something a future developer would ask "why?"

Create ADR immediately in `decisions/`, inform the user: "Recorded as ADR-{NNN}."

### 6. Work by Phases, Not Stories

Stories are for tracking. Work is organized into **phases** — each phase is a coherent vertical slice with 1-3 PRs, each under 2000 lines.

---

## Agent Teams Coordination

The orchestrator (Opus) coordinates Sonnet teammates. Here's how to structure the team by phase:

### Planning Phase

```
Orchestrator assigns work to:
├── Planner agent:
│   "Read REQUIREMENTS.md + user input. Create phase plans in features/{feature}/phases/."
├── Spec Writer agent:
│   "Write specs in features/{feature}/specs/ — start with UI, then API, then database."
└── Test Planner agent:
    "Read all specs. Write features/{feature}/specs/tests.md with test cases per spec."
```

**File ownership during planning:**
- Planner owns: EPIC.md, phases/*.md, stories/*.md
- Spec Writer owns: specs/api.md, specs/database.md, specs/ui/*.md, specs/transformations.md
- Test Planner owns: specs/tests.md
- All agents can read anything, write only to their owned files

### Implementation Phase

```
Orchestrator assigns work to:
├── TDD Writer agent:
│   "Read specs/tests.md. Write failing tests for phase {N} test cases. Confirm RED."
├── Implementer agent(s):
│   "Read failing tests + specs. Write minimum code to make tests pass. Confirm GREEN."
└── Reviewer agent:
    "Read diff against specs. Verify implementation matches spec. Check test coverage."
```

**Sequencing:**
1. TDD Writer writes tests → confirms RED
2. Implementer writes code → confirms GREEN
3. Implementer refactors → confirms still GREEN
4. Reviewer validates against specs
5. PR is created

### Bug Investigation Phase

```
Orchestrator assigns work to:
├── Investigator agent(s):
│   "Reproduce bug. Check logs/traces. Document findings in INVESTIGATION.md."
├── Code Tracer agent:
│   "Trace code path from entry point to bug location. Map the data flow."
└── Fixer agent:
    "Write regression test (RED). Implement fix (GREEN). Run all tests."
```

### Context Loading for Agents

When spawning an agent, tell it which files to read:

```
For planning work:
  "Read: EPIC.md, REQUIREMENTS.md, features/INDEX.md"

For spec writing:
  "Read: REQUIREMENTS.md, stories/{story}.md, features/{feature}/specs/"

For implementation:
  "Read: features/{feature}/specs/tests.md, features/{feature}/specs/api.md,
   features/{feature}/phases/phase{N}.md, services/{service}/PROJECT.md"

For bug investigation:
  "Read: bugs/{id}/INVESTIGATION.md, services/{service}/PROJECT.md"
```

---

## Workflows

### "Create Epic" / "Start New Feature"

1. Interview user: What are we building? Who is it for? What are the constraints?
2. Create epic folder structure (see Project Structure above)
3. Fill in EPIC.md from `references/epic-template.md`
4. Fill in REQUIREMENTS.md from `references/requirements-template.md`
5. Fill in CHECKLIST.md from `references/checklist-template.md`
6. Create `decisions/INDEX.md`
7. Identify features — each deliverable gets a folder under `features/`
8. Create feature folders with empty spec directories

### "Plan Epic" / "Prepare for Implementation"

1. Read EPIC.md and REQUIREMENTS.md
2. For each feature, create specs (UI first → API → Database → Tests):
   - `specs/ui/INDEX.md` + per-page specs from `references/ui-page-template.md`
   - `specs/api.md` from `references/api-spec-template.md`
   - `specs/database.md` from `references/database-spec-template.md`
   - `specs/tests.md` from `references/tests-spec-template.md`
   - `specs/transformations.md` if data mapping is needed
3. Define phases in `features/{feature}/phases/`:
   - `architecture.md` — high-level design
   - `phase1.md`, `phase2.md` etc. from `references/phase-template.md`
4. Validate UI/API contract: every UI data field maps to an API response field
5. Run pre-implementation checklist (below)
6. Update CHECKLIST.md — check off planning items

**Pre-Implementation Checklist:**
- [ ] CHECKLIST.md exists and planning items checked off
- [ ] REQUIREMENTS.md complete
- [ ] decisions/INDEX.md exists, key decisions recorded as ADRs
- [ ] features/INDEX.md lists all features
- [ ] Phases defined with milestones in EPIC.md
- [ ] Per feature: specs/tests.md exists with test cases
- [ ] Per feature: specs/api.md has all endpoints
- [ ] Per feature: specs/ui/ has page specs (if applicable)
- [ ] Per feature: specs/database.md has all tables (if applicable)
- [ ] UI/API contract validated — all UI data fields match API response fields
- [ ] Phase plans exist with task breakdowns

### "Implement Phase N"

1. Read phase plan at `features/{feature}/phases/phaseN.md`
2. Read feature specs in `features/{feature}/specs/`
3. Read REQUIREMENTS.md for constraints
4. Read `services/{service}/PROJECT.md` for tech stack and conventions
5. **TDD Checkpoint:**
   - Does `specs/tests.md` exist? If not → write it first
   - Write test code for this phase's scope
   - Run tests → They MUST fail (RED)
6. **Only after tests fail:** Implement to make tests pass (GREEN)
7. **Run tests and verify output** → Must pass. Fix until green.
8. Refactor while keeping tests green
9. Create PR (< 2000 lines, < 30 files)
10. Update tracking:
    - Check off tasks in `phases/phaseN.md`
    - Add PR to PR table in phase plan
    - Update story status in EPIC.md
    - Check off CHECKLIST.md items

### "Investigate Bug"

1. Determine context: part of an active epic (epic-scoped) or standalone?
2. Create bug folder with INVESTIGATION.md, SOLUTION.md, CHECKLIST.md
3. **Investigation first — never jump to a fix:**
   - Document symptoms: what's happening vs what's expected
   - Reproduce: exact steps to trigger
   - Map architecture: which services, what's the data flow
   - Check logs, metrics, recent changes
   - Track hypotheses: what might be wrong, how to verify/disprove
4. Document root cause in INVESTIGATION.md
5. Propose solutions in SOLUTION.md (options with pros/cons)
6. **Regression test (mandatory):**
   - Write test reproducing the bug (RED)
   - Implement fix (GREEN)
   - Run ALL tests — regression passes, existing tests still pass
7. Create PR, update tracking
8. After deploy: verify fix, document lessons learned

**Common Bug Types:**
- **Data flow**: wrong/missing data — trace source → processing → storage → retrieval
- **Timing/Race conditions**: intermittent failures — check async, concurrency, retries
- **Integration**: A works, B works, A+B fails — check contracts, schemas, versions
- **Configuration**: works in one env, fails in another — compare configs, env vars, flags

### "Continue" / "What's Next"

1. Read `epics/INDEX.md` for in-progress epics
2. Find phases with status `in-progress` or next `planning` phase
3. Present options to user

### "Complete Phase" / "PRs Merged"

1. Update `phases/phaseN.md`: check off tasks, add PRs, update status
2. Update EPIC.md: story status, phase status
3. Update story files: frontmatter status → done
4. Check off CHECKLIST.md items

### "Close Epic"

1. Verify all phases complete
2. Verify all checklist items checked (done or skipped)
3. Create feature documentation in `services/{service}/features/`
4. Sync ADRs from epic to service decisions folder
5. Update EPIC.md status → complete
6. Update `epics/INDEX.md`

### "Create PR"

PR rules:
- < 2000 lines changed
- < 30 files changed
- One deliverable per PR, one phase per PR (1-3 PRs per phase)
- Title: `{type}: {description}` (feat, fix, refactor, test, docs, chore)
- Body: summary, changes, testing evidence, spec references
- If too big: split by layer (DB → API → UI) or by feature slice

---

## Templates

All templates are in the `references/` folder alongside this skill. When creating files, copy the appropriate template and fill it in:

| File to Create | Template |
|---|---|
| EPIC.md | `references/epic-template.md` |
| CHECKLIST.md | `references/checklist-template.md` |
| REQUIREMENTS.md | `references/requirements-template.md` |
| stories/{id}.md | `references/story-template.md` |
| phases/phaseN.md | `references/phase-template.md` |
| decisions/ADR-NNN.md | `references/adr-template.md` |
| specs/api.md | `references/api-spec-template.md` |
| specs/database.md | `references/database-spec-template.md` |
| specs/tests.md | `references/tests-spec-template.md` |
| specs/ui/{page}.md | `references/ui-page-template.md` |
| specs/transformations.md | `references/transformations-template.md` |
| bugs/{id}/INVESTIGATION.md | `references/bug-investigation-template.md` |
| bugs/{id}/SOLUTION.md | `references/bug-solution-template.md` |
| bugs/{id}/CHECKLIST.md | `references/bug-checklist-template.md` |
| features/{name}/README.md | `references/feature-readme-template.md` |
| services/{name}/PROJECT.md | `references/project-template.md` |
| PR description | `references/pr-template.md` |

---

## Writing Style for Agent Context

Keep all markdown files token-efficient. Agents will read these files into their context window:

- **Tables over prose** — structured data in tables, not paragraphs
- **Frontmatter for metadata** — YAML frontmatter for status, IDs, dates
- **ASCII diagrams** — portable, diff-friendly, no external tools needed
- **Short sentences** — direct language, no filler words
- **Link, don't duplicate** — reference other files instead of copying content
- **Consistent naming** — lowercase-kebab-case for folders and files
