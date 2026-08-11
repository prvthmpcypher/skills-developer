---
name: debugging-strategist
description: >-
  Applies systematic debugging methodologies including binary search isolation, hypothesis-driven log analysis, memory leak profiling, race condition detection, and structured root cause analysis. Use when diagnosing complex bugs, investigating production errors, profiling memory leaks, or debugging intermittent failures.
---

# Debugging Strategist

Applies structured, scientific debugging methodologies to isolate, diagnose, and resolve complex software defects efficiently.

## Phased Workflow

### Phase 1: Problem Definition & Hypothesis Formation
1. Reproduce the bug with a minimal, deterministic test case (or document exact reproduction steps if intermittent).
2. Classify the defect: Logic error, State corruption, Race condition, Memory leak, External dependency failure.
3. Form 2-3 ranked hypotheses based on error messages, stack traces, and recent code changes.

### Phase 2: Systematic Isolation
1. **Binary Search Debugging:** Bisect the codebase (git bisect) or execution path to narrow the fault location by 50% each step.
2. **Differential Analysis:** Compare working vs broken states (configs, env vars, data, code versions).
3. **Instrumentation:** Add targeted logging, breakpoints, or assertions to validate/invalidate each hypothesis.
4. **Specialized Profiling:**
   - Memory leaks: Heap snapshots, allocation flamegraphs, weak reference audits.
   - Race conditions: Thread sanitizers, lock ordering analysis, happens-before relationship mapping.
   - Performance: CPU flamegraphs, I/O wait analysis, query EXPLAIN plans.

### Phase 3: Fix, Verify & Prevent
1. Implement the minimal fix that addresses root cause (not symptoms).
2. Write a regression test that fails without the fix and passes with it.
3. Document the root cause and add defensive assertions to prevent recurrence.

## Verification & Quality Checklist
- [ ] Root cause identified and documented (not just symptoms patched).
- [ ] Regression test added that reproduces the original failure mode.
- [ ] Fix is minimal and doesn't introduce new side effects.
- [ ] Related code paths audited for similar patterns.

## Anti-Patterns & Constraints
- NEVER apply a fix without understanding the root cause first.
- NEVER debug by randomly changing code ("shotgun debugging").
- NEVER remove error handling or assertions to "fix" a failing test.
