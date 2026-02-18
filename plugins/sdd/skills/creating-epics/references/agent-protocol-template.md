# Agent Protocol

> Every dev agent MUST read this document before writing code. This is the verification contract — the definition of "done" for agent-driven development.

## Definition of Done

A feature is done when ALL of these are true:

- [ ] Playwright E2E test walks through the user journey (click → database → back)
- [ ] `tsc --noEmit` = 0 errors (both backend and frontend)
- [ ] `eslint` = 0 errors
- [ ] ALL non-skipped tests pass (unit, integration, E2E)
- [ ] No uses of `any` — use correct type or `unknown` with narrowing
- [ ] No `eslint-disable` comments added
- [ ] Evidence pasted (actual terminal output, not summaries)

## TDD Rules

### Tests Are Source of Truth

**NEVER modify test assertions to make failing tests pass.** Fix the application code instead.

Only 3 valid exceptions:
1. **Test bug:** The test itself has a logic error (wrong expected value, wrong setup)
2. **Spec changed:** The requirement changed with PO approval — update test to match new spec
3. **Feature removed:** Feature was explicitly removed from scope

If you're tempted to change a test assertion, STOP and ask: "Is the test wrong, or is my code wrong?" Default assumption: your code is wrong.

### The Cycle

```
RED   → Write a failing test from specs/tests.md
GREEN → Write minimal code to pass it
REFACTOR → Clean up while green
COMMIT → Commit at every green state
REPEAT
```

## Zero-Tolerance Gates

Before claiming any task is done, run these commands and paste the output:

```bash
# TypeScript — ZERO errors
npx tsc --noEmit

# ESLint — ZERO errors
npx eslint src/ tests/

# Tests — ALL pass
{project-specific test command}
```

**"Pre-existing errors" is not an excuse.** If you encounter pre-existing errors, fix them. They are now your responsibility.

**NEVER use `any` to fix a TypeScript error.** Use the correct type. If the type is complex, use `unknown` with type narrowing:

| Bad | Good |
|-----|------|
| `const data: any = response.json()` | `const data: unknown = response.json()` |
| `(item as any).name` | `if ('name' in item) { item.name }` |

## E2E Test Requirement

### Why E2E Tests Are Non-Negotiable

| Bug Class | Unit Test Catches? | E2E Test Catches? |
|-----------|-------------------|-------------------|
| API returns wrong HTTP method (PUT vs PATCH) | No | Yes |
| CSS class not applied / wrong selector | No | Yes |
| WebSocket message shape mismatch | No | Yes |
| Response JSON shape doesn't match frontend expectation | No | Yes |
| Form submission wired to wrong button | No | Yes |
| Auth token not passed in header | No | Yes |
| Database migration missing a column | No | Yes |
| Frontend state not updated after API call | No | Yes |

**Rule:** Every feature MUST have at least 1 happy-path E2E test that:
1. Starts from the UI (browser interaction)
2. Triggers the API (real HTTP request)
3. Hits the database (real query)
4. Verifies the result back in the UI

### Evidence Format

When claiming a task is done, paste the actual terminal output:

```
# REQUIRED — paste actual output, not a summary
$ npx playwright test tests/e2e-browser/{test-file}.spec.ts

  Running 5 tests using 3 workers

  ✓ [1/5] should create a new board (2.3s)
  ✓ [2/5] should add a card (1.8s)
  ✓ [3/5] should vote on a card (2.1s)
  ✓ [4/5] should change phase (1.5s)
  ✓ [5/5] should show results (1.9s)

  5 passed (9.6s)
```

**Not acceptable:** "Tests pass", "All green", "Verified locally"

## "Blocked" Protocol

```
Is the code I need in THIS repository?
├── YES → Build it yourself. You are NOT blocked.
│         "Backend not built yet" = YOUR job to build it.
│         "API doesn't exist" = YOUR job to create it.
│         "Database table missing" = YOUR job to migrate it.
│
├── NO (different repo/service) → Valid blocker.
│   Document: what you need, from where, who owns it.
│
└── DESIGN UNCLEAR → Not blocked. Create ADR, propose design, ask PO.
```

**Things that are NEVER valid blockers in a monolith:**
- "Backend endpoint doesn't exist yet"
- "Database schema not created"
- "WebSocket handler not implemented"
- "Auth middleware not set up"
- "Types not defined yet"

## Task Lifecycle

1. **Receive task** with: exact file:line, root cause, exact fix, verification command
2. **Read context:** `docs/coding-standards.md`, relevant specs, architecture docs
3. **Write failing test** (RED)
4. **Implement fix/feature** (GREEN)
5. **Run zero-tolerance gates** (tsc, eslint, all tests)
6. **Paste evidence** (actual terminal output)
7. **Update task status** → completed
