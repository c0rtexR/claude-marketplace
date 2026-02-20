---
name: brainstorming
description: "You MUST use this before any creative work — creating features, building components, adding functionality, or modifying behavior. Explores user intent, requirements and design before implementation. Also triggers on: 'brainstorm', 'let's think about', 'explore idea', 'what should we build', 'help me design'."
---

# Brainstorming Ideas Into Designs

## Overview

Help turn ideas into fully formed designs and specs through natural collaborative dialogue.

Start by understanding the current project context, then ask questions one at a time to refine the idea. Once you understand what you're building, present the design and get user approval.

<HARD-GATE>
Do NOT invoke any implementation skill, write any code, scaffold any project, or take any implementation action until you have presented a design and the user has approved it. This applies to EVERY project regardless of perceived simplicity.
</HARD-GATE>

## Anti-Pattern: "This Is Too Simple To Need A Design"

Every project goes through this process. A todo list, a single-function utility, a config change — all of them. "Simple" projects are where unexamined assumptions cause the most wasted work. The design can be short (a few sentences for truly simple projects), but you MUST present it and get approval.

## Checklist

You MUST create a task for each of these items and complete them in order:

1. **Explore project context** — check files, docs, recent commits
2. **Ask clarifying questions** — one at a time, understand purpose/constraints/success criteria
3. **Propose 2-3 approaches** — with trade-offs and your recommendation
4. **Present design** — in sections scaled to their complexity, get user approval after each section
5. **Write design doc** — save to `docs/plans/YYYY-MM-DD-<topic>-design.md` and commit
6. **Transition to implementation** — invoke `sdd:writing-plans` to create implementation plan

## Process Flow

```
User has an idea
    │
    ▼
┌─────────────────────────┐
│ Explore project context  │ ◄── Check files, docs, recent commits
└───────────┬─────────────┘
            │
            ▼
┌─────────────────────────┐
│ Ask clarifying questions │ ◄── One at a time, multiple choice preferred
└───────────┬─────────────┘
            │
            ▼
┌─────────────────────────┐
│ Propose 2-3 approaches  │ ◄── With trade-offs and recommendation
└───────────┬─────────────┘
            │
            ▼
┌─────────────────────────┐
│ Present design sections  │◄─┐
└───────────┬─────────────┘  │
            │                 │
            ▼                 │
      ┌───────────┐          │
      │ Approved? │──── No ──┘
      └─────┬─────┘
            │ Yes
            ▼
┌─────────────────────────┐
│ Write design doc         │ ◄── docs/plans/YYYY-MM-DD-<topic>-design.md
└───────────┬─────────────┘
            │
            ▼
┌─────────────────────────┐
│ Invoke sdd:writing-plans │ ◄── Implementation plan
└─────────────────────────┘
```

**The terminal state is invoking `sdd:writing-plans`.** Do NOT invoke any other implementation skill. The ONLY skill you invoke after brainstorming is `sdd:writing-plans`.

## The Process

**Understanding the idea:**
- Check out the current project state first (files, docs, recent commits)
- Ask questions one at a time to refine the idea
- Prefer multiple choice questions when possible, but open-ended is fine too
- Only one question per message — if a topic needs more exploration, break it into multiple questions
- Focus on understanding: purpose, constraints, success criteria

**Exploring approaches:**
- Propose 2-3 different approaches with trade-offs
- Present options conversationally with your recommendation and reasoning
- Lead with your recommended option and explain why

**Presenting the design:**
- Once you believe you understand what you're building, present the design
- Scale each section to its complexity: a few sentences if straightforward, up to 200-300 words if nuanced
- Ask after each section whether it looks right so far
- Cover: architecture, components, data flow, error handling, testing
- Be ready to go back and clarify if something doesn't make sense

## After the Design

**Documentation:**
- Write the validated design to `docs/plans/YYYY-MM-DD-<topic>-design.md`
- Commit the design document to git

**Implementation:**
- Invoke `sdd:writing-plans` to create a detailed implementation plan
- Do NOT invoke any other skill. `sdd:writing-plans` is the next step.

## Key Principles

- **One question at a time** — Don't overwhelm with multiple questions
- **Multiple choice preferred** — Easier to answer than open-ended when possible
- **YAGNI ruthlessly** — Remove unnecessary features from all designs
- **Explore alternatives** — Always propose 2-3 approaches before settling
- **Incremental validation** — Present design, get approval before moving on
- **Be flexible** — Go back and clarify when something doesn't make sense

## Red Flags

| Thought | Reality |
|---------|---------|
| "This is too simple to need a design" | Simple projects are where unexamined assumptions cause the most wasted work. |
| "I already know what they want" | You probably don't. Ask. |
| "Let me just start coding" | Design approval first. Always. |
| "One approach is obvious" | Propose 2-3 anyway. The "obvious" one might not be best. |
| "I'll figure it out as I go" | That's how rework happens. Design first. |

## Integration

**Called by:**
- **sdd:workflow** — When user says "brainstorm", "explore idea", "let's think about", "what should we build"
- Standalone — any time creative work needs design exploration before implementation

**MUST invoke after:**
- **sdd:writing-plans** — REQUIRED after design approval. Creates implementation plan from design.

**Pairs with:**
- **sdd:creating-epics** — Brainstorming can feed into epic creation for larger projects
- **sdd:designing** — If the brainstorm surfaces architectural questions, invoke designing for deep exploration
