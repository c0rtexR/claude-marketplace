# Iplooox Claude Code Marketplace

A [Claude Code](https://claude.ai/claude-code) plugin marketplace.

## Plugins

| Plugin | Description |
|--------|-------------|
| **sdd** | Skills-Driven Development — spec-first, TDD methodology for AI agent teams |

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

The SDD skill activates automatically when you ask Claude to create epics, plan features, implement with TDD, investigate bugs, or coordinate agent teams. You can also invoke it directly:

```
/sdd:workflow
```

## What is Skills-Driven Development?

A complete software development methodology for AI agent teams:

- **Spec-first**: Document UI → API → Database → Tests before writing code
- **TDD**: Tests before code, always (RED → GREEN → REFACTOR)
- **Epic lifecycle**: Structured epics with phases, stories, and checklists
- **Agent Teams**: Orchestrator coordinates planner, spec writer, implementer, and reviewer agents
- **Bug investigation**: Systematic investigation before jumping to fixes

## License

MIT
