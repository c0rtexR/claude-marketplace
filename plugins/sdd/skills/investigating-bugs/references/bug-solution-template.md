---
id: "{BUG-ID}"
chosen_solution: 1
date_resolved: —
---

# {BUG-ID}: Solution

## Proposed Solutions

### Option 1 (Recommended): {Name}

**Description**: {What the fix does}

| Pros | Cons |
|------|------|
| {pro} | {con} |

### Option 2: {Name}

**Description**: {What the fix does}

| Pros | Cons |
|------|------|
| {pro} | {con} |

## Resolution

**Chosen**: Option {N} — {reason}

**Files Changed:**

| File | Change |
|------|--------|
| `{path}` | {what changed} |

**Pull Requests:**

| PR | Description | Status |
|----|-------------|--------|
| {link} | {description} | {merged/open} |

## Verification

- [ ] Regression test written (fails without fix)
- [ ] Regression test passes with fix
- [ ] All existing tests pass
- [ ] Deployed and verified in environment
- [ ] Bug no longer reproducible

## Lessons Learned

- **What caused it**: {root cause summary}
- **How to prevent**: {process/code improvement}
- **What to monitor**: {metric or alert to add}

## Coding Standard Impact

- **Category**: {Selector Safety / API Contracts / Data Integrity / Async Patterns / Type Safety / State Management / Test Reliability / Environment}
- **Existing rule violated**: {CS-NNN if applicable, or "None — new class of bug"}
- **New rule created**: {CS-NNN: {short name} — added to `docs/coding-standards.md`, or "N/A"}
- **Rule file**: `docs/coding-standards.md`
