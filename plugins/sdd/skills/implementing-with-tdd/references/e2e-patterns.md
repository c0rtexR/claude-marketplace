# E2E Test Patterns

> Reference document for writing reliable Playwright E2E tests. These patterns are derived from real failures across multiple projects.

## Selector Priority

Use the most specific selector available. Prefer selectors that are resilient to UI changes:

| Priority | Selector Type | When to Use | Example |
|----------|--------------|-------------|---------|
| 1 (Best) | `data-testid` | Custom attribute for testing | `page.getByTestId('submit-btn')` |
| 2 | `getByRole` | Semantic HTML elements | `page.getByRole('button', { name: /submit/i })` |
| 3 | `getByLabel` | Form fields with labels | `page.getByLabel('Email')` |
| 4 | Exact text | Unique, stable text | `page.getByText('No results found', { exact: true })` |
| 5 | Regex text | Partial text (use carefully) | `page.getByText(/submit/i)` |
| 6 (Avoid) | CSS selector | Only when nothing else works | `page.locator('.btn-primary')` |

**Key rule:** `getByText` with partial match in strict mode hits multiple elements. Always use `{ exact: true }`, `getByRole`, or `.first()` to disambiguate.

## Common Patterns

### Form Submission — Button Click, Not Keyboard

```typescript
// BAD — keyboard Enter doesn't always trigger form submission
await page.getByLabel('Card text').fill('My card');
await page.keyboard.press('Enter');

// GOOD — click the submit button explicitly
await page.getByLabel('Card text').fill('My card');
await page.getByRole('button', { name: /^add card$/i }).click();
```

**Why:** Some forms don't have `onSubmit` handlers on the `<form>` element. Button click is the reliable path.

### WebSocket Waits — Long Timeouts Under Load

```typescript
// BAD — 5 seconds is too short when 6 workers hit the server
await expect(page.getByText('New card')).toBeVisible({ timeout: 5000 });

// GOOD — 10-15 seconds accounts for parallel test load
await expect(page.getByText('New card')).toBeVisible({ timeout: 15000 });
```

**Why:** WebSocket broadcasts under parallel test load (6+ workers) can take 10-15 seconds.

### Multi-User Contexts — Unique Data Per Worker

```typescript
// BAD — hardcoded emails collide across parallel workers
const email = 'test@example.com';

// GOOD — random suffix prevents cross-worker collisions
function generateUniqueEmail(): string {
  const timestamp = Date.now();
  const random = Math.random().toString(36).substring(2, 8);
  return `test-${timestamp}-${random}@example.com`;
}
```

**Why:** Parallel workers share the database. Unique emails/usernames prevent constraint violations.

### Reload Persistence — Verify Database Round-Trip

```typescript
// After creating data, reload and verify it persists
await page.getByRole('button', { name: /^add card$/i }).click();
await expect(page.getByText('My card')).toBeVisible({ timeout: 10000 });

// Reload — data should survive
await page.reload();
await expect(page.getByText('My card')).toBeVisible({ timeout: 10000 });
```

**Why:** Catches cases where data is only in frontend state but never saved to database.

### Phase/State Transitions — Use Explicit UI Controls

```typescript
// BAD — "Next Phase" button might not exist or be ambiguous
await page.getByText('Next Phase').click();

// GOOD — use the specific phase stepper control
await page.getByText('Vote').click(); // Select target phase
await page.getByRole('button', { name: /change phase/i }).click(); // Confirm
```

**Why:** Phase transition UIs often have multi-step flows. Click the specific controls, not generic buttons.

### Dismissing Overlays Before Interaction

```typescript
// If an icebreaker/welcome modal appears, dismiss it first
const startWriting = page.getByRole('button', { name: /start writing/i });
if (await startWriting.isVisible({ timeout: 3000 }).catch(() => false)) {
  await startWriting.click();
}

// Now interact with the board
await page.getByLabel('Card text').fill('My card');
```

**Why:** Overlays block interaction with elements beneath them. Always dismiss before proceeding.

### Scoped vs Global Locators

```typescript
// BAD — scoped locator finds nothing if element is outside the scope
const card = page.getByText('My card');
await card.getByRole('button', { name: 'Vote' }).click(); // Vote button might not be inside card

// GOOD — use global locator when element position is uncertain
await page.locator('button[aria-label="Vote"]').first().click();
```

**Why:** Component boundaries in React don't always match DOM nesting. Use global locators when the element's DOM position is uncertain.

## Common Pitfalls

| Pitfall | Symptom | Fix |
|---------|---------|-----|
| Partial text match | "strict mode violation: getByText resolved to 2 elements" | Use `{ exact: true }`, `getByRole`, or `.first()` |
| Short timeout | Test passes locally, fails in CI | Increase to 10-15s for WebSocket/async operations |
| Hardcoded test data | "unique constraint violation" in parallel runs | Use `generateUniqueEmail()` with random suffix |
| Keyboard submit | Form doesn't submit | Use button click instead of `Enter` |
| Missing await | Flaky pass/fail pattern | Always `await` Playwright actions and assertions |
| Element not interactable | "Element is not visible" | Dismiss overlays/modals first |
| Stale element reference | "Element was detached from DOM" | Re-query the element after navigation/reload |
| Race condition on data | Assertion runs before data arrives | Use `toBeVisible({ timeout })` not immediate assertions |

## Strict Mode Fixes

Playwright strict mode throws when a locator matches multiple elements. Fixes:

```typescript
// Fix 1: Use exact match
page.getByText('Participation', { exact: true })

// Fix 2: Use getByRole for semantic matching
page.getByRole('heading', { name: 'Participation' })

// Fix 3: Add data-testid to the component
page.getByTestId('participation-heading')

// Fix 4: Use .first() when you want the first match
page.getByText('Participation').first()

// Fix 5: Scope to a container
page.getByTestId('analytics-panel').getByText('Participation')
```

**Preference order:** `data-testid` > `getByRole` > `exact: true` > scoped > `.first()`
