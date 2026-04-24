# Systematic Debugging 🔍

> **Stop guessing. Stop shotgun debugging. 4-phase root cause process that actually works.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Zero Dependencies](https://img.shields.io/badge/dependencies-0-green.svg)]()
[![Skill](https://img.shields.io/badge/type-agent--skill-blue.svg)]()

## The Problem

How do you debug now? Be honest:

| Bad Habit | What Happens | Time Wasted |
|-----------|-------------|-------------|
| 🎲 Guess-and-check | Change random things hoping one works | Hours |
| 🩹 Symptom fixing | Patch the error, ignore the cause | Recurs forever |
| 🔁 Loop debugging | Same bug, different day | Days/weeks |
| 🤖 AI spray-and-pray | "Try this... try that..." | 20+ attempts |

**The data:** 70% of debugging time is spent on approaches that don't work. Most bugs are fixed in minutes once the root cause is identified.

## The Solution: 4-Phase Process

```
Bug Report
    │
    ▼
┌──────────────┐
│ 1. OBSERVE   │  What IS happening? (Not what you THINK)
│   Reproduce  │  Gather facts, not assumptions
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ 2. HYPOTHESIZE│  List ALL possible causes
│   Rank them  │  What evidence proves/disproves each?
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ 3. VERIFY    │  Test #1 hypothesis first
│   One at time│  One variable. Document negative results.
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ 4. FIX       │  Fix the ROOT CAUSE
│   + Prevent  │  Add regression test. Document the lesson.
└──────────────┘
```

## Phase 1: OBSERVE — Gather Facts

**Before you touch anything:**

```bash
# Reproduce the issue
# What's the EXACT error message?
# What were you doing when it happened?
# Does it happen every time?

# System state
env | grep -i <relevant_var>
which <command>
<command> --version
```

**Output:** A clear, reproducible bug description. No "it doesn't work" — exact steps, exact error.

## Phase 2: HYPOTHESIZE — Brainstorm Causes

**List every possible cause, rank by likelihood:**

```
Hypothesis                  | Likelihood | Test
────────────────────────────|────────────|──────
Config typo                 | High       | Check config file
Database connection dropped | Medium     | Test DB connection
Race condition              | Low        | Add logging
Memory corruption           | Very Low   | Run under valgrind
```

**Rule:** Never test the fun/interesting hypothesis first. Test the most LIKELY one first.

## Phase 3: VERIFY — Test One at a Time

```
For each hypothesis:
1. State what you'll test
2. Change ONE thing only
3. Record the result (positive OR negative)

❌ "I changed 3 things and it works now" → Which one fixed it? 🤷
✅ "Changed timeout from 5→30s. Still fails. Hypothesis eliminated."
```

**Negative results are valuable:** Every eliminated hypothesis narrows the search.

## Phase 4: FIX & PREVENT

```bash
# Fix the ROOT CAUSE, not the symptom
# Symptom fix: catch the exception and retry
# Root fix: fix why the exception happens

# Add regression test
# If it broke once, it can break again

# Document the lesson
echo "Bug: [description]. Cause: [root cause]. Fix: [what changed]. Lesson: [takeaway]" >> debug-log.md
```

## Anti-Patterns This Prevents

| Anti-Pattern | What People Do | What This Skill Forces |
|-------------|---------------|----------------------|
| **Shotgun debugging** | Change 5 things at once | One variable at a time |
| **Assumption cascade** | "I think it's X" → waste hours | Observe first, hypothesize second |
| **Sunk cost debugging** | "I've spent 3 hours on this hypothesis" | Eliminated? Move on. |
| **AI suggestion spam** | "Try this... try that..." | Hypothesis → Verify → Next |
| **Symptom patching** | Add try-catch around the error | Fix why the error happens |

## Quick Start

```bash
# Claude Code
cp SKILL.md ~/.claude/skills/systematic-debugging/

# OpenClaw
cp SKILL.md ~/.openclaw/workspace/skills/systematic-debugging/

# Any agent: Copy SKILL.md content into your agent's skill directory
```

## Example Session

```
User: "The API returns 500 errors intermittently"

Agent (without this skill):
  "Try restarting the server. Try adding retries. Try checking the logs."

Agent (with this skill):
  "Let's apply systematic debugging.
   
   OBSERVE: How often? Which endpoints? Any patterns in timing?
   
   HYPOTHESIZE:
   1. Database connection pool exhaustion (likely - intermittent)
   2. Memory leak in worker process (possible - gets worse over time)
   3. External API timeout (less likely - would show different error)
   
   Let's test #1 first. Can you check the DB connection pool metrics?"
```

## Works With

- [OpenClaw](https://openclaw.ai)
- Claude Code, Cursor, Codex
- Any agent framework
- Human developers (seriously, try this)

## Related Skills

- [skill-error-recovery](https://github.com/aptratcn/skill-error-recovery) — 4R error recovery framework
- [evr-framework](https://github.com/aptratcn/evr-framework) — Execute-Verify-Report for completions
- [cognitive-debt-guard](https://github.com/aptratcn/cognitive-debt-guard) — Prevent AI code comprehension issues

## License

MIT — Debug smarter, not harder.
