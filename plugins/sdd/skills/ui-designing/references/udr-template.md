---
id: UDR-NNN
date: YYYY-MM-DD
problem: "{What data/information needs to be visualized}"
user_task: "{What the user is trying to accomplish}"
status: # in-progress | decided | no-decision
justification_depth: # practical | principles | full
chosen_family: # comparative | temporal | compositional | spatial | relational | distributional | hierarchical | status-dashboard | narrative | interactive
chosen_variation: "{Name of chosen variation}"
epic: # epic ID if applicable, or "standalone"
---

# UDR-NNN: {Short Title}

## Visualization Problem

### Data Description
{What data is being visualized? What are its dimensions, cardinality, update frequency?}

### User Task
{What is the user trying to DO with this data? Compare? Track over time? Find anomalies? Make a decision?}

### Constraints
{Screen size, accessibility requirements, data volume, real-time updates, existing design system, etc.}

## Justification Depth

{Selected by user at start: practical | principles + practical | full rationale}

---

## Tier 1: Visualization Families

{4-6 families relevant to this data type and user task. Not all families apply.}

### Family: {Name}

```
{ASCII wireframe showing the shape of this family applied to the data}
```

**Best for:** {what user task this family optimizes for}
**Score vs user task:** {high / medium / low}

### Family: {Name}

```
{ASCII wireframe}
```

**Best for:** {what user task this family optimizes for}
**Score vs user task:** {high / medium / low}

{Repeat for 4-6 families}

### Tier 1 Decision

**Top 2 families:** {Family A} and {Family B}
**Reasoning:** {why these two score highest for the user task}

---

## Tier 2: Variations

### Family: {Winning Family A}

#### Variation A1: {Name}

```
{ASCII wireframe — detailed layout with component positions}
```

**Visual Semantics:**
- Color-coding: {what colors mean}
- Emphasis: {what draws attention first}
- Density: {information density level}
- Symbols: {icon/symbol language}

**Optimizes for:** {what this variation prioritizes}

| Pros | Cons |
|------|------|
| {pro} | {con} |

#### Variation A2: {Name}

```
{ASCII wireframe}
```

**Visual Semantics:**
- Color-coding: {what colors mean}
- Emphasis: {what draws attention first}
- Density: {information density level}
- Symbols: {icon/symbol language}

**Optimizes for:** {what this variation prioritizes}

| Pros | Cons |
|------|------|
| {pro} | {con} |

{Repeat for 3-5 variations per winning family}

### Family: {Winning Family B}

{Same structure — 3-5 variations}

---

## Trade-Off Matrix

| Dimension | {Var A1} | {Var A2} | {Var A3} | {Var B1} | {Var B2} |
|-----------|----------|----------|----------|----------|----------|
| {relevant dimension 1} | {rating} | {rating} | {rating} | {rating} | {rating} |
| {relevant dimension 2} | {rating} | {rating} | {rating} | {rating} | {rating} |
| {relevant dimension 3} | {rating} | {rating} | {rating} | {rating} | {rating} |

## Recommendation

**Chosen:** {Variation name}

**Reasoning:**
{At the chosen justification depth — practical / principles + practical / full rationale}

**Visual Semantics Summary:**
- Color-coding: {final color-coding rules}
- Emphasis: {final emphasis patterns}
- Density: {final density decision}
- Symbols: {final icon/symbol language}

**Components Identified:**
{List of UI components this visualization will need — feeds into component-research}

| Component | Purpose | Custom? |
|-----------|---------|---------|
| {component} | {what it does} | {yes/no/maybe} |
