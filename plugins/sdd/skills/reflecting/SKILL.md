---
name: reflecting
description: "Use for capturing learnings, running retrospectives, and turning patterns into process improvements. Makes SDD self-improving. Also triggers on: 'retrospective', 'review learnings', 'what have we learned', 'self-improve', 'reflect', 'log a learning'."
---

# Reflecting

## Overview

Capture learnings as they happen. Detect patterns over time. Turn patterns into concrete process changes. The methodology gets better with every project.

**Core principle:** Every expensive mistake is a learning. If it happens twice, it's a pattern. If it's a pattern, it needs a rule.

## When to Use

- User says "retrospective", "review learnings", "what did we learn", "reflect"
- User says "log a learning", "I noticed a pattern", "we keep doing X"
- Epic is being closed (REQUIRED — part of closure checklist)
- 5+ unreviewed entries accumulate in `docs/learnings/INDEX.md`
- Phase completion (SUGGESTED — quick scan)

## Step 0: Create Task List

**Before doing anything else**, create a task for each step so nothing gets lost:

**Capture mode:**
1. Determine category
2. Determine severity
3. Create entry file
4. Cross-reference with existing entries

**Review mode:**
1. Gather (read INDEX, unreviewed entries, current rules)
2. Detect patterns (group by category, find shared root causes)
3. Propose actions (one per pattern)
4. Write review document
5. Present to user and execute approved actions

## Two Modes

| Mode | Trigger | What Happens |
|------|---------|--------------|
| **Capture** | "log a learning", bug found, agent failure, time sink | Create a single entry in `docs/learnings/entries/` |
| **Review** | "retrospective", epic closure, 5+ unreviewed entries | Analyze all unreviewed entries, detect patterns, propose actions |

---

## Mode 1: Capture a Learning

Quick, low friction. Should take <2 minutes.

### Step 1: Determine the Category

| Category | What It Captures | Example |
|----------|-----------------|---------|
| `bug-pattern` | Recurring bug class (beyond individual CS rules) | Same API contract mismatch 3 times |
| `time-sink` | Went in circles, wasted significant time | Debugging for 1hr, was env config |
| `agent-failure` | Agent cut corners, wrong approach, false blocked | Agent modified test assertions |
| `architecture` | Wrong abstraction, missing pattern, tech debt | Premature optimization slowed us |
| `process-gap` | Skipped step, wrong order, missing gate | Coded before specs, had to redo |
| `coordination` | Miscommunication, wrong routing, context loss | Agents overwrote each other's files |
| `environment` | Setup issues, CI gaps, config drift | DB connection string wrong in CI |
| `success` | Something worked really well — repeat it | Surgical tasks completed in minutes |

### Step 2: Determine Severity

| Severity | Criteria |
|----------|----------|
| **high** | >1 hour wasted, or caused a shipped bug, or blocked the team |
| **medium** | 15-60 min wasted, or required rework |
| **low** | Minor friction, good to know for next time |

### Step 3: Create the Entry

1. Read `docs/learnings/INDEX.md` to get the next `LRNG-NNN` number
2. Create `docs/learnings/entries/LRNG-NNN-{slug}.md` using `references/learning-entry-template.md`
3. Fill in: what happened, impact, root cause
4. Update `docs/learnings/INDEX.md` — add to "Unreviewed Entries" and increment total count

### Step 4: Cross-Reference

Check: are there other entries with the same category or root cause?
- If 3+ entries share a category → note in the Pattern field: "3 entries in {category} → systemic"
- If this matches an existing pattern → link to it

**Done.** Entry captured. It will be reviewed in the next retrospective.

---

## Mode 2: Run a Retrospective

Thorough analysis. Produces concrete actions.

### Step 1: Gather

Read these files:

| File | Why |
|------|-----|
| `docs/learnings/INDEX.md` | Dashboard — see what's unreviewed |
| All entries with `status: new` | The raw learnings to analyze |
| `docs/coding-standards.md` | Current code rules — check for gaps |
| `docs/agent-protocol.md` | Current agent contract — check for gaps |
| `CLAUDE.md` | Current workflow — check for gaps |

### Step 2: Detect Patterns

Group unreviewed entries by category. For each group:

1. **Count:** How many entries in this category?
2. **Trend:** Increasing, stable, or decreasing vs. previous review?
3. **Root cause overlap:** Do multiple entries share the same underlying cause?
4. **Existing rules:** Should an existing CS rule or protocol rule have caught this?
5. **Systemic?** 3+ entries in a category = systemic pattern requiring action

Present findings as a table:

```markdown
| Category | Count | Trend | Shared Root Cause | Existing Rule? | Systemic? |
|----------|-------|-------|-------------------|---------------|-----------|
```

### Step 3: Propose Actions

For each pattern detected, propose exactly ONE of these action types:

| Action Type | When | Applied To |
|-------------|------|-----------|
| New coding standard | Bug/code pattern not covered by existing rules | `docs/coding-standards.md` |
| Protocol update | Agent behavior pattern (cutting corners, false blocked) | `docs/agent-protocol.md` |
| Process change | Workflow gap (skipped step, wrong order) | `CLAUDE.md` or skill files |
| Skill update | Skill missing a step or gate | `skills/{skill}/SKILL.md` |
| No action | One-off incident, not recurring | Document why explicitly |

**Every pattern gets an explicit action or an explicit "no action" with reasoning.** No silent skips.

### Step 4: Write the Review

1. Create `docs/learnings/reviews/YYYY-MM-DD-{slug}.md` using `references/review-template.md`
2. Fill in: entries reviewed, patterns detected, proposed actions
3. Update each reviewed entry: `status: new` → `status: reviewed`
4. Update `docs/learnings/INDEX.md`:
   - Move entries from "Unreviewed" to reviewed
   - Add row to "Review Log"
   - Update "Pattern Summary" table
   - Update metrics

### Step 5: Present and Execute

Present the review to the user:
1. Show the patterns detected
2. Show the proposed actions
3. Ask for approval on each action

For each approved action:
1. Apply the change to the target file
2. Log what changed in the review file under "Actions Applied"
3. Update the learning entry: `action_taken: {description}`

For rejected actions:
1. Log the rejection reason in the review file
2. Update the learning entry: `action_taken: rejected — {reason}`

---

## Folder Structure

```
docs/
├── coding-standards.md          # Code rules (existing)
├── agent-protocol.md            # Agent contract (existing)
└── learnings/
    ├── INDEX.md                 # Dashboard: patterns, unreviewed, metrics
    ├── entries/
    │   ├── LRNG-001-{slug}.md
    │   ├── LRNG-002-{slug}.md
    │   └── ...
    └── reviews/
        ├── YYYY-MM-DD-{slug}.md
        └── ...
```

When setting up for the first time, create the `docs/learnings/` folder structure and initialize `INDEX.md` using `references/learnings-index-template.md`.

---

## Automatic Triggers from Other Skills

These skills should prompt for learning capture at specific moments:

| Skill | When | Category |
|-------|------|----------|
| `sdd:investigating-bugs` | After Step 7 (coding standards feedback) | `bug-pattern` |
| `sdd:coordinating-agent-teams` | Agent failure detected | `agent-failure` or `coordination` |
| `sdd:implementing-with-tdd` | Phase took >2x estimated time | `time-sink` |
| `sdd:creating-epics` | Epic closure | Prompt: "Any learnings to capture?" |

You don't need to enforce these triggers — the other skills reference this skill at the right moments.

---

## Quick Reference

| Phase | What You Do | Output |
|-------|------------|--------|
| Capture | Categorize → Create entry → Cross-reference | New entry in `docs/learnings/entries/` |
| Gather | Read INDEX, unreviewed entries, current rules | Context for analysis |
| Detect | Group by category, find shared root causes, check 3+ threshold | Pattern table |
| Propose | One action per pattern: CS rule, protocol update, process change, skill update, or explicit no-action | Action proposals |
| Write | Create review file, update entry statuses, update INDEX | Review in `docs/learnings/reviews/` |
| Execute | Apply approved actions, log rejections | Updated rules/protocols/skills |

## Red Flags

| Thought | Reality |
|---------|---------|
| "This isn't worth logging" | If it cost >15 min or will happen again, log it |
| "We'll remember this" | You won't. Write it down. |
| "It's just a one-off" | Log it anyway. The third "one-off" reveals the pattern. |
| "The retro can wait" | 5+ unreviewed entries = patterns going undetected |
| "No action needed" | Fine — but document WHY no action. Silent skips hide patterns. |
| "Let's just add a rule" | Rules without pattern evidence are noise. Show the entries. |

## Integration

**Called by:**
- **sdd:workflow** — When user says "retrospective", "review learnings", "reflect"
- **sdd:creating-epics** — REQUIRED during epic closure

**Triggered from:**
- **sdd:investigating-bugs** — Step 7 prompts for learning entry on broader patterns
- **sdd:coordinating-agent-teams** — Agent failures prompt for learning entry
- **sdd:implementing-with-tdd** — Time sinks prompt for learning entry

**Pairs with:**
- **sdd:creating-epics** — Retrospective is part of epic closure checklist
- All skills — reflecting improves the rules that all skills enforce
