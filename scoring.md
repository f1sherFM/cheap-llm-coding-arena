# 🎯 Scoring System

> *"Numbers don't lie. Prompts don't either."*

---

## Score Breakdown

**Maximum possible score:** `35 points`

Each task is evaluated across 7 categories. The total is the sum of all categories.

---

## 📊 Category Rubric

### 1. ✅ Correctness — 10 points

| Score | Description |
|-------|-------------|
| 10 | Code solves the problem completely and correctly |
| 7-9 | Mostly correct; minor logical flaw or missing sub-case |
| 4-6 | Partial solution; major flaw but some progress |
| 1-3 | Wrong approach but compiles/runs |
| 0 | Does not compile, does not run, or completely wrong |

---

### 2. 🛡️ Regression Safety — 5 points

| Score | Description |
|-------|-------------|
| 5 | Zero regressions; all existing tests pass |
| 3-4 | Minor style drift or one non-critical test failure |
| 1-2 | Breaks existing functionality or removes working features |
| 0 | Introduces catastrophic regression (e.g., deletes critical code) |

---

### 3. 🧠 Context Understanding — 5 points

| Score | Description |
|-------|-------------|
| 5 | Perfectly respects existing patterns, naming conventions, and architecture |
| 3-4 | Mostly respects context; one or two deviations |
| 1-2 | Ignores codebase conventions; adds foreign patterns |
| 0 | Hallucinates files, imports, or APIs that do not exist |

---

### 4. 🎨 Code Quality — 5 points

| Score | Description |
|-------|-------------|
| 5 | Clean, idiomatic, well-structured; senior-dev quality |
| 3-4 | Functional but unpolished; minor lint issues |
| 1-2 | Messy; hard to read; duplicated logic |
| 0 | Unreadable or dangerously brittle |

---

### 5. 🧪 Tests / Edge Cases — 5 points

| Score | Description |
|-------|-------------|
| 5 | Includes tests or explicitly handles all edge cases (empty input, None, overflow, etc.) |
| 3-4 | Handles most edge cases; minimal or no tests |
| 1-2 | Ignores obvious edge cases (e.g., division by zero) |
| 0 | No awareness of edge cases; will fail in production |

---

### 6. ⚡ Speed / Stability — 3 points

| Score | Description |
|-------|-------------|
| 3 | Fast response (< 30s); no truncation; complete output |
| 2 | Acceptable speed (30-60s); minor truncation but recoverable |
| 1 | Slow (> 60s) or significant truncation |
| 0 | Timeout, infinite loop, or severely truncated (unusable) |

---

### 7. 🔧 Manual Fixes Needed — 2 points

This category is **inverted** — fewer fixes means more points.

| Score | Description |
|-------|-------------|
| 2 | Zero manual fixes required; drop-in ready |
| 1 | 1-2 trivial fixes (missing import, typo) |
| 0 | Requires non-trivial rework or rewriting |

---

## 🏷️ Viability Tags

After all tasks are scored, models receive a **tag** based on their **average score per task**:

| Tag | Avg Score | Meaning |
|-----|-----------|---------|
| 🟢 **Junior-dev viable** | ≥ 28 / 35 | Safe to assign to a junior dev with minimal review |
| 🟡 **Viable with supervision** | 20-27 / 35 | Good output, but needs a senior eye before merge |
| 🟠 **Task-specific only** | 12-19 / 35 | Works for some task types, fails on others |
| 🔴 **Not viable** | < 12 / 35 | Unreliable for production use |

---

## 📋 Scorecard Template

Each model receives a scorecard per round. Here is the template:

```markdown
# Scorecard: {Model Name} — Round {N}

## Task Scores

| Task | Correctness | Regression | Context | Quality | Tests | Speed | Manual | Total |
|------|-------------|------------|---------|---------|-------|-------|--------|-------|
| 01_bugfix | | | | | | | | |
| 02_feature | | | | | | | | |
| 03_tests | | | | | | | | |
| 04_refactor | | | | | | | | |

## Summary

- **Average Score:** / 35
- **Tag:** {tag}
- **Disqualifications:** {None / Task X}

## Reviewer Notes

{Qualitative observations}
```

---

## 🧮 Example Score Calculation

```text
Task: 01_bugfix
├── Correctness ........... 8 / 10
├── Regression Safety ..... 5 / 5
├── Context Understanding . 4 / 5
├── Code Quality .......... 4 / 5
├── Tests / Edge Cases .... 3 / 5
├── Speed / Stability ..... 3 / 3
└── Manual Fixes Needed ... 2 / 2

Total = 8 + 5 + 4 + 4 + 3 + 3 + 2 = 29 / 35
Tag = 🟢 Junior-dev viable
```
