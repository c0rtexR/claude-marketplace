# {EPIC-ID}: Lifecycle Checklist

> Skip protocol: Items are done `[x]` or skipped `[x] ~~Item~~ — SKIPPED: {reason} (approved by: {name})`. Never leave unchecked.

## Planning

- [ ] EPIC.md created with phases and success criteria
- [ ] REQUIREMENTS.md complete (FR + NFR)
- [ ] features/INDEX.md lists all features
- [ ] decisions/INDEX.md created

## Specifications (per feature)

### Feature: {feature-name}

**Architecture:**
- [ ] architecture.md created with design decisions

**UI Specs:**
- [ ] specs/ui/INDEX.md created
- [ ] Page specs written in specs/ui/pages/ for each view
- [ ] specs/ui/flows.md documents navigation between pages (if >1 page)
- [ ] specs/ui/state.md documents global state structure (if applicable)
- [ ] Loading and error states documented in each page's State Matrix

**API / Data Specs:**
- [ ] specs/api.md has all endpoints with request/response shapes
- [ ] specs/database.md has all table schemas
- [ ] specs/transformations.md has data mappings (if applicable)
- [ ] UI/API contract validated — all UI fields match API fields

**Test Plan:**
- [ ] specs/tests.md complete with test cases
- [ ] Test cases cover all API endpoints
- [ ] Test cases cover edge cases from transformations
- [ ] Test cases cover UI interactions

## Pre-Implementation Validation

- [ ] All spec files consistent with each other
- [ ] Phase plans created with task breakdowns
- [ ] No unresolved UI/API mismatches
- [ ] Architecture decisions recorded as ADRs

## Phase Completion (repeat per phase)

### Phase {N}: {Goal}

- [ ] Tests written and failing (RED)
- [ ] Implementation passes all tests (GREEN)
- [ ] Code refactored, tests still green
- [ ] E2E happy path test passes
- [ ] PR created (< 2000 lines)
- [ ] PR merged
- [ ] Phase plan updated with PR links
- [ ] EPIC.md story/phase status updated
- [ ] Story files updated (status → done)
- [ ] ADRs synced if new decisions made

## Closure

- [ ] All phases complete
- [ ] All success criteria met
- [ ] Feature documentation created in services/
- [ ] ADRs synced to service decisions/
- [ ] EPIC.md status → complete
- [ ] epics/INDEX.md updated
