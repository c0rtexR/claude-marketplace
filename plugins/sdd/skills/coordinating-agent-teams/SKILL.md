---
name: coordinating-agent-teams
description: "Use when running multi-agent teams for SDD work — coordinating planners, spec writers, TDD writers, implementers, and reviewers. Also triggers on: 'use team', 'spawn agents', 'agent team', 'coordinate agents', 'team playbook'."
---

# Coordinating Agent Teams

## Overview

Agents coordinate through the folder structure and document lifecycle. The orchestrator creates teams, assigns roles, and ensures agents work within the SDD methodology.

**Core principle:** The folder structure IS the coordination mechanism. Any agent can read and write any file — coordination comes from task dependencies and document lifecycle rules.

## Step 0: Create Task List

**Before doing anything else**, create a task for each step so nothing gets lost:

1. Select playbook (planning, implementation, or bug investigation)
2. Set up agent environments (worktrees, databases, ports per agent)
3. Spawn agents per playbook phase with context files
4. Monitor agents and review evidence (actual terminal output)
5. Merge agent branches one at a time, test after each
6. Run full test suite after all merges

## When to Use Teams

**Use teams for:**
- Complex epics with multiple features
- Work that benefits from parallel spec writing or review
- Implementation phases that need TDD Writer + Implementer separation

**Don't use teams for:**
- Simple features with 1-2 files
- Bug fixes (unless complex cross-service issues)
- Single-feature epics

Teams add coordination overhead. Solo work with the basic SDD skills is faster for simple tasks.

## Document Lifecycle Rules

Instead of restricting who edits what, restrict **when** edits are allowed:

| Stage | What Can Change | Rule |
|-------|----------------|------|
| **Planning** | Everything in `epics/` | Any agent can freely update. Iterate fast. |
| **Implementation** | Specs (api.md, database.md, ui/, tests.md) | Create ADR first, then update spec with `changed: YYYY-MM-DD — ADR-NNN` in frontmatter. |
| **Implementation** | Progress tracking (phases, CHECKLIST, EPIC, stories) | Always allowed — update statuses, check off tasks. |
| **Post-merge** | Epic docs | Historical. Don't modify. Update `services/` instead. |

**Never silently change a spec that someone is implementing against. Always leave a trail.**

## Non-Engineering Roles

Each role brings a different perspective. When spawning review agents, set their role with a specific prompt:

### Product Owner (PO)
**Perspective:** User value
**Reads:** EPIC.md, REQUIREMENTS.md, stories/*.md, specs/ui/
**Prompt:** "You are the Product Owner. Think from the user's perspective. Challenge whether we're building the right thing. Prioritize by user impact. Ask: 'Is this what users actually need?'"

### QA
**Perspective:** What can go wrong
**Reads:** specs/tests.md, specs/api.md, all specs, implementation code
**Prompt:** "You are QA. Think adversarially — your job is to break things. Find edge cases, boundary conditions, and error scenarios. Test what happens when things go wrong."

### Security
**Perspective:** What can be exploited
**Reads:** specs/api.md, specs/database.md, architecture.md, implementation code
**Prompt:** "You are a Security Researcher. Think like an attacker. Check every endpoint for auth/authz. Check for data over-exposure, injection vectors, insecure defaults. Reference OWASP top 10."

### User Tester
**Perspective:** Actual user experience
**Reads:** specs/ui/pages/*.md, specs/ui/flows.md, stories/*.md
**Prompt:** "You are a User Tester with no technical knowledge. Try to accomplish the task in the user story. Flag anything confusing, unclear, or requiring knowledge a normal user wouldn't have."

## Team Playbook: Planning Phase

Spawn agents with these roles and dependencies:

**Phase 1 — Requirements & Breakdown:**

| Order | Role | Task | Depends On |
|-------|------|------|-----------|
| 1 | PO | Validate requirements. Write stories with acceptance criteria. Challenge scope — what's MVP? | — |
| 2 | Planner | Break epic into features and phases. Create INDEX.md, phase plans. | PO |

**Phase 2 — Specs:**

| Order | Role | Task | Depends On |
|-------|------|------|-----------|
| 3 | Spec Writer | Write specs per feature: UI → API → Database | Planner |
| 4 | Test Planner | Write test plan from all specs | Spec Writer |

**Phase 3 — Spec Review Gate (parallel):**

| Order | Role | Task | Depends On |
|-------|------|------|-----------|
| 5a | PO | Review specs from user perspective | Test Planner |
| 5b | QA | Review test plan for gaps, add missing edge cases | Test Planner |
| 5c | Security | Review specs for vulnerabilities, check auth/authz | Test Planner |

PO, QA, and Security review in parallel. All findings addressed before implementation.

## Team Playbook: Implementation Phase

**Build (sequential):**

| Order | Role | Task | Depends On |
|-------|------|------|-----------|
| 1 | TDD Writer | Write failing tests for phase scope. Confirm RED. | — |
| 2 | Implementer | Write code to pass tests. Confirm GREEN. Refactor. | TDD Writer |
| 3 | Reviewer | Validate against specs. Check coverage. | Implementer |

**Pre-PR Review Gate (parallel):**

| Order | Role | Task | Depends On |
|-------|------|------|-----------|
| 4a | QA | Exploratory testing — try to break it beyond test plan | Reviewer |
| 4b | Security | OWASP top 10 review, check auth enforcement | Reviewer |
| 4c | User Tester | UX validation — can a user accomplish the task? | Reviewer |
| 5 | PO | Accept/reject incorporating all review findings | QA, Security, User Tester |

## Team Playbook: Bug Investigation

| Order | Role | Task | Depends On |
|-------|------|------|-----------|
| 1a | PO | Triage — assess user impact and priority | — |
| 1b | Security | Assess security implications — exploitable? data exposed? | — |
| 2 | Investigator | Reproduce bug, trace code path, document findings | PO, Security |
| 3 | Fixer | Write regression test (RED), implement fix (GREEN), run all tests | Investigator |
| 4 | Security | Verify fix doesn't introduce new vulnerabilities | Fixer |

## Context Loading for Agents

When spawning an agent, tell it which files to read:

| Agent Role | Must Read |
|-----------|----------|
| Planning | EPIC.md, REQUIREMENTS.md, stories/*.md, features/INDEX.md |
| Spec Writing | REQUIREMENTS.md, stories/{story}.md, architecture.md, existing specs |
| Implementation | architecture.md, specs/tests.md, specs/api.md, phases/phase{N}.md, PROJECT.md |
| Bug Investigation | INVESTIGATION.md, PROJECT.md, relevant source code |
| PO Review | EPIC.md, REQUIREMENTS.md, stories, specs/ui/ |
| QA Review | specs/tests.md, specs/api.md, all specs, implementation code |
| Security Review | specs/api.md, specs/database.md, architecture.md, implementation code |
| User Testing | specs/ui/pages/*.md, specs/ui/flows.md, stories/*.md |

## Agent Environment Isolation

**Agents on the same branch silently overwrite each other's files.** This is the most expensive coordination failure. Prevent it:

### Git Worktrees — One Per Agent

Every agent gets its own worktree. Never have two agents working on the same branch:

```bash
# Setup script pattern (create scripts/setup-agent-env.sh):
#!/bin/bash
AGENT_NUM=$1
BRANCH="agent-${AGENT_NUM}-$(date +%s)"
git worktree add "../project-agent-${AGENT_NUM}" -b "$BRANCH"
```

### Per-Agent Databases

Each agent needs its own database to prevent test data collisions:

```bash
createdb project_agent_${AGENT_NUM}
DATABASE_URL=postgres://localhost:5432/project_agent_${AGENT_NUM}
```

### Dynamic Port Assignment

Each agent's dev server on a different port:

```bash
PORT=$((3000 + AGENT_NUM))
PLAYWRIGHT_BASE_URL=http://localhost:${PORT}
```

### Pre-Spawn Checklist

Before spawning any agent:
- [ ] Worktree created (separate directory)
- [ ] Branch created (unique per agent)
- [ ] Database created (isolated)
- [ ] Port assigned (no conflicts)
- [ ] No other agents running on the same branch

### Merge Protocol

After agents complete work:
1. Agent commits to its branch
2. Orchestrator reviews each branch
3. Merge branches one at a time to main
4. Run full test suite after each merge
5. Resolve conflicts immediately

## Task Specificity

**"Fix E2E tests" is NOT a valid task.** Vague tasks waste agent budgets on exploration instead of execution.

Every agent task MUST include:

| Required Field | Why |
|---------------|-----|
| Exact file and line | Agent knows where to look |
| Root cause | Agent knows what's wrong |
| Exact fix | Agent knows what to change |
| Verification command | Agent can prove it worked |

### Examples

| Bad Task | Good Task |
|----------|-----------|
| "Fix E2E tests" | "In `ParticipationChart.tsx:42`, add `data-testid='participation-heading'` to the `<h3>`. This fixes `analytics.spec.ts:539` where `getByText('Participation')` matches both heading and privacy notice. Verify: `npx playwright test tests/e2e-browser/analytics.spec.ts --grep 'participation'`" |
| "Fix TypeScript errors" | "In `server.ts:128`, change `response.json()` return type from `any` to `BoardResponse`. Add interface `BoardResponse { id: string; name: string; phase: Phase }` in `types.ts`. Verify: `npx tsc --noEmit`" |
| "Backend API isn't working" | "In `routes/boards.ts:45`, change `app.put('/boards/:id')` to `app.patch('/boards/:id')`. Frontend sends PATCH but route handler uses PUT. Verify: `npx vitest tests/api/boards.test.ts`" |

### Task Template

```markdown
**File:** `{path}:{line}`
**Root cause:** {what's wrong and why}
**Fix:** {exactly what to change}
**Verify:** `{command to run}`
```

## Orchestrator Role

The orchestrator does NOT write code. The orchestrator's job is coordination, not implementation.

### Orchestrator Flow

```
1. Spawn diagnostic agent → runs tests, reports exact failures
2. Analyze failures → create surgical tasks (see Task Specificity)
3. Spawn fix agents → one per task, each in own worktree
4. Collect evidence → verify actual terminal output from each agent
5. Merge branches → one at a time, test after each
```

### What the Orchestrator Does

- Creates tasks with exact file:line, root cause, fix, verify command
- Assigns tasks to agents
- Reviews evidence (actual terminal output)
- Merges completed work
- Resolves conflicts between agent branches

### What the Orchestrator Does NOT Do

- Write production code
- Write tests
- Run tests directly
- Debug failures directly
- Accept "done" without evidence

**Why:** Context-switching between coordination and coding causes mistakes. The orchestrator who writes code starts making assumptions instead of verifying.

## Model Selection

Use the strongest available model for dev agents and PO. Weaker models cut corners:

| Role | Model Recommendation | Why |
|------|---------------------|-----|
| Dev Agent | Strongest available (Opus) | Writes correct code, follows TDD, runs tests |
| PO Agent | Strongest available (Opus) | Catches agent excuses, reads code to verify |
| Diagnostic Agent | Strongest available | Accurate failure analysis |
| Review Agent | Strong (Opus or Sonnet) | Pattern recognition, security review |

### Observed Failures with Weaker Models

| Behavior | Impact |
|----------|--------|
| Skips E2E tests, claims unit tests sufficient | Integration bugs ship |
| Uses `any` to fix TypeScript errors | Type safety lost |
| Modifies test assertions to match broken code | Bugs hidden, not fixed |
| Declares "blocked" instead of building | Work stalls unnecessarily |
| Pastes summaries instead of actual output | No real verification |
| Accepts "4 of 6 passing" as done | Broken features ship |

## Blocked Protocol

```
Is the code I need in THIS repository?
├── YES → Build it yourself. You are NOT blocked.
│         In a monolith, "backend not built yet" is YOUR job.
│
├── NO (different repo/service I can't access)
│   → Valid blocker. Document:
│     - What you need
│     - From which repo/service
│     - Who owns it
│     - What you've done so far
│
└── DESIGN UNCLEAR
    → Not blocked. Create ADR, propose design, ask PO.
```

**Things that are NEVER valid blockers in a monolith:**
- "Backend endpoint doesn't exist yet"
- "Database schema not created"
- "WebSocket handler not implemented"
- "Auth middleware not set up"
- "Types not defined yet"

**PO must verify any "blocked" claim** by reading the codebase. If the code is in this repo, reject the blocker.

## Communication Routing

Minimize coordination overhead by routing messages correctly:

| Communication Type | Channel | Why |
|-------------------|---------|-----|
| Task status updates | TaskList (update task status) | Visible to all, no context window cost |
| Completion with evidence | Message to orchestrator | Orchestrator needs to verify and merge |
| Blockers | Message to orchestrator | Orchestrator resolves blockers |
| Critical questions | Message to orchestrator | Needs coordination decision |
| PO feedback to dev | Direct message to dev agent | Avoids orchestrator as middleman |
| General status | Don't send | Unnecessary. TaskList shows status. |

**Anti-pattern:** Agent status messages flooding the orchestrator. Every message consumes context window. Only message when you need something or are delivering evidence.

## PO Challenge Rules

The PO must actively challenge agent claims. The PO is not a rubber stamp.

### Things PO Must NEVER Accept Without Investigation

| Claim | PO Response |
|-------|-------------|
| "Blocked on backend" | Read the codebase. Is the code in this repo? If yes, reject. |
| "Out of scope" | Check REQUIREMENTS.md and stories. Is it actually out of scope? |
| "Good enough" | Check acceptance criteria. Does it meet ALL of them? |
| "Works on my machine" | Demand E2E test output. No output = not verified. |
| "Tests pass" | Where's the terminal output? Paste it. |
| "Pre-existing issue" | Fix it or create a task. Not an excuse to ship broken code. |
| "Too complex to test" | If it's too complex to test, it's too complex. Simplify. |

### PO Verification Flow

```
1. Agent claims task is done
2. PO reads the task's verification command
3. PO checks: did agent paste actual terminal output?
   ├── NO → Reject. "Paste the actual test output."
   └── YES → PO reads the output
4. PO checks: do ALL tests pass?
   ├── NO → Reject. "N tests failing. Fix and re-run."
   └── YES → Continue
5. PO checks: tsc --noEmit output pasted? 0 errors?
   ├── NO → Reject. "Run tsc --noEmit and paste output."
   └── YES → Accept
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using teams for simple features | Solo + basic SDD skills is faster. Teams for complex epics only. |
| Agents silently changing specs during implementation | Enforce ADR-first rule. Check frontmatter for `changed:` dates. |
| Skipping the review gate | PO, QA, Security reviews catch different classes of bugs. Don't skip. |
| No context loading for agents | Agents without context produce generic output. Always specify files to read. |
| Running all reviews sequentially | PO, QA, Security, User Tester review in parallel — they're independent. |
| **Multiple agents on same branch** | **One worktree per agent. Never share branches.** |
| **Vague tasks ("fix E2E tests")** | **Every task needs: file:line, root cause, exact fix, verify command.** |
| **Agent claims "blocked" in a monolith** | **PO reads codebase to verify. If code is here, build it.** |
| **Orchestrator writing code** | **Orchestrator coordinates only. Spawn agents to write code.** |
| **Weak model for dev agents** | **Use strongest model. Weak models skip E2E, use `any`, modify tests.** |
| **Trusting claims without evidence** | **Demand actual terminal output. "Tests pass" without proof = not done.** |
| **Not logging agent failures** | **When an agent fails (false blocked, skipped E2E, modified tests), log a learning entry via `sdd:reflecting`. These accumulate and reveal systemic patterns in retrospectives.** |

## Integration

**Called by:**
- **sdd:workflow** — When user says "use team", "spawn agents", "coordinate"

**Required before (depends on playbook):**
- **sdd:creating-epics** — Epic structure MUST exist before Planning Phase playbook
- **sdd:writing-specs** — Specs MUST exist before Implementation Phase playbook

**Subagents should use:**
- **sdd:writing-specs** — Spec Writer and Test Planner agents follow this skill
- **sdd:implementing-with-tdd** — TDD Writer and Implementer agents follow this skill
- **sdd:investigating-bugs** — Investigator and Fixer agents follow this skill

**Pairs with:**
- **sdd:creating-epics** — For epic-level team structure and tracking
- **sdd:reflecting** — Log agent failures and coordination issues as learning entries. These get reviewed in retrospectives to improve the coordination process.
