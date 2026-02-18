---
name: implementing-with-tdd
description: "Use when implementing a feature phase, writing code, creating PRs, or completing phases. Enforces test-first discipline within SDD workflow. Also triggers on: 'implement phase', 'start coding', 'create PR', 'complete phase'."
---

# Implementing with TDD

## Overview

Read the specs. Write tests that fail. Write code to pass them. Verify. Ship.

**Core principle:** No production code without a failing test first. No "done" without running tests and confirming output.

## When to Use

- Specs and phase plans exist, ready to implement
- User says "implement phase N", "start coding", "build this"
- Creating a PR after implementation
- Marking a phase as complete

## Before You Write Any Code

Read these files in order:

1. `features/{feature}/phases/phase{N}.md` — what to build in this phase
2. `features/{feature}/architecture.md` — design context and decisions
3. `features/{feature}/specs/tests.md` — test plan (your TDD source)
4. `features/{feature}/specs/api.md` — API contracts
5. `features/{feature}/specs/ui/pages/*.md` — UI specs (if applicable)
6. `services/{service}/PROJECT.md` — tech stack, conventions, key paths
7. `REQUIREMENTS.md` — hard constraints

## TDD Checkpoint

Before writing any implementation:

**Does `specs/tests.md` exist?**
- Yes → proceed
- No → STOP. **REQUIRED SUB-SKILL:** Invoke `sdd:writing-specs` to write the test plan first. Do not proceed without it.

## The Cycle: RED → GREEN → REFACTOR

### RED — Write a Failing Test

Write one test from `specs/tests.md` that describes desired behavior.

```
1. Pick the next test case from specs/tests.md
2. Write test code
3. Run it
4. Confirm it FAILS (not errors — fails because feature is missing)
```

**Test fails?** Good. Proceed to GREEN.
**Test passes?** You're testing existing behavior. Fix the test.
**Test errors?** Fix the error. Re-run until it fails correctly.

### GREEN — Minimal Code to Pass

Write the simplest code to make the test pass. Nothing more.

```
1. Write minimum implementation code
2. Run tests
3. Confirm the test PASSES
4. Confirm ALL other tests still pass
```

Don't add features. Don't refactor. Don't "improve" unrelated code. Just pass the test.

### REFACTOR — Clean Up While Green

After green:
- Remove duplication
- Improve names
- Extract helpers if needed

Keep running tests. Stay green. Don't add behavior.

### Commit and Repeat

Commit at every GREEN state. Pick the next test case. Start RED again.

## Closed-Loop Verification

**You MUST run tests yourself.** Never claim "done" without:
1. Executing the test command
2. Reading the output
3. Confirming all tests pass
4. Confirming no errors or warnings in output

If tests fail → fix and re-run until green. No exceptions.

## Tests Are Source of Truth

**NEVER modify test assertions to make failing tests pass.** Fix the application code instead.

Tests describe the intended behavior. If a test fails, the code is wrong — not the test. Only 3 valid exceptions:

| Exception | When It Applies | Action |
|-----------|----------------|--------|
| Test bug | Test has a logic error (wrong expected value, wrong setup) | Fix the test, document why |
| Spec changed | Requirement changed with PO approval | Update test to match new spec, reference ADR |
| Feature removed | Feature explicitly removed from scope | Delete the test |

**Default assumption:** Your code is wrong. If you're tempted to change a test assertion, STOP and ask: "Is the test wrong, or is my code wrong?"

## Zero-Tolerance Gates

Before claiming ANY task is done, run these and paste the output:

| Gate | Command | Requirement |
|------|---------|-------------|
| TypeScript | `npx tsc --noEmit` | 0 errors (both backend and frontend) |
| ESLint | `npx eslint src/ tests/` | 0 errors |
| Tests | `{project test command}` | ALL non-skipped tests pass |

**Rules:**
- "Pre-existing errors" is NOT an excuse — fix them. They are now your responsibility.
- NEVER use `any` to fix a TypeScript error. Use the correct type or `unknown` with type narrowing.
- NEVER add `eslint-disable` comments. Fix the underlying issue.

| Bad | Good |
|-----|------|
| `const data: any = res.json()` | `const data: unknown = res.json()` |
| `(item as any).name` | `if ('name' in item) { item.name }` |
| `// eslint-disable-next-line` | Fix the actual lint error |

## E2E Test Requirement

After all unit/integration tests pass for the phase:
- Write at least 1 happy-path E2E test verifying the full flow works end-to-end
- Run it. Confirm it passes.

### Why E2E Tests Are Non-Negotiable

Unit tests prove individual functions work. E2E tests prove the feature works for a real user. The real bugs are integration cracks:

| Bug Class | Unit Test Catches? | E2E Test Catches? |
|-----------|-------------------|-------------------|
| API returns wrong HTTP method (PUT vs PATCH) | No | Yes |
| CSS class not applied / wrong selector | No | Yes |
| WebSocket message shape mismatch | No | Yes |
| Response JSON shape doesn't match frontend | No | Yes |
| Form submission wired to wrong button | No | Yes |
| Auth token not passed in header | No | Yes |
| Database migration missing a column | No | Yes |
| Frontend state not updated after API call | No | Yes |

**Rule:** If you can't show a Playwright test walking through the user journey from click to database and back, the feature is NOT done.

For E2E test patterns and common pitfalls, see `references/e2e-patterns.md`.

### Evidence Format

When claiming done, paste **actual terminal output** — not a summary:

```
# REQUIRED — paste actual output
$ npx playwright test tests/e2e-browser/{test-file}.spec.ts

  Running 5 tests using 3 workers

  ✓ [1/5] should create a new board (2.3s)
  ✓ [2/5] should add a card (1.8s)
  ✓ [3/5] should vote on a card (2.1s)
  ✓ [4/5] should change phase (1.5s)
  ✓ [5/5] should show results (1.9s)

  5 passed (9.6s)
```

**Not acceptable:** "Tests pass", "All green", "Verified locally", "4 of 6 passing"

## Creating a PR

PR rules:
- **< 2000 lines** changed
- **< 30 files** changed
- **One deliverable** per PR, one phase per PR (1-3 PRs per phase)
- **Title format:** `{type}: {description}` — types: feat, fix, refactor, test, docs, chore, perf
- **Body:** Use `references/pr-template.md` — summary, changes list, spec references, testing evidence

If too big: split by layer (DB → API → UI) or by feature slice.

## Completing a Phase

After PR is created/merged:

1. Update `phases/phase{N}.md` — check off tasks, add PR to PR table, set status
2. Update EPIC.md — story status, phase status in Phases Overview
3. Update story files — frontmatter `status:` → done
4. Check off CHECKLIST.md phase completion items
5. Update features/INDEX.md status

**Why this matters:** Future sessions load these files for context. Outdated status wastes tokens and causes confusion.

## Quick Reference

| Step | What You Do | Verify |
|------|------------|--------|
| Load context | Read phase plan, architecture, specs, PROJECT.md | Understand scope |
| TDD checkpoint | Check specs/tests.md exists | Has test cases |
| RED | Write one failing test | Test fails for right reason |
| GREEN | Write minimal code | Test passes, all tests green |
| REFACTOR | Clean up | All tests still green |
| Commit | Commit at green state | Clean commit |
| Repeat | Next test case | All phase tests done |
| E2E | Write happy-path E2E test | Passes end-to-end |
| PR | Create PR (< 2000 lines) | Follows template |
| Track | Update phase, epic, stories, checklist | All statuses current |

## Red Flags — Stop and Correct

- Writing code before a test exists → delete code, write test first
- Test passes immediately → you're testing existing behavior, fix the test
- "I'll add tests after" → no. RED before GREEN, always
- "Too simple to test" → simple code breaks. 30 seconds for the test.
- PR over 2000 lines → split it
- Claiming "done" without running tests → run them, read output
- Skipping E2E test → write at least 1 happy path
- **Changing test assertions to match broken code** → fix the code, not the test
- **Using `any` to suppress TypeScript errors** → use correct type or `unknown` with narrowing
- **"4 of 6 passing"** → ALL non-skipped tests must pass. 4/6 is not done.
- **"Pre-existing errors"** → they're your errors now. Fix them.
- **Pasting a summary instead of terminal output** → paste actual `npx` output as evidence

## Integration

**Required before:**
- **sdd:writing-specs** — specs/tests.md MUST exist. If missing, invoke `sdd:writing-specs` first (see TDD Checkpoint).

**Called by:**
- **sdd:writing-specs** (Step 8) — After specs complete and pre-implementation checklist passes
- **sdd:workflow** — When user says "implement phase N", "start coding", "create PR"

**Pairs with:**
- **sdd:investigating-bugs** — Invoke when bugs are found during implementation. Bug skill's regression test follows the RED→GREEN cycle from this skill.
- **sdd:creating-epics** — For epic-level tracking and phase closure

**Subagents should use:**
- **sdd:coordinating-agent-teams** — For TDD Writer + Implementer role separation in teams
