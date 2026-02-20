# Iplooox Claude Code Marketplace

A [Claude Code](https://claude.ai/claude-code) plugin marketplace.

## Plugins

| Plugin | Version | Description |
|--------|---------|-------------|
| **[sdd](plugins/sdd/)** | 2.6.0 | Skills-Driven Development — spec-first, TDD methodology with 12 skills for AI agent teams |

## Installation

### 1. Add the marketplace

```
/plugin marketplace add iploooox/claude-marketplace
```

### 2. Install a plugin

```
/plugin install sdd@iplooox-marketplace
```

### 3. Use it

SDD activates automatically when you ask Claude to brainstorm, plan, implement, debug, or coordinate. You can also invoke any skill directly:

```
/sdd:workflow
/sdd:brainstorming
/sdd:implementing-with-tdd
```

## Skills-Driven Development (SDD)

A complete software development methodology for AI agent teams. 12 skills covering every phase from idea to retrospective.

### Skills at a Glance

**Ideation & Planning**

| Skill | Description |
|-------|-------------|
| `sdd:brainstorming` | Collaborative design exploration — clarify requirements, propose 2-3 approaches, get design approval |
| `sdd:writing-plans` | Bite-sized implementation plans with exact files, code, and test commands |
| `sdd:creating-epics` | Epic structure, folder hierarchy, stories, requirements, lifecycle management |

**Research & Design**

| Skill | Description |
|-------|-------------|
| `sdd:spiking` | Prove technical feasibility — build a POC or investigate existing code |
| `sdd:designing` | Architecture exploration with ASCII diagrams, trade-off matrices, ADRs |
| `sdd:ui-designing` | Visualization R&D — 4-6 families, drill into variations, visual semantics |
| `sdd:component-research` | Component audit — landscape scan, gap analysis, build/reuse/extend decisions |

**Specification & Implementation**

| Skill | Description |
|-------|-------------|
| `sdd:writing-specs` | UI -> API -> Database -> Test plan -> Phase plans, with contract validation |
| `sdd:implementing-with-tdd` | RED-GREEN-REFACTOR with zero-tolerance gates and evidence-based completion |
| `sdd:investigating-bugs` | Investigation-first debugging with mandatory regression tests |

**Coordination & Improvement**

| Skill | Description |
|-------|-------------|
| `sdd:coordinating-agent-teams` | Team playbooks, agent isolation, task specificity, PO challenge rules |
| `sdd:reflecting` | Capture learnings, detect patterns, run retrospectives, improve the process |

### Core Principles

- **Spec-first** — document UI, API, database, and tests before writing code
- **TDD** — tests before code, always (RED -> GREEN -> REFACTOR)
- **Evidence over claims** — paste actual terminal output, not "tests pass"
- **Task lists** — every skill creates a task list before doing any work
- **Self-improving** — learnings from mistakes become coding standards and process rules
- **Skills auto-chain** — each skill tells you which skill comes next

See the full [SDD documentation](plugins/sdd/README.md) for details.

## License

MIT
