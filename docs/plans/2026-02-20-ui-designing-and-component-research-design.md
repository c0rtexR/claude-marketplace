# UI Designing & Component Research Skills — Design Doc

> **Approved:** 2026-02-20

## Problem

SDD has a gap between architecture decisions (`sdd:designing`) and UI spec writing (`sdd:writing-specs` Step 2). The current UI page template jumps straight to one wireframe with a components table. No exploration of HOW to visualize data — bar chart vs heatmap vs card grid vs timeline. No research into WHETHER existing components can deliver the chosen visualization.

Two missing capabilities:
1. **Visualization R&D** — exploring multiple ways to present the same data, with deep reasoning about why one approach works best
2. **Component landscape audit** — researching what exists, what needs modification, what must be built from scratch, and whether we're reusing because it's right or because we're lazy

## Decisions

| Decision | Choice | Reasoning |
|----------|--------|-----------|
| Scope | Information architecture + visual semantics | Not full design system. Color-coding meaning, icon language, density — yes. Typography scales, spacing systems — no. |
| Exploration structure | Two-tier: families then variations | First round: 4-6 visualization families. Pick best 2. Second round: 3-5 variations within each. Depth without chaos. |
| Component research output | High-level assessment | Flags what exists, what's missing, what needs modification. Detailed modification specs go into `writing-specs`. |
| Innovation gate | 3 options with recommendation (AskUserQuestion pattern) | Per component: reuse / extend / build. Recommendation + reasoning. User picks. |
| Justification depth | User-configurable via AskUserQuestion | Skill asks at start: practical only / principles + practical / full rationale. |
| Skill chaining | Sequential: ui-designing → component-research | Visualization decision drives component search, not the other way around. Innovation-first. |
| Placement | Standalone siblings of sdd:designing | Three separate concerns: architecture, visualization, component feasibility. |
| Naming | `sdd:ui-designing` + `sdd:component-research` | Clear parallel with existing `sdd:designing` but scoped. |
| Task tracking | Both skills create task list at start | All steps tracked. Nothing gets lost. |
| UI gate | Context check + AskUserQuestion if unclear | Not every app has a UI. Skills only chain when UI is involved. |

---

## Skill 1: `sdd:ui-designing`

### Purpose

Deep R&D into how to visualize data or design a UI screen. Explores multiple visualization families, drills into variations, and documents reasoning at the user's chosen depth.

### Flow

```
Step 1: Create task list for all steps
        │
Step 2: Frame the visualization problem
        What data? What user task? What decisions will users make?
        │
Step 3: Ask justification depth (AskUserQuestion)
        Option A: Practical only ("users compare values → bar chart")
        Option B: Principles + practical (Gestalt, data-ink ratio + practical)
        Option C: Full rationale (design principles, cognitive science, best practices)
        │
Step 4: Explore visualization families (Tier 1)
        Generate 4-6 families, each with ASCII wireframe
        Score against user task. Present to user. Pick top 2.
        │
Step 5: Deep-dive variations (Tier 2)
        3-5 variations within each winning family
        Each with ASCII wireframe + visual semantics
        │
Step 6: Trade-off matrix
        Compare variations across dimensions relevant to THIS data
        │
Step 7: Recommend with reasoning (at chosen justification depth)
        Present recommendation + alternatives via AskUserQuestion
        │
Step 8: Document as UI Design Record (UDR)
        Permanent artifact in decisions/explorations/
        │
Step 9: Transition gate
        Custom components needed? → suggest sdd:component-research
        No custom components? → suggest sdd:writing-specs
```

### Tier 1: Visualization Families

The skill identifies which families apply based on the data type and user task. Not all families apply to every problem.

| Family | Best For | Example Variations |
|--------|----------|-------------------|
| Comparative | Comparing values across categories | Bar, column, bullet, lollipop, radar |
| Temporal | Showing change over time | Line, area, sparkline, timeline, Gantt |
| Compositional | Parts of a whole | Stacked bar, treemap, sunburst, waffle, donut |
| Spatial | Geographic or layout-based | Heatmap, floor plan, node map, choropleth |
| Relational | Connections between entities | Network graph, chord diagram, Sankey, arc |
| Distributional | Spread and density of values | Histogram, box plot, violin, scatter, bee swarm |
| Hierarchical | Nested groupings | Tree, org chart, nested cards, accordion, indented list |
| Status/Dashboard | KPI overview, at-a-glance health | Card grid, gauge, traffic light, scorecard, sparkline strip |
| Narrative | Guided storytelling through data | Infographic, step flow, annotated chart, scrollytelling |
| Interactive | User-driven exploration | Filter panel, drill-down, pivot table, faceted search |

Each family gets an ASCII wireframe showing the shape:

```
Family: Comparative — Grouped Bar Chart

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

### Visual Semantics Layer

For each variation, document:
- **Color-coding rules** — what colors mean (danger, success, warning, neutral)
- **Emphasis patterns** — what draws attention first (size, position, contrast)
- **Density decisions** — information density vs breathing room
- **Icon/symbol language** — what symbols mean (▲ up, ▼ down, → stable)

### Output: UI Design Record (UDR)

Template: `references/udr-template.md`

Stored in `decisions/explorations/UDR-NNN-{slug}.md`

---

## Skill 2: `sdd:component-research`

### Purpose

After choosing HOW to visualize (from `ui-designing`), research WHETHER existing components can deliver it. Produces a build/reuse/customize decision per component with the explicit "lazy vs innovating" gate.

### Flow

```
Step 1: Create task list from visualization decision
        One task per component need from the UDR
        │
Step 2: Per component — landscape scan
        What libraries/components exist?
        What do they support? What's their API?
        │
Step 3: Per component — gap analysis
        Design needs vs component provides
        Rate: perfect fit / needs tweaks / major customization / build from scratch
        │
Step 4: Per component — present 3 options with recommendation
        Option A: Use existing (reuse path)
        Option B: Extend/customize existing (modify path)
        Option C: Build from scratch (innovate path)
        + Recommendation with reasoning
        Ask user via AskUserQuestion pattern
        │
Step 5: Innovation check (explicit gate)
        Side-by-side: IDEAL (from UDR) vs REUSE GIVES US
        "Are we following existing patterns because they're right,
         or because we're being lazy?"
        User sees exactly what they'd lose by reusing.
        User decides.
        │
Step 6: Document as Component Research Record (CRR)
        Summary of all component decisions
        │
Step 7: Transition
        Suggest sdd:writing-specs — CRR feeds into UI specs
```

### The 3-Option Pattern (Per Component)

```
Component: Sprint Velocity Chart

Option A: Recharts BarChart (Recommended)
  Supports: grouped bars, tooltips, responsive
  Missing: custom color-coding by threshold, trend arrows
  Effort: ~2hr customization
  Trade-off: 80% of design. Trend arrows overlaid.

Option B: Custom D3 bar chart
  Supports: everything — full control
  Missing: nothing, but requires building from zero
  Effort: ~8hr build
  Trade-off: 100% of design. Higher maintenance.

Option C: Nivo BarChart
  Supports: grouped bars, themes, animations
  Missing: custom overlays, limited tooltip customization
  Effort: ~4hr customization
  Trade-off: 70% of design. Tooltip limitations visible.

Recommendation: Option A (Recharts). 80% coverage is strong.
Missing 20% can be overlaid with wrapper component.
```

### The Innovation Check

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
Question: Is the gap acceptable or does it compromise the design?
```

### Output: Component Research Record (CRR)

Template: `references/crr-template.md`

Summary table:

| Component | Verdict | Library | Gaps | Effort |
|-----------|---------|---------|------|--------|
| Velocity chart | Extend | Recharts | Trend arrows, threshold colors | ~2hr |
| Team scorecard | Build | — | Nothing fits card layout | ~4hr |
| Filter panel | Reuse | Radix Select | None | ~30min |

---

## Integration

### Workflow Chain (with UI Gate)

```
sdd:designing
    │
    ▼
UI involved? ──── NO ──→ sdd:writing-specs
    │
   YES (from context or AskUserQuestion)
    ▼
sdd:ui-designing
    │
    ▼
Custom components needed? ──── NO ──→ sdd:writing-specs
    │
   YES
    ▼
sdd:component-research
    │
    ▼
sdd:writing-specs
```

Each gate checks context first (epic docs, feature description, existing code). Asks the user via AskUserQuestion only if it can't determine from context.

### Workflow Route Table Additions

| User Says | Invoke | Why |
|-----------|--------|-----|
| "How should we show this data?", "visualize", "UI options", "wireframe options" | **sdd:ui-designing** | Visualization R&D |
| "What components exist?", "component audit", "build or reuse?", "what library?" | **sdd:component-research** | Component landscape |

### Skill Chain

| From | To | When |
|------|-----|------|
| `designing` | `ui-designing` | After architecture decision, IF UI involved |
| `ui-designing` | `component-research` | After visualization chosen, IF custom components needed |
| `component-research` | `writing-specs` | After all component decisions — feeds into UI specs |
| `ui-designing` | `designing` | If visualization choice surfaces an architecture question |
| `component-research` | `spiking` | If a component needs a feasibility spike |

### Folder Structure

```
epics/{EPIC-ID}-{slug}/decisions/
├── INDEX.md
├── explorations/
│   ├── EXP-001-{slug}.md          # Architecture explorations (existing)
│   ├── UDR-001-{slug}.md          # UI Design Records (NEW)
│   └── CRR-001-{slug}.md          # Component Research Records (NEW)
```

### Files to Create/Modify

| File | Action |
|------|--------|
| `skills/ui-designing/SKILL.md` | **CREATE** — new skill |
| `skills/ui-designing/references/udr-template.md` | **CREATE** — UI Design Record template |
| `skills/component-research/SKILL.md` | **CREATE** — new skill |
| `skills/component-research/references/crr-template.md` | **CREATE** — Component Research Record template |
| `skills/workflow/SKILL.md` | **MODIFY** — add to route table, diagram, skill chain |
| `skills/writing-specs/SKILL.md` | **MODIFY** — reference UDR/CRR when writing UI specs |
| `skills/designing/SKILL.md` | **MODIFY** — add UI gate in integration section |
| `.claude-plugin/plugin.json` | **MODIFY** — version 2.5.0, 10 skills |
