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
- **Effort estimates are rough.** Plus/minus 50% is fine. The point is relative comparison, not project planning.

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
| "This library is popular so it must be good" | Popular does not mean right for YOUR design. Check the gap analysis. |
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
