# {Project Name}

## Development Workflow

This project uses **Skills-Driven Development** (SDD). Load the SDD skill for all development workflows: epic creation, planning, implementation, bug investigation, and team coordination.

## Coding Standards

Read `docs/coding-standards.md` before writing any code. These rules are derived from real bugs and are non-negotiable.

## Agent Protocol

Every dev agent MUST read `docs/agent-protocol.md` for the verification contract: Definition of Done, TDD rules, zero-tolerance gates, E2E requirements, and evidence format.

## Project Layout

- `epics/` — Active work: specs, plans, decisions, bugs
- `services/` — Source of truth (update after merge only)
- `docs/` — Project documentation (coding standards, agent protocol)

## Active Epics

See `epics/INDEX.md` for current status.

## Bug Logging

When you find a bug during implementation or testing, log it in `bugs/{BUG-ID}/` following the structure in `bugs/README.md`. Map it to a coding standard category in `docs/coding-standards.md`. If it's a new class of bug, add a new rule.
