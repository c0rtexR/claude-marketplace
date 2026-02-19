---
id: EXP-NNN
date: YYYY-MM-DD
question: "{The design question}"
status: # in-progress | decided | no-decision
decision: # ADR-NNN reference if decided
epic: # epic ID if applicable, or "standalone"
---

# EXP-NNN: {Short Title}

## Design Question

{What decision needs to be made? What are the constraints?}

## Design Space

### Moving Parts
{What components/systems are involved?}

### Fixed Constraints
{What can't change? Tech stack, existing contracts, performance requirements, etc.}

## Options

### Option A: {Name}

```
{ASCII diagram — component relationships, data flow}
```

**Optimizes for:** {what this approach prioritizes}

| Pros | Cons |
|------|------|
| {pro} | {con} |

### Option B: {Name}

```
{ASCII diagram}
```

**Optimizes for:** {what this approach prioritizes}

| Pros | Cons |
|------|------|
| {pro} | {con} |

### Option C: {Name} (if applicable)

```
{ASCII diagram}
```

**Optimizes for:** {what this approach prioritizes}

| Pros | Cons |
|------|------|
| {pro} | {con} |

## Trade-Off Matrix

| Dimension | Option A | Option B | Option C |
|-----------|----------|----------|----------|
| {relevant dimension 1} | {rating} | {rating} | {rating} |
| {relevant dimension 2} | {rating} | {rating} | {rating} |
| {relevant dimension 3} | {rating} | {rating} | {rating} |

## Recommendation

**Chosen:** {Option X}

**Why:** {Reasoning — what trade-offs are we accepting and why}

**Decision:** Formalized as {ADR-NNN} in `decisions/ADR-NNN-{slug}.md`
