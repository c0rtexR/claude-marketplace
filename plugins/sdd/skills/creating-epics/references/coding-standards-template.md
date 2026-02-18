# Coding Standards

> These rules are derived from real bugs found in this project. Each rule prevents an entire class of future mistakes. Treat them as non-negotiable.

## How to Use This Document

- **Before writing code:** Read the relevant sections
- **During code review:** Check against these rules
- **When you find a bug:** Check if an existing rule covers it. If not, add a new rule (see "Adding New Standards" below)

---

## Rules

### CS-001: {Category} — {Short Name}

**Bug:** {What went wrong — the actual bug that motivated this rule}

**Rule:** {The specific, actionable rule to follow}

**How to comply:**
- {Step 1}
- {Step 2}

| Bad | Good |
|-----|------|
| `{code that violates the rule}` | `{code that follows the rule}` |

---

### CS-002: {Category} — {Short Name}

**Bug:** {What went wrong}

**Rule:** {The rule}

**How to comply:**
- {Step 1}

| Bad | Good |
|-----|------|
| `{bad}` | `{good}` |

---

## Common Categories

| Category | What It Covers |
|----------|---------------|
| Selector Safety | DOM selectors, test locators, CSS targeting |
| API Contracts | Request/response shapes, HTTP methods, status codes |
| Data Integrity | Unique constraints, validation, sanitization |
| Async Patterns | WebSocket handling, timeouts, race conditions |
| Type Safety | TypeScript strictness, `any` avoidance, type narrowing |
| State Management | Frontend state, stale closures, re-render safety |
| Test Reliability | Flaky test prevention, deterministic assertions |
| Environment | Config differences, database setup, port conflicts |

## Enforcement

1. **PO review:** PO checks PRs against these standards before accepting
2. **Bug logging:** Every bug gets mapped to a coding standard category. If none fits, create a new rule
3. **Agent protocol:** Dev agents MUST read this file before writing code (referenced in `docs/agent-protocol.md`)

## Adding New Standards

When you discover a new class of bug:

1. Assign the next `CS-NNN` number
2. Pick a category from the table above (or create a new one)
3. Fill in: Bug (what happened), Rule (what to do), How to comply, Bad/Good examples
4. Add the rule to this document in the appropriate section
5. Reference the bug ID if one exists
