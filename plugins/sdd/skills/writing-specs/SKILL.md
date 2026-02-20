---
name: writing-specs
description: "Use when documenting what to build before coding — writing architecture docs, UI specs, API specs, database schemas, test plans, or phase plans. Also triggers on: 'plan epic', 'write specs', 'prepare for implementation', 'design feature'."
---

# Writing Specs

## Overview

Document the UI first, design the API to serve it, design the database to store it, write the test plan to verify it. This order ensures everything fits together before a single line of code is written.

**Core principle:** Specs are the contract. Write them before code. Change them through ADRs during implementation.

## When to Use

- After creating an epic (features identified, ready to plan)
- User says "plan this", "write specs", "prepare for implementation"
- Designing a new feature within an existing epic
- Need to update specs during implementation (create ADR first)

## Step 0: Create Task List

**Before doing anything else**, create a task for each step so nothing gets lost:

1. Document architecture (current → target state)
2. Write UI specs (per page)
3. Write API specs (endpoints, contracts)
4. Run contract validation (forward + reverse checks)
5. Write database specs (tables, relationships, migrations)
6. Write test plan (unit, integration, E2E)
7. Additional specs if needed (transformations)
8. Define phases (vertical slices with task breakdowns)
9. Pre-implementation validation checklist

## The Spec Order

```
UI specs → API specs → Database specs → Test plan → Phase plans
```

**Why this order:**
1. UI defines what the user sees and needs
2. API serves the UI — every UI data field must map to an API response
3. Database stores what the API needs
4. Tests verify the contracts between all layers
5. Phases break the work into implementable chunks

## Step 1: Document Architecture

For each feature, create `architecture.md` from `references/architecture-template.md`:

- **Current state** — what exists today (diagram + component table)
- **Target state** — what it should look like after (diagram + component table)
- **Changes required** — delta between current and target
- **Key design decisions** — why this approach
- **Risks** — what could go wrong

Use ASCII diagrams:
```
[Browser] → [API Gateway] → [Auth Service]
                          → [Feature Service] → [Database]
```

## Step 2: Write UI Specs

**If a UDR exists** (from `sdd:ui-designing`): The chosen visualization and visual semantics from the UDR become the wireframe and component basis for the page spec. Reference the UDR ID.

**If a CRR exists** (from `sdd:component-research`): Component decisions (reuse/extend/build, library choices) inform the Components table. Reference the CRR ID.

Create per-page specs in `specs/ui/pages/` from `references/ui-page-template.md`:

Each page spec includes:
- **Purpose and URL**
- **ASCII wireframe** — layout with component positions
- **Components table** — name, type, data source, interactions
- **State matrix** — Loading, Empty, Error, Populated states
- **User interactions** — click, type, submit actions
- **Data requirements** — what data this page needs from the API

Also create:
- `specs/ui/INDEX.md` — overview of all pages
- `specs/ui/flows.md` — navigation flows between pages (if >1 page)
- `specs/ui/state.md` — global state structure (if applicable)

## Step 3: Write API Specs

Create `specs/api.md` from `references/api-spec-template.md`:

- **Base path and authentication**
- **Endpoints table** — Method, Path, Description, Auth Required
- **Per-endpoint details** — query params, request body, response body, error codes
- **UI/API contract mapping** — every UI data field maps to an API response field
- **TypeScript response types** — define interfaces for every response shape in the spec

**Validation checkpoint:** Walk through each UI page. Can every piece of data shown be served by the API? If not, the API spec is incomplete.

### Contract Validation (Mandatory)

After writing the API spec, run both checks:

**Forward check** — every UI field has an API source:

| UI Page | UI Field | API Endpoint | Response Field |
|---------|----------|-------------|----------------|
| {page} | {field shown to user} | {GET /path} | {response.field} |

If any UI field has no API source → add the missing endpoint or field.

**Reverse check** — every API field has a UI consumer:

| API Endpoint | Response Field | UI Consumer |
|-------------|----------------|-------------|
| {GET /path} | {response.field} | {page.component} |

If any API field has no consumer → remove it (don't over-fetch).

### Contract Audit Process

When changing a backend response shape during implementation:

1. Create ADR documenting the change and why
2. `grep` frontend code for all callers of the affected endpoint
3. Update the TypeScript response type
4. Update ALL frontend callers
5. Update the API spec
6. Run E2E tests to verify nothing broke

## Step 4: Write Database Specs

Create `specs/database.md` from `references/database-spec-template.md`:

- **Entity-Relationship diagram** (ASCII)
- **Tables** — columns, types, constraints, indexes
- **Relationships** — foreign keys, join tables
- **Migration plan** — how to get from current to target schema
- **API-to-Database mapping** — which API fields come from which tables

## Step 5: Write Test Plan

Create `specs/tests.md` from `references/tests-spec-template.md`:

- **Unit tests** — per-function behavior verification
- **Integration tests** — API endpoint tests, database queries
- **E2E tests** — full user flow tests
- **Acceptance criteria coverage** — map each acceptance criterion to test cases
- **Test fixtures** — shared test data

This file is the source of truth for TDD. Implementation starts by writing test code from this plan.

## Step 6: Additional Specs (If Needed)

- `specs/transformations.md` from `references/transformations-template.md` — data mapping between systems (field mapping table, transformation rules, validation rules)

## Step 7: Define Phases

Create phase plans in `features/{feature}/phases/` from `references/phase-template.md`:

Each phase is a coherent vertical slice:
- **Goal** — what this phase delivers
- **Scope** — specific features/endpoints/pages included
- **Tasks** — backend, frontend, testing broken down
- **Dependencies** — what must exist before this phase
- **PR target** — < 2000 lines, < 30 files per PR, 1-3 PRs per phase

## Step 8: Pre-Implementation Validation

**REQUIRED SUB-SKILL:** After this checklist passes, invoke `sdd:implementing-with-tdd` to begin implementation. Do not start coding without invoking the skill.

Run through this checklist before any coding begins:

- [ ] CHECKLIST.md exists and planning items checked off
- [ ] REQUIREMENTS.md complete
- [ ] Key decisions recorded as ADRs in `decisions/`
- [ ] features/INDEX.md lists all features with status
- [ ] Per feature: architecture.md documents current → target state
- [ ] Per feature: specs/tests.md exists with test cases
- [ ] Per feature: specs/api.md has all endpoints
- [ ] Per feature: specs/ui/pages/ has page specs (if applicable)
- [ ] Per feature: specs/database.md has all tables (if applicable)
- [ ] UI/API contract validated — all UI data fields match API response fields
- [ ] TypeScript response types defined in API spec for every endpoint
- [ ] Forward contract check done — every UI field maps to an API response field
- [ ] Reverse contract check done — every API response field has a UI consumer
- [ ] Phase plans exist with task breakdowns

## Creating ADRs During Implementation

Once implementation starts, specs become reference docs. To change a spec:

1. Create ADR in `decisions/` using `references/adr-template.md`
2. Update the spec
3. Add `changed: YYYY-MM-DD — ADR-NNN` to the spec's frontmatter
4. Inform anyone implementing against that spec

**Never silently change a spec that someone is implementing against.**

## Quick Reference

| Spec | Creates | Template | Key Content |
|------|---------|----------|-------------|
| Architecture | `architecture.md` | `references/architecture-template.md` | Current → target state, design decisions |
| UI Pages | `specs/ui/pages/{page}.md` | `references/ui-page-template.md` | Wireframe, components, state matrix |
| API | `specs/api.md` | `references/api-spec-template.md` | Endpoints, request/response, error codes |
| Database | `specs/database.md` | `references/database-spec-template.md` | Tables, relationships, migrations |
| Tests | `specs/tests.md` | `references/tests-spec-template.md` | Unit, integration, E2E test cases |
| Phases | `phases/phase{N}.md` | `references/phase-template.md` | Scope, tasks, PR targets |

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Writing API spec before UI spec | UI drives API. Reverse the order. |
| API returns data UI doesn't need | Trim API responses to match UI requirements. |
| UI needs data API doesn't provide | Add missing endpoint or field to API spec. |
| No test plan before implementation | Tests.md is mandatory. Write it from specs. |
| Phases too large (>2000 lines) | Split by layer (DB → API → UI) or feature slice. |
| Changing specs without ADR | Always create ADR first during implementation. |

## Integration

**Required before:**
- **sdd:creating-epics** — Epic structure MUST exist before writing specs (features identified, stories written)

**Required after:**
- **sdd:implementing-with-tdd** — REQUIRED after Step 8 (pre-implementation checklist passes). Invoke to begin TDD cycle.

**Called by:**
- **sdd:creating-epics** (Step 4) — After features identified
- **sdd:implementing-with-tdd** (TDD Checkpoint) — When specs/tests.md doesn't exist
- **sdd:workflow** — When user says "plan this", "write specs", "design feature"
- **sdd:ui-designing** — Visualization decided, feeds into UI specs
- **sdd:component-research** — Component decisions feed into UI specs and architecture

**Subagents should use:**
- **sdd:coordinating-agent-teams** — For parallelizing spec writing across agents
