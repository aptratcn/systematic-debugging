# Debugging Cheatsheet

Quick-reference card for the 4-phase systematic debugging process.

---

## Phase 1: Observe — "What's Happening?"

**Goal:** Gather facts before forming theories.

| Step | Action | Example |
|------|--------|---------|
| 1.1 | Reproduce the bug | "Running `parse(null)` throws TypeError" |
| 1.2 | Note exact error message | Copy the full stack trace |
| 1.3 | Identify the scope | Which module? Which function? Which line? |
| 1.4 | Check recent changes | `git log --oneline -10` |
| 1.5 | Gather environment info | OS, version, dependencies, config |

**Key Questions:**
- ❓ Can I reproduce it reliably?
- ❓ Does it happen every time or intermittently?
- ❓ Did it work before? When did it break?
- ❓ What's different about the failing case vs. the working case?

**Anti-Patterns to Avoid:**
- ❌ Jumping to solutions before understanding the problem
- ❌ Changing multiple things at once
- ❌ Assuming you know the cause

---

## Phase 2: Hypothesize — "Why Might This Happen?"

**Goal:** Generate specific, testable explanations.

### Hypothesis Template
```
I believe [X] is causing [Y] because [Z].
I can test this by [action].
If I'm right, I'll see [expected result].
If I'm wrong, I'll see [alternative result].
```

### Common Hypothesis Categories

| Category | Pattern | Quick Test |
|----------|---------|------------|
| **Null/undefined** | Missing value passed through | Log the input at the failing line |
| **Type mismatch** | String vs number, object vs array | `typeof` / `instanceof` check |
| **Async timing** | Race condition, unresolved promise | Add console.log before/after await |
| **State mutation** | Shared object modified unexpectedly | Deep clone before passing |
| **Off-by-one** | Loop boundary, index miscalculation | Print indices at boundary |
| **Config/env** | Missing env var, wrong base URL | Print config at startup |
| **Dependency** | Version mismatch, breaking change | Check lockfile, diff versions |
| **Encoding** | UTF-8 vs ASCII, BOM, whitespace | Hex dump the input |

### Prioritization Heuristic
1. **Simplest first** — Null check before race condition theory
2. **Most likely first** — Type errors > cosmic ray bit flips
3. **Most impactful first** — If it affects all users, prioritize
4. **Cheapest to test first** — Console.log > profiler setup

**Anti-Patterns to Avoid:**
- ❌ Vague hypotheses ("something's wrong with the data")
- ❌ Untestable hypotheses ("the framework is buggy")
- ❌ Fixating on one hypothesis and ignoring alternatives

---

## Phase 3: Test — "Does the Evidence Support It?"

**Goal:** Verify or eliminate hypotheses with minimal, targeted experiments.

### Testing Strategies

| Strategy | When to Use | How |
|----------|------------|-----|
| **Log injection** | Quick sanity check | `console.log('>>> value:', value)` at key points |
| **Breakpoint** | Need to inspect state | Set breakpoint, examine variables |
| **Isolation** | Multiple interacting systems | Reproduce with minimal test case |
| **Bisection** | Don't know when it broke | `git bisect` to find the commit |
| **Substitution** | Suspect a dependency | Swap with a mock/stub |
| **Comparison** | Working vs broken case | Diff the inputs/outputs side-by-side |

### The Binary Search Trick
When you don't know where the bug is:
1. Put a checkpoint in the **middle** of the execution path
2. Is the state correct at the midpoint?
   - **Yes** → Bug is in the second half
   - **No** → Bug is in the first half
3. Repeat on the suspect half
4. ~O(log n) steps to find the exact line

### Test Case Template
```javascript
// Test for: [hypothesis description]
// Expected: [what should happen]
// Actual: [what's happening]

const input = { /* minimal repro case */ };
const result = functionUnderTest(input);
console.log('Result:', result);
console.log('Expected:', expectedValue);
console.log('Match:', JSON.stringify(result) === JSON.stringify(expectedValue));
```

**Anti-Patterns to Avoid:**
- ❌ Testing multiple hypotheses at once (can't tell which fix worked)
- ❌ Changing code without a clear hypothesis (shotgun debugging)
- ❌ Ignoring contradictory evidence ("but it should work!")

---

## Phase 4: Fix — "Resolve and Prevent"

**Goal:** Apply the fix, verify it, and prevent recurrence.

### Fix Checklist

- [ ] **Fix addresses the root cause**, not just the symptom
- [ ] **Minimal change** — Don't refactor while fixing
- [ ] **Add a regression test** — Prevents the bug from returning
- [ ] **Verify the original repro case** — Does it work now?
- [ ] **Check for similar bugs** — Same pattern elsewhere?
- [ ] **No new bugs introduced** — Run existing test suite
- [ ] **Document the fix** — Commit message explains what and why

### Commit Message Template
```
fix: [short description of the bug]

Root cause: [what was actually wrong]
Symptom: [what the user saw]
Fix: [what was changed and why]

Test: [how it was verified]
Replaces: #[issue number] (if applicable)
```

### Prevention Patterns

| Bug Type | Prevention |
|----------|-----------|
| Null/undefined | Add input validation, use optional chaining |
| Type mismatch | TypeScript strict mode, runtime type checks |
| Async timing | Proper error handling, Promise.allSettled |
| State mutation | Immutable patterns, deep clone on boundaries |
| Off-by-one | Use range-based loops, test boundaries explicitly |
| Config/env | Validate config at startup, fail fast with clear message |
| Dependency | Pin versions, lockfile committed, CI catches drift |

**Anti-Patterns to Avoid:**
- ❌ "Fixing" by adding a try-catch that swallows the error
- ❌ Patching symptoms instead of root causes
- ❌ Forgetting the regression test (it will come back)

---

## Emergency Debugging (Production Fire)

When the site is down and pressure is high:

```
1. BREATHE. Rushed fixes create second bugs.
2. OBSERVE: What's the error? What changed? When did it start?
3. MITIGATE: Rollback > hotfix. Restore service first.
4. INVESTIGATE: Apply the 4-phase process on the rolled-back code.
5. FIX: In a branch, with tests, then deploy deliberately.
```

**Rule:** Never debug in production. Reproduce locally.

---

## Quick Reference Card

```
OBSERVE  →  Reproduce, get the error, check git log
    ↓
HYPOTHESIZE  →  "X causes Y because Z" (specific, testable)
    ↓
TEST  →  One hypothesis at a time, minimal experiment
    ↓
FIX  →  Root cause, regression test, verify, prevent
```

**The One Rule:** Never change code without a hypothesis. If you can't explain *why* your change should fix it, you're not debugging — you're guessing.
