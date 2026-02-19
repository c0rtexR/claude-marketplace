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
