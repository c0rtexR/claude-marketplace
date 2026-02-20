---
name: ui-designing
description: "Use when deciding how to visualize data or design UI layouts — exploring visualization families, comparing wireframe variations, and choosing the best approach with deep reasoning. Also triggers on: 'how should we show this data?', 'visualize', 'UI options', 'wireframe options', 'visualization', 'chart options', 'dashboard layout'."
---

# UI Designing

## Overview

Deep R&D into how to visualize data or design a UI screen. Explores multiple visualization families, drills into variations within the best families, and documents reasoning at the user's chosen depth.

**Core principle:** There are dozens of valid ways to visualize the same data. Explore broadly before committing. A bar chart, heatmap, card grid, timeline, or infographic each tells a different story.

## When to Use

- "How should we show this data?"
- "What's the best way to visualize sprint velocity?"
- "Dashboard layout options?"
- "Should this be a chart or a table or cards?"
- After architecture decisions (phased) — the visualization approach is unclear
- Standalone — facing a UI design fork at any point

## Step 1: Create Task List

**Before doing anything else**, create a task for each step so nothing gets lost:

1. Frame the visualization problem
2. Ask justification depth
3. Explore visualization families (Tier 1)
4. User picks top 2 families
5. Deep-dive variations (Tier 2)
6. Trade-off matrix
7. Recommend with reasoning
8. Document as UDR
9. Transition gate

## Step 2: Frame the Visualization Problem

What data needs to be visualized? What is the user trying to DO?

Good: "Sprint velocity per team over the last 6 sprints — users need to compare teams and spot trends"
Bad: "Design a dashboard"

Document in the UDR:
- **Data description** — dimensions, cardinality, update frequency
- **User task** — compare? track over time? find anomalies? make a decision?
- **Constraints** — screen size, accessibility, data volume, real-time, existing design system

## Step 3: Ask Justification Depth

Ask the user via AskUserQuestion:

**"How deep should the design reasoning go?"**

| Option | Description |
|--------|-------------|
| **Practical only** | Justify by user task: "users compare values → bar chart beats pie chart for comparison" |
| **Principles + practical** (Recommended) | Reference design principles when they add insight (Gestalt proximity, data-ink ratio) but ground in practical task |
| **Full rationale** | Every choice gets design principles, cognitive science, and data visualization best practices |

Use the chosen depth for all reasoning in Steps 6-7.

## Step 4: Explore Visualization Families (Tier 1)

Generate 4-6 families relevant to this data type and user task. **Not all families apply** — pick only those that make sense for the data.

### Available Families

| Family | Best For | Example Variations |
|--------|----------|-------------------|
| **Comparative** | Comparing values across categories | Bar, column, bullet, lollipop, radar |
| **Temporal** | Showing change over time | Line, area, sparkline, timeline, Gantt |
| **Compositional** | Parts of a whole | Stacked bar, treemap, sunburst, waffle, donut |
| **Spatial** | Geographic or layout-based | Heatmap, floor plan, node map, choropleth |
| **Relational** | Connections between entities | Network graph, chord diagram, Sankey, arc |
| **Distributional** | Spread and density of values | Histogram, box plot, violin, scatter, bee swarm |
| **Hierarchical** | Nested groupings | Tree, org chart, nested cards, accordion, indented list |
| **Status/Dashboard** | KPI overview, at-a-glance health | Card grid, gauge, traffic light, scorecard |
| **Narrative** | Guided storytelling through data | Infographic, step flow, annotated chart |
| **Interactive** | User-driven exploration | Filter panel, drill-down, pivot table, faceted search |

Each family gets an ASCII wireframe showing the *shape* of how the data would appear:

```
Family: Comparative — Grouped Bar

┌─────────────────────────────────┐
│  Sprint Velocity by Team        │
│                                 │
│  ████  ░░░░                     │
│  ████  ░░░░  ████               │
│  ████  ░░░░  ████  ░░░░         │
│  ████  ░░░░  ████  ░░░░  ████   │
│  ─────┼──────┼──────┼──────     │
│  Team A  Team B  Team C  Team D │
│                                 │
│  ████ Planned   ░░░░ Actual     │
└─────────────────────────────────┘

Family: Status/Dashboard — Scorecard Grid

┌──────────┐ ┌──────────┐ ┌──────────┐
│ Team A   │ │ Team B   │ │ Team C   │
│ ▲ 42pts  │ │ ▼ 28pts  │ │ → 35pts  │
│ +12%     │ │ -8%      │ │ 0%       │
│ ●●●●○    │ │ ●●○○○    │ │ ●●●○○    │
└──────────┘ └──────────┘ └──────────┘
```

Score each family against the user task (high / medium / low).

## Step 5: User Picks Top 2 Families

Present all families with wireframes and scores. Ask the user which 2 to explore deeper. Use AskUserQuestion if the user doesn't have a strong preference.

## Step 6: Deep-Dive Variations (Tier 2)

For each winning family, explore 3-5 variations. Each variation gets:

1. **ASCII wireframe** — detailed layout with component positions
2. **Visual semantics** — color-coding rules, emphasis patterns, density, icon/symbol language
3. **Pros and cons** table
4. **What it optimizes for**

## Step 7: Trade-Off Matrix

Compare all variations (from both families) across dimensions **relevant to THIS data and user task**. Don't use generic dimensions.

Example dimensions:
- Scannability (how quickly can users find what they need?)
- Data density (how much info fits without clutter?)
- Comparison support (can users compare values easily?)
- Trend visibility (are changes over time obvious?)
- Accessibility (works for color-blind users? screen readers?)
- Mobile-friendliness (works on small screens?)

## Step 8: Recommend with Reasoning

Present your recommendation at the chosen justification depth. Use the AskUserQuestion pattern:
- 3 options (top variations) with your recommendation marked
- Reasoning at the depth level chosen in Step 3
- User picks or requests revisions

## Step 9: Document as UI Design Record (UDR)

Use `references/udr-template.md`. Store in `decisions/explorations/UDR-NNN-{slug}.md`.

The UDR is a permanent artifact. Future developers should understand WHY this visualization approach was chosen, what alternatives were considered, and what trade-offs were accepted.

## Step 10: Transition Gate

Check context: does the chosen visualization need custom components?

```
Components identified in UDR?
├── YES, some marked "custom" or "maybe"
│   → Suggest: "Invoke sdd:component-research to audit component options"
│
├── YES, all standard/existing
│   → Suggest: "Invoke sdd:writing-specs to document the UI specs"
│
└── NO components identified
    → Ask: "Do you need to research component options, or proceed to specs?"
```

---

## Folder Structure

**Epic-scoped:**
```
epics/{EPIC-ID}-{slug}/decisions/
├── INDEX.md
├── explorations/
│   ├── EXP-001-{slug}.md          # Architecture explorations
│   └── UDR-001-{slug}.md          # UI Design Records
```

**Standalone:**
```
decisions/
├── INDEX.md
├── explorations/
│   └── UDR-001-{slug}.md
```

Use `references/udr-template.md` for each UI Design Record.

## Key Disciplines

- **Always explore at least 4 families in Tier 1.** Fewer means you're not looking broadly enough.
- **Every family and variation gets an ASCII wireframe.** If you can't draw it, you haven't thought it through.
- **Visual semantics are not optional.** Color-coding, emphasis, density, and symbols are part of the design, not decoration.
- **The user task drives everything.** "Users need to compare" → comparative families score high. Don't pick families because they look cool.
- **"No good options" is valid.** If none of the variations serve the user task well, document why and rethink the data model.
- **Task list at the start, always.** Track every step. Nothing gets lost.

## Quick Reference

| Phase | What You Do | Output |
|-------|------------|--------|
| Task list | Create tasks for all steps | Tracked progress |
| Frame | Data, user task, constraints | Problem documented |
| Depth | Ask user: practical / principles / full | Justification level set |
| Tier 1 | 4-6 families with wireframes, scored | Families explored |
| Pick | User selects top 2 families | Scope narrowed |
| Tier 2 | 3-5 variations per family with visual semantics | Variations explored |
| Evaluate | Trade-off matrix across relevant dimensions | Comparison table |
| Recommend | 3 options with reasoning at chosen depth | User picks approach |
| Document | UDR in decisions/explorations/ | Permanent record |
| Transition | Component-research or writing-specs | Next skill identified |

## Red Flags

| Thought | Reality |
|---------|---------|
| "Obviously this should be a bar chart" | That's one family. What about cards? Timeline? Heatmap? Explore. |
| "3 options is enough" | For Tier 1, you need 4-6 families. For Tier 2, 3-5 per family. Go wider. |
| "Visual semantics are obvious" | Document them explicitly. What's "obvious" to you is invisible to the next developer. |
| "The user probably wants a dashboard" | Ask. Frame the user task first. Maybe they want a single focused visualization. |
| "This data only works as a table" | Tables are a valid family. But challenge yourself — could a heatmap or card grid work better? |
| "I'll pick the prettiest one" | Pick the one that best serves the USER TASK, not the one that looks nicest. |

## Integration

**Called by:**
- **sdd:workflow** — When user says "visualize", "UI options", "how should we show this data?"
- **sdd:designing** — After architecture decision, IF UI is involved (check context or ask user)

**Can trigger:**
- **sdd:component-research** — After visualization chosen, if custom components needed
- **sdd:designing** — If visualization choice surfaces an architecture question

**Feeds into:**
- **sdd:writing-specs** — Chosen visualization becomes the wireframe in UI page specs
- **sdd:component-research** — Components identified in UDR drive the component audit

**Pairs with:**
- **sdd:designing** — Architecture decisions may constrain visualization options
- **sdd:component-research** — Sequential: visualization first, then component feasibility
- **sdd:creating-epics** — UDRs live in the epic's decisions/explorations folder
