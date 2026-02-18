---
name: workflow
description: "Skills-Driven Development — a complete spec-first, TDD methodology for AI agent teams. Use this skill whenever the user wants to create an epic, plan features, implement with TDD, investigate bugs, or coordinate agent teams for software development. Also triggers on: 'new project', 'start feature', 'plan epic', 'implement phase', 'investigate bug', 'what's next', 'create PR', 'close epic'."
---

<EXTREMELY-IMPORTANT>
SDD is a methodology with 6 specialized skills. You MUST invoke the correct sub-skill for the task at hand. Do NOT try to wing it from memory — invoke the skill so you get the current version.

This is not optional. Each skill contains checklists, templates, and process gates that prevent real bugs.
</EXTREMELY-IMPORTANT>

# Skills-Driven Development (SDD)

## The Workflow

```
New project / "let's build X"
    │
    ▼
┌─────────────────┐
│ sdd:creating-    │ ◄── Create epic structure, interview user,
│ epics            │     set up folder hierarchy, CLAUDE.md
└────────┬────────┘
         │ Features identified
         ▼
┌─────────────────┐
│ sdd:writing-     │ ◄── UI specs → API specs → DB specs →
│ specs            │     Test plan → Phase plans
└────────┬────────┘
         │ Specs complete, phases defined
         ▼
┌─────────────────┐
│ sdd:implementing-│ ◄── RED → GREEN → REFACTOR → E2E →
│ with-tdd         │     Zero-tolerance gates → PR
└────────┬────────┘
         │ Bug found during implementation
         ▼
┌─────────────────┐
│ sdd:investigating│ ◄── Investigate → Root cause → Regression
│ -bugs            │     test → Fix → Coding standards feedback
└─────────────────┘

         ┌─────────────────┐
         │ sdd:coordinating│ ◄── Overlay: use when ANY phase
         │ -agent-teams    │     needs multi-agent coordination
         └─────────────────┘

         ┌─────────────────┐
         │ sdd:reflecting   │ ◄── Capture learnings, run retrospectives,
         │                  │     turn patterns into process improvements
         └─────────────────┘
```

## Route to the Right Skill

Match the user's intent and **invoke the skill immediately:**

| User Says | Invoke | Why |
|-----------|--------|-----|
| "New project", "let's build X", "create epic" | **sdd:creating-epics** | Start with structure |
| "Plan this", "write specs", "design feature" | **sdd:writing-specs** | Specs before code |
| "Implement phase N", "start coding", "build this" | **sdd:implementing-with-tdd** | TDD discipline |
| "Create PR", "complete phase" | **sdd:implementing-with-tdd** | PR is part of implementation |
| "Fix this bug", "there's an issue", "debug" | **sdd:investigating-bugs** | Investigation first |
| "What's next", "resume work" | **sdd:creating-epics** | Check epic status |
| "Close epic", "wrap up" | **sdd:creating-epics** | Closure process |
| "Use team", "spawn agents", "coordinate" | **sdd:coordinating-agent-teams** | Team playbooks |
| "Retrospective", "review learnings", "reflect" | **sdd:reflecting** | Self-improvement |
| "Log a learning", "I noticed a pattern" | **sdd:reflecting** | Capture learning |
| "Write tests" (no specs exist) | **sdd:writing-specs** | Test plan comes from specs |
| "Write tests" (specs exist) | **sdd:implementing-with-tdd** | TDD cycle |

## How to Invoke

Use the `Skill` tool:

```
Skill: sdd:creating-epics
Skill: sdd:writing-specs
Skill: sdd:implementing-with-tdd
Skill: sdd:investigating-bugs
Skill: sdd:coordinating-agent-teams
Skill: sdd:reflecting
```

**Announce it:** "Using sdd:{skill-name} to {what you're doing}."

## Skill Types

All SDD skills are **rigid** — follow them exactly. Don't adapt away the discipline. The checklists, gates, and templates exist because skipping them caused real bugs.

## Skill Chain

Skills auto-chain at transition points. Each skill tells you which skill to invoke next:

- `creating-epics` → after Step 4 → **MUST invoke** `sdd:writing-specs`
- `writing-specs` → after Step 8 → **MUST invoke** `sdd:implementing-with-tdd`
- `implementing-with-tdd` → TDD Checkpoint fails → **MUST invoke** `sdd:writing-specs`
- `implementing-with-tdd` → bug found → **MUST invoke** `sdd:investigating-bugs`
- `investigating-bugs` → Step 5 regression test → follows `sdd:implementing-with-tdd` RED→GREEN cycle
- `coordinating-agent-teams` → tell subagents to use the relevant skill for their role
- `reflecting` → captures learnings from any skill; retrospective improves all skills' rules

**You don't need to remember this.** Each skill's Integration section has the directives.

## Red Flags

| Thought | Reality |
|---------|---------|
| "I know the SDD methodology" | Skills evolve. Invoke the current version. |
| "This is too simple for a skill" | Simple features have complex integration cracks. Use the skill. |
| "Let me just write the code" | Code without specs → integration bugs. Invoke `sdd:writing-specs` first. |
| "I'll add tests after" | RED before GREEN. Invoke `sdd:implementing-with-tdd`. |
| "I'll just fix this quickly" | Investigation first. Invoke `sdd:investigating-bugs`. |
| "I don't need a team for this" | Probably true. But if you do, invoke `sdd:coordinating-agent-teams`. |
