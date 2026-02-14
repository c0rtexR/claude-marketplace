---
feature: "{feature-name}"
status: draft
---

# {Feature Name} — Architecture

## Current State

{How the relevant parts of the system work today. Load from services/{service}/PROJECT.md and codebase exploration.}

```
{ASCII diagram of current architecture — affected components and data flow}
```

| Component | Current Behavior | Location |
|-----------|-----------------|----------|
| {component} | {what it does today} | `path/to/code` |

## Target State

{What the system should look like after this feature is implemented.}

```
{ASCII diagram of target architecture — new/changed components highlighted}
```

| Component | Target Behavior | New/Changed |
|-----------|----------------|-------------|
| {component} | {what it should do} | New / Changed / Unchanged |

## Changes Required

| Area | Current | Target | Type |
|------|---------|--------|------|
| {component/layer} | {how it works now} | {how it should work} | Add / Modify / Remove |

## Key Design Decisions

| Decision | Choice | Why |
|----------|--------|-----|
| {decision} | {choice} | {rationale — or link to ADR} |

## Risks

| Risk | Impact | Mitigation |
|------|--------|-----------|
| {risk} | {what breaks if this goes wrong} | {how to prevent/detect} |
