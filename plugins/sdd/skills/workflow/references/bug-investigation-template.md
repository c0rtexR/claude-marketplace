---
id: "{BUG-ID}"
status: investigating
severity: "{critical/high/medium/low}"
reported: YYYY-MM-DD
epic: "{EPIC-ID or 'standalone'}"
---

# {BUG-ID}: {Bug Title}

## Status

| Field | Value |
|-------|-------|
| Status | investigating |
| Severity | {severity} |
| Reported | YYYY-MM-DD |
| Resolved | — |
| Service(s) | {affected services} |

## Symptoms

- **Expected behavior**: {what should happen}
- **Actual behavior**: {what actually happens}
- **Error messages**: {exact error text}
- **Frequency**: {always / intermittent / specific conditions}

## Reproduction Steps

1. {Step 1}
2. {Step 2}
3. {Step 3}
4. Observe: {what goes wrong}

## Architecture

```
{ASCII diagram of affected services and data flow}
```

| Component | Role | Relevant? |
|-----------|------|-----------|
| {service} | {what it does} | Yes/No |

## Investigation Notes

### {Date}: {What was checked}

{Findings, observations, log snippets.}

### Hypotheses

| # | Hypothesis | Evidence For | Evidence Against | Status |
|---|-----------|-------------|-----------------|--------|
| 1 | {theory} | {supporting evidence} | {contradicting evidence} | {confirmed/disproved/open} |

## Root Cause

{Clear description of WHY the bug happens.}

**Code location**: `{file}:{function}` in `{repository}`

**Why it happened**: {design flaw / edge case / recent change / etc.}
