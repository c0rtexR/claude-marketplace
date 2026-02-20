# Skills-Driven Development (SDD)

A spec-first, TDD methodology for AI agent teams. 12 specialized skills that guide Claude Code through every phase of software development — from brainstorming to retrospective.

**Version:** 2.6.0 | **License:** MIT

## Why SDD?

AI agents are powerful but undisciplined. Without structure, they skip tests, write code before specs, jump to fixes without investigation, and claim "done" without evidence. SDD solves this with rigid skills — each one a battle-tested process with checklists, gates, and templates that prevent real bugs.

Every skill was built from actual failures. The rules exist because skipping them caused expensive mistakes.

## Installation

```
/plugin marketplace add iploooox/claude-marketplace
/plugin install sdd@iplooox-marketplace
```

SDD activates automatically based on what you ask Claude to do. You can also invoke any skill directly:

```
/sdd:workflow
/sdd:brainstorming
/sdd:implementing-with-tdd
```

## The Workflow

```
New idea / "let's think about X"
    |
    v
 brainstorming -----> Explore ideas, clarify requirements,
    |                  design before implementation
    v
 writing-plans -----> Bite-sized implementation plan
    |                  with exact files, code, commands
    v
 creating-epics ----> Epic structure, folder hierarchy,
    |                  stories, requirements
    v
 spiking -----------> Prove feasibility before specs:
    |                  build POC or investigate existing code
    v
 designing ---------> Explore architecture options,
    |                  trade-offs, diagrams -> ADR
    v
 ui-designing ------> Visualization families -> variations ->
    |                  wireframes -> visual semantics -> UDR
    v
 component-research > Landscape scan -> gap analysis ->
    |                  build/reuse/extend -> innovation check
    v
 writing-specs -----> UI specs -> API specs -> DB specs ->
    |                  Test plan -> Phase plans
    v
 implementing ------> RED -> GREEN -> REFACTOR -> E2E ->
 with-tdd             Zero-tolerance gates -> PR
    |
    v
 investigating-bugs > Investigate -> Root cause -> Regression
                       test -> Fix -> Coding standards feedback

 coordinating ------> Overlay: use when ANY phase
 agent-teams          needs multi-agent coordination

 reflecting --------> Capture learnings, run retrospectives,
                       turn patterns into process improvements
```

Not every project needs every skill. Skip what doesn't apply — the workflow diagram shows decision points (technical unknowns? UI involved? custom components needed?) that gate each transition.

## Skills Reference

### Ideation & Planning

| Skill | What It Does | When to Use |
|-------|-------------|-------------|
| **brainstorming** | Collaborative design exploration. One question at a time, 2-3 approaches with trade-offs, incremental design approval. | "Let's think about X", "brainstorm", "explore idea" |
| **writing-plans** | Bite-sized implementation plans (2-5 min per step) with exact file paths, complete code, and test commands. | "Write plan", "break this down", "implementation plan" |
| **creating-epics** | Epic structure with folder hierarchy, stories, requirements, checklists. Also handles "what's next" and epic closure. | "New project", "create epic", "what's next", "close epic" |

### Research & Design

| Skill | What It Does | When to Use |
|-------|-------------|-------------|
| **spiking** | Prove technical feasibility before committing to specs. Two modes: build a POC (greenfield) or investigate existing code. | "Is this feasible?", "spike", "POC", "can we do X?" |
| **designing** | Explore architecture options with ASCII diagrams, trade-off matrices, and formalized ADRs. Minimum 2 options always. | "Design options", "how should we build X?", "architecture" |
| **ui-designing** | Deep UI/visualization R&D. Two-tier exploration: 4-6 visualization families, then 3-5 variations within top 2. Visual semantics, wireframes, UDR. | "How should we show this?", "visualize", "UI options" |
| **component-research** | Audit existing components. Landscape scan, gap analysis, 3-option decisions (reuse/extend/build) with explicit innovation check. | "Component audit", "build or reuse?", "what library?" |

### Specification

| Skill | What It Does | When to Use |
|-------|-------------|-------------|
| **writing-specs** | Full spec suite in order: UI specs, API specs, database specs, test plan, phase plans. Includes contract validation (forward + reverse checks). | "Write specs", "plan this", "design feature" |

### Implementation

| Skill | What It Does | When to Use |
|-------|-------------|-------------|
| **implementing-with-tdd** | RED-GREEN-REFACTOR cycle. Zero-tolerance gates (TypeScript, ESLint, all tests). E2E test required. Evidence-based completion (paste actual terminal output). | "Implement phase N", "start coding", "create PR" |
| **investigating-bugs** | Investigation-first discipline. Symptoms, reproduce, hypotheses, root cause, regression test, then fix. Feeds back to coding standards. | "Fix this bug", "debug", "there's an issue" |

### Coordination & Improvement

| Skill | What It Does | When to Use |
|-------|-------------|-------------|
| **coordinating-agent-teams** | Team playbooks for planning, implementation, and bug investigation. Agent isolation (worktrees, databases, ports). Task specificity rules. PO challenge protocol. | "Use team", "spawn agents", "coordinate" |
| **reflecting** | Self-improvement system. Capture learnings, detect patterns (3+ entries = systemic), run retrospectives, turn patterns into coding standards, protocol updates, or process changes. | "Retrospective", "log a learning", "reflect" |

## Key Concepts

### Every Skill Starts with a Task List

Every skill creates a task list before doing any work. This prevents steps from getting lost during long-running executions — especially important for multi-step skills like ui-designing (10 steps) or writing-specs (9 steps).

### Skills Auto-Chain

Skills tell you which skill to invoke next at transition points:

- `brainstorming` -> `writing-plans` -> `implementing-with-tdd`
- `creating-epics` -> `writing-specs` -> `implementing-with-tdd`
- `designing` -> `ui-designing` -> `component-research` -> `writing-specs`
- `investigating-bugs` -> regression test follows `implementing-with-tdd` RED-GREEN cycle
- `reflecting` improves rules that all other skills enforce

You don't need to memorize this. Each skill's Integration section has the directives.

### Rigid Skills

All SDD skills are **rigid** — follow them exactly. The checklists, gates, and templates exist because skipping them caused real bugs. Don't adapt away the discipline.

### Evidence Over Claims

"Tests pass" is not evidence. Actual terminal output is evidence. SDD enforces this at every level:

- Implementation requires pasting `npx tsc --noEmit` and test runner output
- PO agents must verify claims by reading code, not trusting summaries
- "Blocked" claims must be verified against the codebase

## Folder Structure

SDD creates a structured working area for every project:

```
project-root/
├── CLAUDE.md                        # Points agents to SDD skills
├── epics/
│   ├── INDEX.md                     # Epic catalog
│   └── {EPIC-ID}-{slug}/
│       ├── EPIC.md                  # Overview, phases, success criteria
│       ├── CHECKLIST.md             # Lifecycle tracking
│       ├── REQUIREMENTS.md          # Hard requirements
│       ├── decisions/               # ADRs, explorations, UDRs, CRRs
│       ├── spikes/                  # Technical feasibility investigations
│       ├── stories/                 # User stories
│       ├── bugs/                    # Bug investigations
│       └── features/                # Feature specs + phase plans
├── docs/
│   ├── coding-standards.md          # Bug-derived rules (CS-001, CS-002, ...)
│   ├── agent-protocol.md            # Agent verification contract
│   └── learnings/                   # Self-improvement system
│       ├── INDEX.md                 # Dashboard
│       ├── entries/                 # Individual learning entries
│       └── reviews/                 # Retrospective summaries
└── services/                        # Source of truth (update after merge only)
```

## Templates

Each skill ships with reference templates for every artifact it produces. Templates are in `skills/{skill-name}/references/` and are automatically used when the skill runs. 26 templates total across all skills.

## License

MIT
