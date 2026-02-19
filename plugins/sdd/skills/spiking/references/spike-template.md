---
id: SPIKE-NNN
date: YYYY-MM-DD
question: "{The specific technical question}"
mode: # build | investigate
status: # in-progress | go | no-go | needs-more-investigation
time_budget: # optional, e.g. "2 hours"
epic: # epic ID if applicable, or "standalone"
---

# SPIKE-NNN: {Short Title}

## Question

{One specific technical question. Not "explore everything" — one question.}

## Success Criteria

{What evidence would answer go vs no-go? Be specific.}
- [ ] {Criterion 1 — e.g. "Response time < 200ms under 1000 concurrent users"}
- [ ] {Criterion 2}

## Evidence

### Build Mode
{POC implementation notes, code in `poc/` directory}

### Investigate Mode
{Landscape map, code path traces, measurements, impact assessment}

### Raw Data
{Benchmark results, log snippets, test output, profiling data}

## Verdict

**Status:** {go | no-go | needs-more-investigation}

**Summary:** {One paragraph — what did we learn?}

**If go:** {What approach to take, key constraints discovered}

**If no-go:** {Why it doesn't work, what alternatives exist}

**If needs-more-investigation:** {What questions remain, what to try next}

## Impact Assessment (Investigate Mode)

| Dimension | Finding |
|-----------|---------|
| Files affected | {list} |
| Dependencies | {list} |
| Hard constraints | {what can't move} |
| Risk level | {low / medium / high} |
| Estimated effort | {if go, how much work is the real implementation?} |
