# PR Template

## Title Format

`{type}: {concise description}`

Types: feat, fix, refactor, test, docs, chore, perf

## Body

### Summary

{1-2 sentences: what changed and why.}

### Changes

- {Key change 1}
- {Key change 2}

### Specs

- [API Spec](features/{feature}/specs/api.md)
- [Test Plan](features/{feature}/specs/tests.md)

### Phase

{Epic ID} — Phase {N}: {Goal}

### Acceptance Criteria Evidence

| AC | How Verified | Evidence |
|----|-------------|----------|
| {acceptance criterion} | {E2E test / unit test / manual} | {test file:line or screenshot} |

### E2E Test Output

```
# Paste actual terminal output here — not a summary
$ npx playwright test tests/e2e-browser/{test-file}.spec.ts

  {paste full output}
```

### TypeScript & Lint Check

```
$ npx tsc --noEmit
# {paste output — must show 0 errors}

$ npx eslint src/ tests/
# {paste output — must show 0 errors}
```

### Testing

- [ ] Unit tests added ({count})
- [ ] Integration tests added ({count})
- [ ] E2E tests added ({count})
- [ ] All existing tests pass
- [ ] E2E test output pasted above

### Checklist

- [ ] All tests pass (output pasted)
- [ ] `tsc --noEmit` = 0 errors (output pasted)
- [ ] `eslint` = 0 errors (output pasted)
- [ ] No `any` types introduced
- [ ] No `eslint-disable` comments added
- [ ] No debug statements
- [ ] Specs up to date
- [ ] Phase plan updated with PR link
