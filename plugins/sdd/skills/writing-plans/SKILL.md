---
name: writing-plans
description: "Use when you have a spec, design, or requirements for a multi-step task and need a detailed implementation plan before touching code. Also triggers on: 'write plan', 'implementation plan', 'break this down', 'plan the tasks', 'task breakdown'."
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for the codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs they might need to check, how to test it. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume they are a skilled developer, but know almost nothing about the toolset or problem domain. Assume they don't know good test design very well.

**Announce at start:** "Using sdd:writing-plans to create the implementation plan."

**Save plans to:** `docs/plans/YYYY-MM-DD-<feature-name>.md`

## Bite-Sized Task Granularity

**Each step is one action (2-5 minutes):**
- "Write the failing test" — step
- "Run it to make sure it fails" — step
- "Implement the minimal code to make the test pass" — step
- "Run the tests and make sure they pass" — step
- "Commit" — step

## Plan Document Header

**Every plan MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use sdd:implementing-with-tdd to implement this plan task-by-task.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

**Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

**Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

**Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

**Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

## Remember

- Exact file paths always
- Complete code in plan (not "add validation")
- Exact commands with expected output
- DRY, YAGNI, TDD, frequent commits

## Execution Handoff

After saving the plan, offer execution choice:

**"Plan complete and saved to `docs/plans/<filename>.md`. Two execution options:**

**1. Direct (this session)** — I implement task-by-task using `sdd:implementing-with-tdd`, review between tasks

**2. Agent team (parallel)** — Use `sdd:coordinating-agent-teams` to dispatch agents for independent tasks

**Which approach?"**

**If Direct chosen:**
- **REQUIRED SUB-SKILL:** Use `sdd:implementing-with-tdd`
- Stay in this session
- Implement task-by-task with TDD discipline

**If Agent team chosen:**
- **REQUIRED SUB-SKILL:** Use `sdd:coordinating-agent-teams`
- Dispatch agents per task with full context
- Each agent follows `sdd:implementing-with-tdd`

## Red Flags

| Thought | Reality |
|---------|---------|
| "The plan doesn't need exact file paths" | It does. The implementer has zero context. |
| "They'll figure out the test" | Write the test in the plan. Assume bad test instincts. |
| "This step is obvious" | Write it anyway. Obvious to you is invisible to the agent. |
| "One big task is fine" | Break it down. Each step is 2-5 minutes. |
| "I'll add more detail later" | Add it now. The plan is the contract. |

## Integration

**Called by:**
- **sdd:brainstorming** — After design approval, brainstorming invokes this to create the implementation plan
- **sdd:workflow** — When user says "write plan", "implementation plan", "break this down"

**MUST invoke after:**
- **sdd:implementing-with-tdd** — REQUIRED. Implementation follows TDD discipline.
- OR **sdd:coordinating-agent-teams** — For parallel execution with agent teams.

**Feeds into:**
- **sdd:implementing-with-tdd** — The plan becomes the task list for TDD implementation
- **sdd:coordinating-agent-teams** — The plan's tasks become agent assignments

**Pairs with:**
- **sdd:brainstorming** — Brainstorming produces the design; writing-plans produces the implementation plan
- **sdd:writing-specs** — Specs provide the requirements; plans provide the task breakdown
