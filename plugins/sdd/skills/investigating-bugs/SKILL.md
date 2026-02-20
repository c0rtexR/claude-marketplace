---
name: investigating-bugs
description: "Use when investigating bugs, fixing defects, or debugging issues within an SDD project. Enforces investigation-first discipline with mandatory regression tests. Also triggers on: 'investigate bug', 'fix bug', 'there's a bug', 'debug issue'."
---

# Investigating Bugs

## Overview

Investigate first. Fix second. Prove it with a test.

**Core principle:** Never jump to a fix. Document what's wrong, find the root cause, then write a regression test before implementing the fix.

## When to Use

- A bug is reported or discovered
- Tests are failing unexpectedly
- User says "fix this bug", "there's an issue", "this is broken"
- Something works in one environment but fails in another

## Step 0: Create Task List

**Before doing anything else**, create a task for each step so nothing gets lost:

1. Determine context (epic-scoped or standalone, create bug folder)
2. Investigate (symptoms, reproduce, map architecture, evidence, hypotheses)
3. Document root cause
4. Propose solutions (options with pros/cons, get user approval)
5. Fix with regression test (RED → GREEN)
6. Create PR and update tracking
7. Feed back to coding standards (new CS rule or note existing)

## Step 1: Determine Context

Is this bug part of an active epic or standalone?

**Epic-scoped:** Create folder at `epics/{EPIC-ID}-{slug}/bugs/{BUG-ID}/`
**Standalone:** Create folder at `bugs/{BUG-ID}/`

Create these files using templates from `references/`:

| File | Template |
|------|----------|
| INVESTIGATION.md | `references/bug-investigation-template.md` |
| SOLUTION.md | `references/bug-solution-template.md` |
| CHECKLIST.md | `references/bug-checklist-template.md` |
| logs/ | Directory for raw evidence |

## Step 2: Investigate (Do NOT Fix Yet)

Fill in INVESTIGATION.md as you go:

### Document Symptoms
- What's happening vs what's expected
- Error messages (exact text)
- Screenshots, log snippets → save in `logs/`

### Reproduce
- Exact steps to trigger the bug
- Does it happen every time?
- If intermittent → gather more data, don't guess

### Map Architecture
- Which services are involved?
- What's the data flow from input to bug?
- Draw it:
```
[User Input] → [API] → [Service] → [Database]
                         ↑ Bug is here
```

### Check Evidence
- Recent changes (git log, recent commits)
- Logs and metrics
- Environment differences (dev vs staging vs prod)

### Track Hypotheses

| # | Hypothesis | Evidence For | Evidence Against | Status |
|---|-----------|-------------|-----------------|--------|
| 1 | {theory} | {what supports it} | {what contradicts it} | testing / confirmed / rejected |

Test one hypothesis at a time. Don't fix multiple things at once.

## Step 3: Document Root Cause

Once confirmed, update INVESTIGATION.md with:
- **Root cause** — what's actually wrong
- **Code location** — exact file and line
- **Why it happened** — the underlying reason, not just the symptom

## Step 4: Propose Solutions

Fill in SOLUTION.md with options:

For each option:
- Description of the fix
- Pros and cons
- Files that would change
- Risk assessment

Get user approval on which option to implement.

## Step 5: Fix with Regression Test (Mandatory)

**RED:** Write a test that reproduces the bug
```
1. Write test that triggers the buggy behavior
2. Run it → confirm it FAILS (reproduces the bug)
```

**GREEN:** Implement the fix
```
1. Apply the chosen fix
2. Run the regression test → confirm it PASSES
3. Run ALL tests → confirm nothing else broke
```

Never fix a bug without a regression test. The test proves the fix works and prevents the bug from returning.

## Step 6: Create PR and Update Tracking

1. Create PR with fix + regression test
2. Update CHECKLIST.md — check off items
3. If epic-scoped: update epic tracking
4. After deploy: verify fix in production
5. Document lessons learned in SOLUTION.md

## Step 7: Feed Back to Coding Standards

Every bug is an opportunity to prevent an entire class of future mistakes. After fixing a bug:

### Check Existing Standards

Read `docs/coding-standards.md`. Does an existing rule cover this bug?
- **Yes** → Note the rule number in the bug's SOLUTION.md. The rule existed but wasn't followed — address in process review.
- **No** → Add a new rule (see below).

### Add a New Rule

If this bug represents a new class of mistake:

1. Assign the next `CS-NNN` number
2. Pick a category:

| Category | Examples |
|----------|---------|
| Selector Safety | Partial text match, wrong locator, missing data-testid |
| API Contracts | Wrong HTTP method, missing field, wrong response shape |
| Data Integrity | Missing unique constraint, no validation, SQL injection |
| Async Patterns | Short timeout, missing await, race condition |
| Type Safety | Using `any`, wrong cast, missing type narrowing |
| State Management | Stale closure, missing re-render, wrong dependency array |
| Test Reliability | Hardcoded data, flaky assertion, environment dependency |
| Environment | Wrong database URL, port conflict, missing env var |

3. Write the rule using this format:

```markdown
### CS-NNN: {Category} — {Short Name}

**Bug:** {What went wrong — the actual bug}

**Rule:** {The specific, actionable rule}

**How to comply:**
- {Step 1}
- {Step 2}

| Bad | Good |
|-----|------|
| `{code that violates}` | `{code that follows}` |
```

4. Add to `docs/coding-standards.md`
5. Reference the bug ID and rule in SOLUTION.md under "Coding Standard Impact"

### Log a Learning Entry (If Broader Pattern)

If this bug reveals a pattern beyond a single code rule — e.g., the same class of API mismatch keeps recurring, or environment issues keep causing debugging time — also create a learning entry:

1. Invoke `sdd:reflecting` in capture mode
2. Category is usually `bug-pattern`, but could be `environment`, `architecture`, etc.
3. Cross-reference with existing learnings — is this the 3rd occurrence?

Not every bug needs a learning entry. But if you think "we keep hitting this kind of thing", log it.

## Common Bug Types

| Type | Symptoms | Investigation Strategy |
|------|----------|----------------------|
| **Data flow** | Wrong/missing data | Trace source → processing → storage → retrieval |
| **Timing/Race condition** | Intermittent failures | Check async operations, concurrency, retries, ordering |
| **Integration** | A works, B works, A+B fails | Check contracts, schemas, API versions between services |
| **Configuration** | Works in one env, fails in another | Compare configs, env vars, feature flags, secrets |

## Quick Reference

| Phase | What You Do | Output |
|-------|------------|--------|
| Context | Epic-scoped or standalone? | Bug folder created |
| Investigate | Symptoms → Reproduce → Map → Evidence → Hypotheses | INVESTIGATION.md filled |
| Root cause | Confirm one hypothesis | Root cause documented |
| Solutions | Options with pros/cons | SOLUTION.md, user picks one |
| Regression test | Write test reproducing bug (RED) | Failing test |
| Fix | Implement fix (GREEN), run all tests | All tests pass |
| Ship | PR, tracking updates, deploy verification | Bug closed |

## Red Flags — Stop and Investigate More

- Jumping straight to a fix without understanding the bug
- "I think it's X, let me just change it" — form a hypothesis, test it
- Changing multiple things at once — one variable at a time
- Fix didn't work → don't add another fix on top, re-investigate
- No regression test → write one before fixing
- "Works on my machine" → check environment differences systematically

## Integration

**Called by:**
- **sdd:implementing-with-tdd** — When bugs are found during implementation
- **sdd:workflow** — When user says "fix this bug", "there's an issue", "debug"

**Uses internally:**
- **sdd:implementing-with-tdd** — Step 5 regression test follows the RED→GREEN cycle. The TDD discipline applies to bug fixes.

**Pairs with:**
- **sdd:creating-epics** — For epic-level bug tracking
- **sdd:coordinating-agent-teams** — For team-based bug investigation playbook (Triage → Investigate → Fix → Verify)
- **sdd:reflecting** — Step 7 prompts for learning entry when a bug reveals a broader pattern
