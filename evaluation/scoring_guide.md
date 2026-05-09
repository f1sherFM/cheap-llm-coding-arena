# 🎯 Scoring Guide

> *"How we turn raw code into numbers — and why those numbers matter."*

---

## Overview

Each task is scored across **7 categories** with a maximum of **35 points**.

This guide defines the rubric for each category. Reviewers use this document as the single source of truth. If a score is challenged, this guide settles it.

---

## 1. ✅ Correctness — 10 points

> **Question:** Does the code solve the stated problem completely and correctly?

### What 10/10 Looks Like

- The code compiles / runs without errors.
- All functional requirements in the task description are satisfied.
- No logical errors or missing branches.
- Edge cases implied by the task are handled.

### What 5/10 Looks Like

- The code runs but misses a secondary requirement (e.g., validates `name` but forgets max length).
- A partial solution that works for the happy path but fails on edge cases.

### What 0/10 Looks Like

- Syntax errors prevent execution.
- The solution addresses the wrong problem entirely.
- The code runs but produces incorrect results for the primary use case.

### Quick Reference

| Score | Meaning |
|-------|---------|
| 10 | Perfect solution; all requirements met |
| 7-9 | Mostly correct; minor flaw or missing sub-case |
| 4-6 | Partial solution; major flaw but some progress |
| 1-3 | Wrong approach but compiles / runs |
| 0 | Does not compile, does not run, or completely wrong |

---

## 2. 🛡️ Regression Safety — 5 points

> **Question:** Does the code break anything that previously worked?

### What 5/5 Looks Like

- All existing tests pass unchanged.
- No API contracts are violated.
- No existing functionality is removed or altered.
- The fix is surgical — it touches only what needs to change.

### What 2/5 Looks Like

- One existing test fails due to a changed return type or renamed field.
- A previously working code path is now unreachable due to an early return.

### What 0/5 Looks Like

- Catastrophic regression: critical code deleted, API schema changed, or multiple existing tests fail.
- The "fix" introduces a new bug worse than the original.

### Quick Reference

| Score | Meaning |
|-------|---------|
| 5 | Zero regressions; all existing tests pass |
| 3-4 | Minor style drift or one non-critical test failure |
| 1-2 | Breaks existing functionality or removes working features |
| 0 | Catastrophic regression (e.g., deletes critical code) |

---

## 3. 🧠 Context Understanding — 5 points

> **Question:** Does the code respect the existing codebase's patterns, conventions, and architecture?

### What 5/5 Looks Like

- Uses existing helper functions and imports.
- Follows the project's naming conventions.
- Respects the existing abstraction layers (e.g., does not bypass the service layer to query the DB directly).
- Matches the style of surrounding code (async vs sync, typing patterns, etc.).

### What 2/5 Looks Like

- Imports a foreign pattern (e.g., uses `dataclasses` when the project uses `BaseModel`).
- Bypasses an existing abstraction for convenience.
- Minor naming inconsistency.

### What 0/5 Looks Like

- Hallucinates files, imports, or APIs that do not exist in the provided context.
- Completely ignores the existing architecture (e.g., writes a synchronous function in an async codebase).
- Introduces a new framework or paradigm without justification.

### Quick Reference

| Score | Meaning |
|-------|---------|
| 5 | Perfectly respects existing patterns, naming, and architecture |
| 3-4 | Mostly respects context; one or two deviations |
| 1-2 | Ignores codebase conventions; adds foreign patterns |
| 0 | Hallucinates files, imports, or APIs that do not exist |

---

## 4. 🎨 Code Quality — 5 points

> **Question:** Is the code clean, idiomatic, and maintainable?

### What 5/5 Looks Like

- Senior-developer quality: clear naming, proper decomposition, no duplication.
- Idiomatic for the language (e.g., Pythonic list comprehensions where appropriate).
- No unnecessary complexity.
- Self-documenting where possible; comments only where logic is subtle.

### What 2/5 Looks Like

- Functional but unpolished: verbose variable names, minor lint issues, slight duplication.
- Mixed paradigms (e.g., mixes functional and imperative styles inconsistently).

### What 0/5 Looks Like

- Unreadable or dangerously brittle code.
- Massive functions with no decomposition.
- Copy-paste duplication that would be a maintenance nightmare.
- Inconsistent formatting that would fail any reasonable linter.

### Quick Reference

| Score | Meaning |
|-------|---------|
| 5 | Clean, idiomatic, well-structured; senior-dev quality |
| 3-4 | Functional but unpolished; minor lint issues |
| 1-2 | Messy; hard to read; duplicated logic |
| 0 | Unreadable or dangerously brittle |

---

## 5. 🧪 Tests / Edge Cases — 5 points

> **Question:** Does the output include tests or explicitly handle edge cases?

### What 5/5 Looks Like

- Includes tests that cover:
  - Happy path
  - Empty input
  - Boundary conditions (e.g., max length, division by zero)
  - Invalid inputs (negative values, null, wrong types)
- Or, for non-test tasks: the code itself explicitly checks all obvious edge cases.

### What 2/5 Looks Like

- Handles the obvious happy path and one edge case.
- Missing tests for null / None inputs or boundary conditions.

### What 0/5 Looks Like

- No awareness of edge cases.
- Code that will fail in production the first time it encounters an empty list, zero, or None.
- No tests provided when the task explicitly asked for them.

### Quick Reference

| Score | Meaning |
|-------|---------|
| 5 | Includes tests or handles all edge cases (empty, None, overflow, etc.) |
| 3-4 | Handles most edge cases; minimal or no tests |
| 1-2 | Ignores obvious edge cases (e.g., division by zero) |
| 0 | No awareness of edge cases; will fail in production |

---

## 6. ⚡ Speed / Stability — 3 points

> **Question:** Was the response generated quickly and completely?

### What 3/3 Looks Like

- Response received in under 30 seconds.
- No truncation; the full solution is present.
- No provider-side errors or timeouts.

### What 1/3 Looks Like

- Slow response (> 60 seconds) or significant truncation.
- The solution is cut off mid-function and cannot be reasonably completed by concatenation.

### What 0/3 Looks Like

- Timeout, infinite loop, or severely truncated output that is unusable without a retry.
- Provider error that prevents any meaningful output.

### Quick Reference

| Score | Meaning |
|-------|---------|
| 3 | Fast response (< 30s); no truncation; complete output |
| 2 | Acceptable speed (30-60s); minor truncation but recoverable |
| 1 | Slow (> 60s) or significant truncation |
| 0 | Timeout, infinite loop, or severely truncated (unusable) |

---

## 7. 🔧 Manual Fixes Needed — 2 points

> **Question:** How much human intervention is required before this code can be merged?

This category is **inverted** — fewer fixes means more points.

### What 2/2 Looks Like

- Zero manual fixes required. Copy-paste ready.
- No missing imports, no typos, no misaligned indentation.
- Can be committed as-is and pass CI.

### What 1/2 Looks Like

- 1-2 trivial fixes needed: a missing import, a typo in a variable name, a closing bracket.
- Fixes take under 2 minutes.

### What 0/2 Looks Like

- Requires non-trivial rework or rewriting.
- The approach is wrong and needs to be redone.
- Multiple files need changes not suggested by the model.

### Quick Reference

| Score | Meaning |
|-------|---------|
| 2 | Zero manual fixes required; drop-in ready |
| 1 | 1-2 trivial fixes (missing import, typo) |
| 0 | Requires non-trivial rework or rewriting |

---

## Final Score Calculation

```text
Correctness          (0-10)
+ Regression Safety  (0-5)
+ Context Understanding (0-5)
+ Code Quality       (0-5)
+ Tests / Edge Cases (0-5)
+ Speed / Stability  (0-3)
+ Manual Fixes Needed (0-2)
───────────────────────────
= TOTAL              (0-35)
```

### Tag Assignment

| Total | Tag |
|-------|-----|
| ≥ 28 | 🟢 Junior-dev viable |
| 20-27 | 🟡 Viable with supervision |
| 12-19 | 🟠 Task-specific only |
| < 12 | 🔴 Not viable |

---

## Adjudication Policy

If two independent reviewers differ by **more than 3 points total**, a third reviewer adjudicates. The adjudicator:

1. Reads both scorecards without seeing the scores
2. Scores independently
3. The final score is the median of the three scores

All adjudications are documented in the scorecard with the note: `"Adjudicated by [reviewer_name]."`
