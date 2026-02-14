# {BUG-ID}: Bug Checklist

> Skip protocol: `[x] ~~Item~~ — SKIPPED: {reason} (approved by: {name})`

## Investigation

- [ ] Bug folder created (INVESTIGATION.md, SOLUTION.md, CHECKLIST.md)
- [ ] Symptoms documented
- [ ] Reproduction steps documented
- [ ] Architecture mapped (affected services, data flow)
- [ ] Logs/metrics checked
- [ ] Recent changes reviewed

## Root Cause

- [ ] Root cause identified and documented
- [ ] Code location pinpointed
- [ ] INVESTIGATION.md status → root-cause-found

## Solution

- [ ] Solution options documented in SOLUTION.md
- [ ] Chosen solution approved by team

## Fix Implementation

- [ ] Regression test written (RED — reproduces bug)
- [ ] Fix implemented (GREEN — test passes)
- [ ] All existing tests still pass
- [ ] PR created and linked

## Post-Deploy Verification

- [ ] Deployed to environment
- [ ] Verified bug no longer reproducible
- [ ] Logs/metrics show improvement

## Closure

- [ ] Lessons learned documented
- [ ] Feature docs updated (if behavior changed)
- [ ] INVESTIGATION.md status → resolved
- [ ] Epic bugs table updated (if epic-scoped)
