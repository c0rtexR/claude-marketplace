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
│       │       ├── CHECKLIST.md
│       │       └── logs/         # Raw evidence: log snippets, stack traces
│       └── features/
│           ├── INDEX.md          # Feature catalog + dependency map
│           └── {feature-name}/
│               ├── architecture.md       # Feature architecture & design decisions
│               ├── specs/
│               │   ├── api.md            # API contracts
│               │   ├── database.md       # Database schemas
│               │   ├── tests.md          # Test plan (TDD source)
│               │   ├── transformations.md # Data mappings (if needed)
│               │   └── ui/
│               │       ├── INDEX.md      # UI spec overview
│               │       ├── pages/
│               │       │   └── {page}.md # Per-page specs (ASCII + bindings)
│               │       ├── flows.md      # Navigation flows between pages
│               │       └── state.md      # Global state structure (if applicable)
│               └── phases/
│                   └── phase{N}.md       # Phase execution plans
├── services/                     # Source of truth (update after merge only)
│   ├── INDEX.md
│   └── {service}/
│       ├── PROJECT.md            # Tech stack, key paths, conventions
│       └── features/
├── bugs/                         # Standalone bugs (no epic context)
│   └── {BUG-ID}/
│       ├── INVESTIGATION.md
│       ├── SOLUTION.md
│       ├── CHECKLIST.md
│       └── logs/
└── docs/
    └── workflow.md
```

**features/INDEX.md convention:**

```markdown
| Feature | Service | Depends on | Architecture | Specs | Phases | Status |
|---------|---------|-----------|-------------|-------|--------|--------|
| {name} | {service} | — | [arch](...) | api, db, tests, ui | 1 of 2 | planning |
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

The orchestrator creates a team and spawns teammates to parallelize work. Any agent can read and write any file — there is no file ownership. Coordination comes from **task dependencies** and **document lifecycle rules** (see below).

### Document Lifecycle Rules

Instead of restricting who can edit what, restrict **when** edits are allowed based on the document's lifecycle stage:

| Stage | What can change | Rule |
|-------|----------------|------|
| **Planning** — specs not yet implemented against | Everything in `epics/` | Any agent can freely update any spec, plan, or doc. Iterate fast. |
| **Implementation** — code is being written against specs | Specs (api.md, database.md, ui/, tests.md) | Specs are now reference docs. To change a spec: create an ADR first, then update the spec and add `changed: YYYY-MM-DD — ADR-NNN` to frontmatter. This ensures the implementer and reviewer see the change. |
| **Implementation** — progress tracking | Phase plans, CHECKLIST.md, EPIC.md, story files | Always allowed — check off tasks, add PRs, update statuses. |
| **Post-merge** — code is in production | Epic docs | Historical. Don't modify. Update `services/` with final docs. New work = new phase or new epic. |

**Key principle: Never silently change a spec that someone is implementing against. Always leave a trail.**

### Non-Engineering Roles

Engineers optimize for technical correctness. These roles bring different perspectives that catch different classes of problems. Each agent should be prompted to think from their specific angle:

**Product Owner (PO)** — Optimizes for user value:
- "Is this what users actually need? What problem does this solve?"
- "We don't need this for MVP — defer to next phase"
- "This feature serves 80% of users, that one serves 5%"

**QA** — Thinks adversarially about what can go wrong:
- "What happens if I paste 10,000 characters into this field?"
- "What if the network drops mid-submit? What's the recovery?"
- "The test plan covers the happy path — what about these 12 edge cases?"

**Security** — Thinks adversarially about what can be exploited:
- "What if I call this endpoint without auth? What about with another user's token?"
- "This API returns the full user object — does the UI actually need the email and address?"
- "Where's the input validation? This is a SQL injection vector."

**User Tester** — Tests the actual experience, not the spec:
- "I can't figure out how to do X without reading documentation"
- "This error message says 'Error 422' — that tells me nothing"
- "This flow makes sense on desktop but is unusable on mobile"

These roles operate at **two gates**: after specs (catch it cheap) and after implementation (catch it before merge). Review agents run in parallel since they're evaluating independently.

### Team Playbook: Planning

Create a team for the epic. Spawn agents with task dependencies:

**Phase 1 — Requirements & Breakdown:**

| Role | Task | Reads | Writes | Depends on |
|------|------|-------|--------|------------|
| PO | Validate requirements reflect real user needs. Write stories with acceptance criteria from user perspective. Challenge scope — what's MVP vs nice-to-have? Ensure each phase delivers user-visible value. | EPIC.md, REQUIREMENTS.md, user input | REQUIREMENTS.md, stories/*.md, EPIC.md (success criteria) | — |
| Planner | Break down epic into features and phases | EPIC.md, REQUIREMENTS.md, stories, user input | EPIC.md, phases/*.md, features/INDEX.md | PO |

**Phase 2 — Specs:**

| Role | Task | Reads | Writes | Depends on |
|------|------|-------|--------|------------|
| Spec Writer | Write specs per feature: UI → API → Database | REQUIREMENTS.md, stories, architecture.md, phase plans | specs/api.md, specs/database.md, specs/ui/*.md, specs/transformations.md | Planner |
| Test Planner | Write test plan from all specs | All specs | specs/tests.md | Spec Writer |

**Phase 3 — Spec Review Gate (parallel):**

| Role | Task | Reads | Writes | Depends on |
|------|------|-------|--------|------------|
| PO | Review specs from user perspective. Do UI specs match user expectations? Flag anything technically correct but user-hostile. | All specs, stories, REQUIREMENTS.md | Review comments, stories (refine acceptance criteria) | Test Planner |
| QA | Review test plan for gaps. Identify missing edge cases, boundary conditions, error recovery scenarios, negative tests. Add missing test cases. | specs/tests.md, all specs | specs/tests.md (additional cases) | Test Planner |
| Security | Review specs for vulnerabilities. Check auth/authz on every endpoint. Check for data over-exposure in API responses. Check input validation rules. Check database constraints. | specs/api.md, specs/database.md, architecture.md | Review comments, specs/tests.md (security test cases) | Test Planner |

PO, QA, and Security review in parallel after specs and test plan are complete. All findings must be addressed before implementation begins.

### Team Playbook: Implementation

**Build (sequential):**

| Role | Task | Reads | Writes | Depends on |
|------|------|-------|--------|------------|
| TDD Writer | Write failing tests for phase {N} scope. Confirm RED. | specs/tests.md, specs/api.md, PROJECT.md | Test code | — |
| Implementer | Write minimum code to pass tests. Confirm GREEN. Refactor. | Failing tests, all specs, PROJECT.md | Implementation code | TDD Writer |
| Reviewer | Validate implementation matches specs. Check coverage. | Diff, specs, test results | Review comments, CHECKLIST.md | Implementer |

**Pre-PR Review Gate (parallel):**

| Role | Task | Reads | Writes | Depends on |
|------|------|-------|--------|------------|
| QA | Exploratory testing: try to break it beyond the test plan. Test error recovery, boundary values, concurrent usage, unexpected input. | Implementation, specs, test results | Bug reports, additional test cases | Reviewer |
| Security | Security review: check implementation for OWASP top 10. Verify auth is enforced. Check for data leaks, injection, insecure defaults. Verify input validation. | Implementation diff, specs/api.md, architecture.md | Security findings, additional test cases | Reviewer |
| User Tester | UX validation: can a user actually accomplish the task? Test the flow end-to-end as a user would. Check error messages are helpful. Check accessibility basics. | UI specs, stories, running application | UX findings, usability issues | Reviewer |
| PO | Acceptance review: does the implementation deliver the user value in stories? Incorporate QA/Security/UX findings into accept/reject decision. | Stories, all review findings | Story status, accept/reject | QA, Security, User Tester |

1. TDD Writer writes tests → confirms they fail (RED)
2. Implementer writes code → confirms tests pass (GREEN)
3. Implementer refactors → confirms still GREEN
4. Reviewer validates against specs (technical review)
5. QA + Security + User Tester review in parallel (adversarial review)
6. PO makes accept/reject decision incorporating all review findings
7. PR is created

### Team Playbook: Bug Investigation

PO and Security triage in parallel. Investigators work after triage. Fixer waits for root cause.

| Role | Task | Reads | Writes | Depends on |
|------|------|-------|--------|------------|
| PO | Triage: assess user impact and priority. How many users affected? Is there a workaround? | Bug report, EPIC.md | INVESTIGATION.md (user impact) | — |
| Security | Assess security implications. Is this exploitable? Is data exposed? Does this need an incident response? | Bug report, specs/api.md, architecture.md | INVESTIGATION.md (security assessment) | — |
| Investigator | Reproduce bug, check logs/traces, document findings | INVESTIGATION.md, PROJECT.md, source code | INVESTIGATION.md, logs/ | PO, Security |
| Code Tracer | Trace code path from entry to bug. Map data flow. | Source code, INVESTIGATION.md | INVESTIGATION.md | PO, Security |
| Fixer | Write regression test (RED). Implement fix (GREEN). Run all tests. | INVESTIGATION.md, SOLUTION.md | Test code, fix code, CHECKLIST.md | Investigator, Code Tracer |
| Security | Verify fix doesn't introduce new vulnerabilities. Verify the original security concern is addressed. | Fix diff, SOLUTION.md | Review comments | Fixer |

### Context Loading for Agents

When spawning an agent, tell it which files to read and how to think:

```
For PO:
  Read: EPIC.md, REQUIREMENTS.md, stories/*.md, features/{feature}/specs/ui/
  Prompt: "You are the Product Owner. Think from the user's perspective, not the system's.
   Challenge whether we're building the right thing. Prioritize by user impact."

For QA:
  Read: specs/tests.md, specs/api.md, all specs, implementation code
  Prompt: "You are QA. Think adversarially — your job is to break things. Find edge cases,
   boundary conditions, and error scenarios the test plan missed. Test what happens when
   things go wrong, not just when they go right."

For Security:
  Read: specs/api.md, specs/database.md, architecture.md, implementation code
  Prompt: "You are a Security Researcher. Think like an attacker. Check every endpoint for
   auth/authz. Check for data over-exposure, injection vectors, insecure defaults,
   and missing input validation. Reference OWASP top 10."

For User Tester:
  Read: specs/ui/pages/*.md, specs/ui/flows.md, stories/*.md
  Prompt: "You are a User Tester. You have no technical knowledge. Try to accomplish the
   task described in the user story. Flag anything confusing, unclear, or requiring
   knowledge a normal user wouldn't have. Check error messages, accessibility, and flow."

For planning:
  Read: EPIC.md, REQUIREMENTS.md, stories/*.md, features/INDEX.md

For spec writing:
  Read: REQUIREMENTS.md, stories/{story}.md, features/{feature}/architecture.md,
   features/{feature}/specs/

For implementation:
  Read: features/{feature}/architecture.md, features/{feature}/specs/tests.md,
   features/{feature}/specs/api.md, features/{feature}/phases/phase{N}.md,
   services/{service}/PROJECT.md

For bug investigation:
  Read: bugs/{id}/INVESTIGATION.md, services/{service}/PROJECT.md
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
2. For each feature, create `architecture.md` from `references/architecture-template.md` — document current state, target state, and changes required
3. Create specs (UI first → API → Database → Tests):
   - `specs/ui/INDEX.md` + per-page specs in `specs/ui/pages/` from `references/ui-page-template.md`
   - `specs/ui/flows.md` — navigation flows (if >1 page)
   - `specs/ui/state.md` — global state structure (if applicable)
   - `specs/api.md` from `references/api-spec-template.md`
   - `specs/database.md` from `references/database-spec-template.md`
   - `specs/tests.md` from `references/tests-spec-template.md`
   - `specs/transformations.md` if data mapping is needed
4. Define phases in `features/{feature}/phases/`:
   - `phase1.md`, `phase2.md` etc. from `references/phase-template.md`
5. Validate UI/API contract: every UI data field maps to an API response field
6. Run pre-implementation checklist (below)
7. Update CHECKLIST.md — check off planning items

**Pre-Implementation Checklist:**
- [ ] CHECKLIST.md exists and planning items checked off
- [ ] REQUIREMENTS.md complete
- [ ] decisions/INDEX.md exists, key decisions recorded as ADRs
- [ ] features/INDEX.md lists all features
- [ ] Phases defined with milestones in EPIC.md
- [ ] Per feature: architecture.md documents design decisions
- [ ] Per feature: specs/tests.md exists with test cases
- [ ] Per feature: specs/api.md has all endpoints
- [ ] Per feature: specs/ui/pages/ has page specs (if applicable)
- [ ] Per feature: specs/ui/flows.md exists (if >1 page)
- [ ] Per feature: specs/database.md has all tables (if applicable)
- [ ] UI/API contract validated — all UI data fields match API response fields
- [ ] Phase plans exist with task breakdowns

### "Implement Phase N"

1. Read phase plan at `features/{feature}/phases/phaseN.md`
2. Read `features/{feature}/architecture.md` for design context
3. Read feature specs in `features/{feature}/specs/`
4. Read REQUIREMENTS.md for constraints
5. Read `services/{service}/PROJECT.md` for tech stack and conventions
6. **TDD Checkpoint:**
   - Does `specs/tests.md` exist? If not → write it first
   - Write test code for this phase's scope
   - Run tests → They MUST fail (RED)
7. **Only after tests fail:** Implement to make tests pass (GREEN)
8. **Run tests and verify output** → Must pass. Fix until green.
9. Refactor while keeping tests green
10. **E2E test:** Write at least 1 happy path E2E test verifying the full flow works end-to-end
11. Create PR (< 2000 lines, < 30 files)
12. Update tracking:
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

1. If a team is active: check the task list for pending, blocked, or in-progress tasks. Present team status.
2. Read `epics/INDEX.md` for in-progress epics
3. Find phases with status `in-progress` or next `planning` phase
4. Present options to user

### "Complete Phase" / "PRs Merged"

1. Update `phases/phaseN.md`: check off all completed tasks, add merged PRs to the PR table, update status
2. Update EPIC.md: story status table, phase status in Phases Overview
3. Update story files: frontmatter `status:` → done
4. Check off CHECKLIST.md phase completion items
5. If team is active: mark completed tasks as done, check for newly unblocked work

**Why this matters:** Future sessions load these files for context. Outdated status wastes tokens and causes confusion. Every status field must reflect reality.

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
| CLAUDE.md | `references/claude-md-template.md` |
| EPIC.md | `references/epic-template.md` |
| features/{name}/architecture.md | `references/architecture-template.md` |
| CHECKLIST.md | `references/checklist-template.md` |
| REQUIREMENTS.md | `references/requirements-template.md` |
| stories/{id}.md | `references/story-template.md` |
| phases/phaseN.md | `references/phase-template.md` |
| decisions/ADR-NNN.md | `references/adr-template.md` |
| specs/api.md | `references/api-spec-template.md` |
| specs/database.md | `references/database-spec-template.md` |
| specs/tests.md | `references/tests-spec-template.md` |
| specs/ui/pages/{page}.md | `references/ui-page-template.md` |
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
