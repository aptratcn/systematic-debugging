---
name: systematic-debugging
version: 1.0.0
description: Systematic Debugging - 4-phase root cause process. No guessing, no random fixes. Trace evidence, identify root cause, fix systematically.
emoji: 🔍
tags: [debugging, troubleshooting, root-cause, systematic, ai-agent]
---

# Systematic Debugging 🔍

4-phase root cause process. No guessing, no random fixes.

## The Problem

Most AI debugging looks like:
```
"Try X"
"Didn't work"
"Try Y"
"Didn't work"
"Try Z"
"That broke something else"
```

This is not debugging. This is guessing.

## The Solution: 4-Phase Process

### Phase 1: Reproduce & Document 🔒

**Goal:** Establish a reliable reproduction

**Steps:**
1. Get exact error message
2. Identify exact conditions that trigger it
3. Create minimal reproduction case
4. Document what you know

```
Documentation Template:

## Bug Report

**Error Message:**
[Exact error, full stack trace if available]

**Reproduction Steps:**
1. [Exact steps to trigger]

**Expected:**
[What should happen]

**Actual:**
[What happens instead]

**Environment:**
- OS: [from uname -a]
- Version: [from package.json or version command]
- Last working: [if known]
- First broken: [if known]

**Minimal Reproduction:**
[Smallest code that triggers the issue]
```

**Gate:** Can you reproduce it on command? If no, don't proceed.

### Phase 2: Gather Evidence 🔒

**Goal:** Collect data, not theories

**Steps:**
1. Check logs (application, system, error)
2. Add logging/debugging if needed
3. Compare working vs broken cases
4. Document findings, not conclusions

```
Evidence Collection Checklist:

[ ] Application logs checked
[ ] System logs checked
[ ] Network requests logged (if applicable)
[ ] Database queries logged (if applicable)
[ ] Memory/CPU profile checked (if performance issue)
[ ] Compared with last known working version
[ ] Searched for similar issues (GitHub issues, Stack Overflow)

Evidence Table:

| Observation | Data |
|-------------|------|
| Error occurs when | [condition] |
| Error does NOT occur when | [condition] |
| Related log entries | [entries] |
| Different in working vs broken | [diffs] |
```

**Gate:** Do you have concrete evidence, not just theories? If no, gather more.

### Phase 3: Identify Root Cause 🔒

**Goal:** Find THE reason, not A reason

**Steps:**
1. Form hypothesis based on evidence
2. Design test to validate hypothesis
3. Run test
4. If hypothesis wrong, form new hypothesis (go to step 1)
5. If right, verify it's THE root cause (not just symptom)

```
Root Cause Validation:

Hypothesis: [Why you think it happens]
Test: [How to prove/disprove]
Expected if hypothesis correct: [What you'll see]
Actual result: [What you actually saw]

Root Cause vs Symptom Check:
- Is this the earliest point where things go wrong? → If no, trace back further
- Would fixing this definitely solve the problem? → If maybe, not root cause yet
- Can you explain the full chain from this cause to observed error? → If no, gaps exist

Root Cause Found: [Yes/No]
If Yes: [State the root cause]
If No: [What else to investigate]
```

**Gate:** Can you explain the full chain from root cause to observed error? If no, not root cause yet.

### Phase 4: Fix & Verify 🔒

**Goal:** Fix the root cause, verify it works

**Steps:**
1. Design fix for root cause (not symptom)
2. Implement fix
3. Verify original error is gone
4. Verify fix doesn't break anything else
5. Add regression test
6. Document fix

```
Fix Verification Checklist:

[ ] Fix targets root cause, not symptom
[ ] Original error no longer occurs
[ ] All existing tests still pass
[ ] New regression test added
[ ] Edge cases tested
[ ] Related functionality verified
[ ] Fix documented in code comments or docs

Before/After:

| Metric | Before | After |
|--------|--------|-------|
| Error occurs | Yes | No |
| Tests passing | X/Y | Y/Y |
| Side effects | N/A | None detected |
```

**Gate:** All checkboxes checked? If no, not done yet.

## Anti-Patterns

| Anti-Pattern | Why It's Bad | Fix |
|--------------|--------------|-----|
| "Let me try X" | Random guessing | Reproduce first |
| "I think it's..." | Theory without evidence | Gather evidence |
| Fixing symptoms | Problem comes back | Find root cause |
| "It works now" | Might not be fixed | Verify systematically |
| No regression test | Bug comes back | Add test |

## Debugging Decision Tree

```
Error occurs
    |
    v
Can you reproduce it?
    |
    +-- No --> Add logging, try again
    |
    +-- Yes --> Document reproduction
                    |
                    v
              Evidence gathered?
                    |
                    +-- No --> Check logs, add logging, compare versions
                    |
                    +-- Yes --> Hypothesis formed?
                                    |
                                    +-- No --> Analyze evidence
                                    |
                                    +-- Yes --> Test hypothesis
                                                    |
                                                    +-- Wrong --> New hypothesis
                                                    |
                                                    +-- Right --> Root cause found?
                                                                    |
                                                                    +-- No --> Trace back further
                                                                    |
                                                                    +-- Yes --> Fix & verify
```

## Integration with Other Skills

- **EVR Framework** — Fix is executed, verified, reported
- **Workflow Checkpoint** — Track debugging progress
- **Error Recovery** — Systematic retry after identifying cause

## Quick Reference

```
Phase 1: Reproduce → Document exact error and steps
Phase 2: Evidence → Logs, comparisons, data (no theories yet)
Phase 3: Root Cause → Hypothesis → Test → Validate
Phase 4: Fix → Target root cause → Verify → Regression test
```

## License

MIT
