# UI Designing & Component Research Skills — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add two new SDD skills (`sdd:ui-designing` and `sdd:component-research`) and integrate them into the existing workflow.

**Architecture:** Two new skill folders with SKILL.md + reference templates each. Modifications to workflow, designing, and writing-specs for integration. Plugin version bump 2.4.0 → 2.5.0.

**Tech Stack:** Markdown skill files, YAML frontmatter, ASCII diagrams.

---

### Task 1: Create `ui-designing/references/udr-template.md`

**Files:**
- Create: `plugins/sdd/skills/ui-designing/references/udr-template.md`

**Step 1: Create the UDR template**

```markdown
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
```

**Step 2: Verify the file was created**

Run: `ls -la plugins/sdd/skills/ui-designing/references/udr-template.md`
Expected: File exists

**Step 3: Commit**

```bash
git add plugins/sdd/skills/ui-designing/references/udr-template.md
git commit -m "feat(sdd): add UDR template for ui-designing skill"
```

---

### Task 2: Create `ui-designing/SKILL.md`

**Files:**
- Create: `plugins/sdd/skills/ui-designing/SKILL.md`

**Step 1: Create the skill file**

```markdown
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
```

**Step 2: Verify the file was created**

Run: `ls -la plugins/sdd/skills/ui-designing/SKILL.md`
Expected: File exists

**Step 3: Commit**

```bash
git add plugins/sdd/skills/ui-designing/SKILL.md
git commit -m "feat(sdd): add ui-designing skill"
```

---

### Task 3: Create `component-research/references/crr-template.md`

**Files:**
- Create: `plugins/sdd/skills/component-research/references/crr-template.md`

**Step 1: Create the CRR template**

```markdown
---
id: CRR-NNN
date: YYYY-MM-DD
visualization_source: # UDR-NNN reference
status: # in-progress | decided
epic: # epic ID if applicable, or "standalone"
---

# CRR-NNN: {Short Title}

## Source

Based on: {UDR-NNN} — {visualization decision summary}

## Components Needed

| # | Component | Purpose | From UDR |
|---|-----------|---------|----------|
| 1 | {component name} | {what it does in the visualization} | {UDR reference} |

---

## Component 1: {Name}

### Landscape Scan

| Library | Component | Version | License |
|---------|-----------|---------|---------|
| {library} | {component name} | {version} | {license} |

### Gap Analysis

| Feature Needed | {Library A} | {Library B} | Custom Build |
|---------------|-------------|-------------|--------------|
| {feature 1} | ✅ | ✅ | ✅ |
| {feature 2} | ✅ | ❌ | ✅ |
| {feature 3} | ❌ | ❌ | ✅ |
| Coverage | {X}% | {X}% | 100% |

### Options

**Option A: {Library/approach} (Recommended)**
- Supports: {what it covers}
- Missing: {what gaps remain}
- Effort: {estimated time}
- Trade-off: {what % of design is achieved}

**Option B: {Library/approach}**
- Supports: {what it covers}
- Missing: {what gaps remain}
- Effort: {estimated time}
- Trade-off: {what % of design is achieved}

**Option C: Build from scratch**
- Supports: everything — full control
- Missing: nothing, but requires building from zero
- Effort: {estimated time}
- Trade-off: 100% of design. Higher maintenance burden.

**Recommendation:** {Option X}. {Reasoning.}

**User Decision:** {what the user chose}

---

{Repeat for each component}

---

## Innovation Check

### Side-by-Side Comparison

```
┌─────────────────────────┐  ┌─────────────────────────┐
│ IDEAL (from UDR)        │  │ REUSE GIVES US          │
│                         │  │                         │
│ {ideal wireframe}       │  │ {reuse wireframe}       │
│                         │  │                         │
└─────────────────────────┘  └─────────────────────────┘
```

### Gap Assessment

| Aspect | Ideal | Reuse | Gap |
|--------|-------|-------|-----|
| {aspect} | {ideal behavior} | {reuse behavior} | {what's lost} |

### Verdict

{Are we following existing patterns because they're right, or because we're being lazy?}

**Decision:** {innovate / reuse / extend — with reasoning}

---

## Summary

| Component | Verdict | Library | Gaps | Effort |
|-----------|---------|---------|------|--------|
| {component 1} | {reuse/extend/build} | {library or "custom"} | {gaps} | {effort} |
| {component 2} | {reuse/extend/build} | {library or "custom"} | {gaps} | {effort} |

**Total estimated effort:** {sum}
```

**Step 2: Verify the file was created**

Run: `ls -la plugins/sdd/skills/component-research/references/crr-template.md`
Expected: File exists

**Step 3: Commit**

```bash
git add plugins/sdd/skills/component-research/references/crr-template.md
git commit -m "feat(sdd): add CRR template for component-research skill"
```

---

### Task 4: Create `component-research/SKILL.md`

**Files:**
- Create: `plugins/sdd/skills/component-research/SKILL.md`

**Step 1: Create the skill file**

```markdown
---
name: component-research
description: "Use when deciding whether to build, extend, or reuse UI components — auditing existing libraries, analyzing gaps, and making build-vs-reuse decisions. Also triggers on: 'what components exist?', 'component audit', 'build or reuse?', 'what library?', 'component options', 'UI component research'."
---

# Component Research

## Overview

After deciding HOW to visualize data (from `sdd:ui-designing`), research WHETHER existing components can deliver it. Produces a build/reuse/customize decision per component with an explicit "lazy vs innovating" gate.

**Core principle:** Innovation starts from the ideal, not from what's available. Design the perfect component first, then see if reality can match it.

## When to Use

- "What components exist for this chart type?"
- "Should we use Recharts or build custom?"
- "Build or reuse?"
- After `sdd:ui-designing` — visualization chosen, need to audit component options
- Standalone — evaluating a component library for any UI need

## Step 1: Create Task List

**Before doing anything else**, create a task for each component that needs research, plus the innovation check:

For each component from the UDR (or from the user's request):
1. Landscape scan for {component name}
2. Gap analysis for {component name}
3. Present 3 options for {component name}

Plus:
- Innovation check (after all components)
- Document as CRR

## Step 2: Per Component — Landscape Scan

What libraries and components exist for this need?

Research:
- Popular libraries in the project's ecosystem (React → Recharts, Nivo, Victory, Tremor, shadcn/ui, etc.)
- What each component supports (props, customization, theming)
- Version, maintenance status, license
- Bundle size impact

Document findings in a scan table per component.

## Step 3: Per Component — Gap Analysis

Compare what the design needs (from the UDR) vs what each option provides:

| Feature Needed | Library A | Library B | Custom Build |
|---------------|-----------|-----------|--------------|
| Grouped bars | ✅ | ✅ | ✅ |
| Threshold coloring | ❌ | ✅ | ✅ |
| Trend arrows | ❌ | ❌ | ✅ |
| Coverage | 70% | 85% | 100% |

Rate each option: perfect fit / needs tweaks / major customization / build from scratch.

## Step 4: Per Component — Present 3 Options with Recommendation

Use the AskUserQuestion pattern. Always present exactly 3 options:

**Option A:** Use existing component (reuse path)
- What it supports, what's missing, effort estimate, design coverage %

**Option B:** Extend/customize existing component (modify path)
- What to wrap/override, effort estimate, design coverage %

**Option C:** Build from scratch (innovate path)
- Full control, effort estimate, 100% design coverage, maintenance trade-off

**Include your recommendation** with reasoning. Present via AskUserQuestion — the user picks.

### Example

```
Component: Sprint Velocity Chart

Option A: Recharts BarChart (Recommended)
  Supports: grouped bars, tooltips, responsive
  Missing: threshold coloring, trend arrows
  Effort: ~2hr customization
  Trade-off: 80% of design

Option B: Custom D3 bar chart
  Supports: everything — full control
  Missing: nothing, builds from zero
  Effort: ~8hr build
  Trade-off: 100% of design. Higher maintenance.

Option C: Nivo BarChart
  Supports: grouped bars, themes, animations
  Missing: custom overlays, limited tooltips
  Effort: ~4hr customization
  Trade-off: 70% of design

Recommendation: Option A. 80% coverage is strong, missing 20%
can be overlaid with wrapper component.
```

## Step 5: Innovation Check (Explicit Gate)

**After all component decisions**, present a side-by-side comparison:

```
┌─────────────────────────┐  ┌─────────────────────────┐
│ IDEAL (from UDR)        │  │ REUSE GIVES US          │
│                         │  │                         │
│ ████  ░░░░  ▲+12%      │  │ ████  ░░░░              │
│ ████  ░░░░  ▼-8%       │  │ ████  ░░░░              │
│ Custom threshold colors │  │ Default library colors  │
│ Trend arrows inline     │  │ No trend indicators     │
│ Animated transitions    │  │ Basic transitions       │
└─────────────────────────┘  └─────────────────────────┘

Gap: Trend arrows, threshold colors, animation quality.
```

Ask the user explicitly:

**"Are we following existing patterns because they're right, or because we're being lazy?"**

The user sees exactly what they'd lose by reusing. They decide whether the gap is acceptable or whether it compromises the design.

## Step 6: Document as Component Research Record (CRR)

Use `references/crr-template.md`. Store alongside the UDR in `decisions/explorations/CRR-NNN-{slug}.md`.

Summary table:

| Component | Verdict | Library | Gaps | Effort |
|-----------|---------|---------|------|--------|
| Velocity chart | Extend | Recharts | Trend arrows, threshold colors | ~2hr |
| Team scorecard | Build | — | Nothing fits card layout | ~4hr |
| Filter panel | Reuse | Radix Select | None | ~30min |

## Step 7: Transition

Suggest `sdd:writing-specs` — the CRR feeds directly into UI page specs, API specs, and component architecture.

---

## Folder Structure

**Epic-scoped:**
```
epics/{EPIC-ID}-{slug}/decisions/
├── INDEX.md
├── explorations/
│   ├── UDR-001-{slug}.md          # UI Design Records
│   └── CRR-001-{slug}.md          # Component Research Records
```

**Standalone:**
```
decisions/
├── INDEX.md
├── explorations/
│   └── CRR-001-{slug}.md
```

Use `references/crr-template.md` for each Component Research Record.

## Key Disciplines

- **Start from the ideal, not from what's available.** Design the perfect component first (from UDR), then check if reality can match it.
- **Always present exactly 3 options.** Reuse, extend, build. Even when the answer seems obvious.
- **The innovation check is not optional.** Side-by-side IDEAL vs REUSE forces an honest assessment.
- **High-level assessment only.** Detailed modification specs belong in `writing-specs`. This skill decides WHAT to use, not HOW to implement it.
- **Task list at the start, always.** One task per component + innovation check. Track everything.
- **Effort estimates are rough.** ±50% is fine. The point is relative comparison, not project planning.

## Quick Reference

| Phase | What You Do | Output |
|-------|------------|--------|
| Task list | One task per component + innovation check | Tracked progress |
| Scan | What libraries/components exist? | Landscape documented |
| Gap | Design needs vs component provides | Coverage % per option |
| Options | 3 options with recommendation per component | User picks per component |
| Innovation check | Side-by-side IDEAL vs REUSE | Honest lazy-vs-innovating verdict |
| Document | CRR in decisions/explorations/ | Permanent record |
| Transition | Suggest writing-specs | Next skill identified |

## Red Flags

| Thought | Reality |
|---------|---------|
| "Just use Recharts, it's fine" | Fine for what? Check the gap analysis. 70% coverage might compromise the design. |
| "Building custom is always better" | Custom means maintenance burden. Reuse is fine when it covers 90%+. |
| "The gap doesn't matter" | Show the user the side-by-side. Let THEM decide if the gap matters. |
| "I'll figure out the component later" | Component feasibility affects the design. Research now, not during implementation. |
| "This library is popular so it must be good" | Popular ≠ right for YOUR design. Check the gap analysis. |
| "We can just modify it to fit" | How much modification? If >50% custom, you're building a custom component with extra dependencies. |

## Integration

**Called by:**
- **sdd:workflow** — When user says "component audit", "build or reuse?", "what library?"
- **sdd:ui-designing** — After visualization chosen, if custom components needed

**Feeds into:**
- **sdd:writing-specs** — Component decisions feed into UI specs and architecture

**Can trigger:**
- **sdd:spiking** — If a component needs a feasibility spike ("can Recharts actually do this?")

**Pairs with:**
- **sdd:ui-designing** — Sequential: visualization first, then component feasibility
- **sdd:designing** — Architecture constraints may limit component choices
- **sdd:creating-epics** — CRRs live in the epic's decisions/explorations folder
```

**Step 2: Verify the file was created**

Run: `ls -la plugins/sdd/skills/component-research/SKILL.md`
Expected: File exists

**Step 3: Commit**

```bash
git add plugins/sdd/skills/component-research/SKILL.md
git commit -m "feat(sdd): add component-research skill"
```

---

### Task 5: Modify `workflow/SKILL.md`

**Files:**
- Modify: `plugins/sdd/skills/workflow/SKILL.md`

**Step 1: Update the skill count and description**

In the frontmatter (line 3), change:
```
description: "Skills-Driven Development — a complete spec-first, TDD methodology for AI agent teams. Use this skill whenever the user wants to create an epic, plan features, implement with TDD, investigate bugs, coordinate agent teams, run spikes, or explore design options. Also triggers on: 'new project', 'start feature', 'plan epic', 'implement phase', 'investigate bug', 'what's next', 'create PR', 'close epic', 'spike', 'explore', 'design options'."
```
To:
```
description: "Skills-Driven Development — a complete spec-first, TDD methodology for AI agent teams. Use this skill whenever the user wants to create an epic, plan features, implement with TDD, investigate bugs, coordinate agent teams, run spikes, explore design options, design UI visualizations, or research components. Also triggers on: 'new project', 'start feature', 'plan epic', 'implement phase', 'investigate bug', 'what's next', 'create PR', 'close epic', 'spike', 'explore', 'design options', 'visualize', 'UI options', 'component audit', 'build or reuse'."
```

**Step 2: Update the EXTREMELY-IMPORTANT section**

Change line 7:
```
SDD is a methodology with 8 specialized skills.
```
To:
```
SDD is a methodology with 10 specialized skills.
```

**Step 3: Update the ASCII workflow diagram**

Replace the existing diagram (lines 16-63) with:

```
New project / "let's build X"
    │
    ▼
┌─────────────────┐
│ sdd:creating-    │ ◄── Create epic structure, interview user,
│ epics            │     set up folder hierarchy, CLAUDE.md
└────────┬────────┘
         │ Technical unknowns?
         ▼
┌─────────────────┐
│ sdd:spiking      │ ◄── Prove feasibility before specs:
│                  │     build POC or investigate existing code
└────────┬────────┘
         │ Design approach unclear?
         ▼
┌─────────────────┐
│ sdd:designing    │ ◄── Explore options, trade-offs, diagrams
│                  │     → ADR formalizing the decision
└────────┬────────┘
         │ UI involved?
         ▼
┌─────────────────┐
│ sdd:ui-designing │ ◄── Visualization families → variations →
│                  │     wireframes → visual semantics → UDR
└────────┬────────┘
         │ Custom components needed?
         ▼
┌──────────────────┐
│ sdd:component-   │ ◄── Landscape scan → gap analysis →
│ research         │     build/reuse/extend → innovation check
└────────┬─────────┘
         │ Features identified
         ▼
┌─────────────────┐
│ sdd:writing-     │ ◄── UI specs → API specs → DB specs →
│ specs            │     Test plan → Phase plans
└────────┬────────┘
         │ Specs complete, phases defined
         ▼
┌─────────────────┐
│ sdd:implementing-│ ◄── RED → GREEN → REFACTOR → E2E →
│ with-tdd         │     Zero-tolerance gates → PR
└────────┬────────┘
         │ Bug found during implementation
         ▼
┌─────────────────┐
│ sdd:investigating│ ◄── Investigate → Root cause → Regression
│ -bugs            │     test → Fix → Coding standards feedback
└─────────────────┘

         ┌─────────────────┐
         │ sdd:coordinating│ ◄── Overlay: use when ANY phase
         │ -agent-teams    │     needs multi-agent coordination
         └─────────────────┘

         ┌─────────────────┐
         │ sdd:reflecting   │ ◄── Capture learnings, run retrospectives,
         │                  │     turn patterns into process improvements
         └─────────────────┘
```

**Step 4: Add to route table**

After the "design options", "explore" row, add these two rows:

```
| "How should we show this?", "visualize", "UI options", "wireframe options" | **sdd:ui-designing** | Visualization R&D |
| "What components exist?", "component audit", "build or reuse?", "what library?" | **sdd:component-research** | Component landscape |
```

**Step 5: Add to invocation list**

After `Skill: sdd:designing`, add:

```
Skill: sdd:ui-designing
Skill: sdd:component-research
```

**Step 6: Add to skill chain**

After the `designing` entries, add:

```
- `designing` → UI involved? → **can invoke** `sdd:ui-designing`
- `ui-designing` → custom components needed? → **can invoke** `sdd:component-research`
- `ui-designing` → visualization decided, no custom components → feeds into `sdd:writing-specs`
- `component-research` → all decisions made → feeds into `sdd:writing-specs`
- `component-research` → needs feasibility check → **can invoke** `sdd:spiking`
```

**Step 7: Commit**

```bash
git add plugins/sdd/skills/workflow/SKILL.md
git commit -m "feat(sdd): integrate ui-designing and component-research into workflow"
```

---

### Task 6: Modify `designing/SKILL.md`

**Files:**
- Modify: `plugins/sdd/skills/designing/SKILL.md`

**Step 1: Add UI gate to Integration section**

At the end of the Integration section (after the existing "Pairs with" entries), add a new subsection:

```markdown
**UI gate:**
- After architecture decision, check context: does this feature involve a UI?
- If YES → suggest `sdd:ui-designing` for visualization exploration
- If NO → suggest `sdd:writing-specs` directly
- If UNCLEAR → ask the user: "Does this feature involve a user interface?"
```

Also add to the "Can trigger" list:
```
- **sdd:ui-designing** — Architecture decision made, feature involves UI, visualization approach unclear
```

**Step 2: Commit**

```bash
git add plugins/sdd/skills/designing/SKILL.md
git commit -m "feat(sdd): add UI gate to designing skill integration"
```

---

### Task 7: Modify `writing-specs/SKILL.md`

**Files:**
- Modify: `plugins/sdd/skills/writing-specs/SKILL.md`

**Step 1: Add UDR/CRR reference to Step 2 (UI Specs)**

At the start of Step 2 (Write UI Specs), add a note:

```markdown
**If a UDR exists** (from `sdd:ui-designing`): The chosen visualization and visual semantics from the UDR become the wireframe and component basis for the page spec. Reference the UDR ID.

**If a CRR exists** (from `sdd:component-research`): Component decisions (reuse/extend/build, library choices) inform the Components table. Reference the CRR ID.
```

**Step 2: Add to Integration section**

Add to "Called by":
```
- **sdd:ui-designing** — Visualization decided, feeds into UI specs
- **sdd:component-research** — Component decisions feed into UI specs and architecture
```

**Step 3: Commit**

```bash
git add plugins/sdd/skills/writing-specs/SKILL.md
git commit -m "feat(sdd): reference UDR/CRR in writing-specs UI step"
```

---

### Task 8: Modify `.claude-plugin/plugin.json`

**Files:**
- Modify: `plugins/sdd/.claude-plugin/plugin.json`

**Step 1: Bump version and update description**

Change version from `"2.4.0"` to `"2.5.0"`.

Change description from:
```
"Skills-Driven Development — a spec-first, TDD methodology with 8 practical skills: creating-epics, writing-specs, implementing-with-tdd, investigating-bugs, coordinating-agent-teams, reflecting, spiking, and designing. Pick the skill that matches what you're doing."
```
To:
```
"Skills-Driven Development — a spec-first, TDD methodology with 10 practical skills: creating-epics, writing-specs, implementing-with-tdd, investigating-bugs, coordinating-agent-teams, reflecting, spiking, designing, ui-designing, and component-research. Pick the skill that matches what you're doing."
```

**Step 2: Commit**

```bash
git add plugins/sdd/.claude-plugin/plugin.json
git commit -m "chore(sdd): bump version to 2.5.0 — 10 skills"
```

---

### Task 9: Verify all files and cross-references

**Step 1: Verify all new files exist**

Run:
```bash
ls -la plugins/sdd/skills/ui-designing/SKILL.md
ls -la plugins/sdd/skills/ui-designing/references/udr-template.md
ls -la plugins/sdd/skills/component-research/SKILL.md
ls -la plugins/sdd/skills/component-research/references/crr-template.md
```
Expected: All 4 files exist

**Step 2: Verify skill count in workflow**

Run: `grep -c "Skill: sdd:" plugins/sdd/skills/workflow/SKILL.md`
Expected: 10

**Step 3: Verify version bump**

Run: `grep '"version"' plugins/sdd/.claude-plugin/plugin.json`
Expected: `"version": "2.5.0"`

**Step 4: Verify integration references**

Run: `grep -l "ui-designing" plugins/sdd/skills/*/SKILL.md`
Expected: workflow, designing, writing-specs, ui-designing, component-research

Run: `grep -l "component-research" plugins/sdd/skills/*/SKILL.md`
Expected: workflow, writing-specs, ui-designing, component-research

**Step 5: Final commit (if any fixes needed)**

```bash
git add -A
git commit -m "fix(sdd): cross-reference fixes for ui-designing and component-research"
```
