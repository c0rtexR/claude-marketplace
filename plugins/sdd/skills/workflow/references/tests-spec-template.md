# {Feature Name} — Test Plan

## Test Categories

### 1. Unit Tests — {Area}

Source: `specs/{spec-file}.md`

| Test Case | Input | Expected | Priority |
|-----------|-------|----------|----------|
| it should {behavior} when {condition} | {input} | {output} | P0 |
| it should reject {invalid input} | {input} | {error} | P0 |
| it should handle empty {thing} | — | {default} | P1 |

### 2. Integration Tests

| Test Case | Components | Expected | Priority |
|-----------|------------|----------|----------|
| {API} returns correct data from {DB} | API → DB | {shape} | P0 |
| {Error} propagates correctly | A → B | {handling} | P1 |

### 3. E2E Tests

| Scenario | Steps | Expected | Priority |
|----------|-------|----------|----------|
| Happy path: {flow} | 1. ... 2. ... 3. ... | {result} | P0 |

## Acceptance Criteria Coverage

| Acceptance Criterion | Test Case(s) | Status |
|---------------------|--------------|--------|
| {AC from story} | {test name} | Covered |

## Test Fixtures

| Fixture | Purpose | Used By |
|---------|---------|---------|
| {fixture} | {what it provides} | Tests 1, 2 |
