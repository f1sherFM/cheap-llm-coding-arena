# 🔬 Methodology

> *"How we test, why we test this way, and what it means for you."*

---

## Overview

The **cheap-llm-coding-arena** is not a synthetic benchmark. It is a controlled experiment designed to replicate the real-world workflow of a developer using an LLM to solve a coding task.

Every model is put through the same scenario: receive a task, read context, generate code, and (implicitly) justify the solution. The only variable is the model itself.

---

## 1. Task Design

### Real-World Origin

All tasks are derived from **real coding problems** encountered in production codebases. They are anonymized and simplified for reproducibility, but they retain the core complexity that makes them challenging:

- **State management** across async boundaries
- **Implicit dependencies** not explicitly listed in the task description
- **Edge cases** discovered only after the bug hit production
- **Refactoring risks** where changing one module breaks three others

### Task Types

| Type | What We Measure |
|---|---|
| 🐛 Bug Fix | Can the model diagnose root cause vs. symptoms? |
| ✨ Feature Add | Can it extend existing architecture without breaking it? |
| 🧪 Tests | Can it infer intent and cover edge cases? |
| 🔧 Refactor | Can it improve structure while preserving behavior? |

📄 See individual task descriptions in [`tasks/`](tasks/).

---

## 2. Prompt Construction

### The "Frozen Prompt" Rule

> **Every model receives the exact same prompt.**

The prompt consists of:

1. **System message** — identical for all models (no provider-specific optimizations)
2. **Task description** — the user request, written in natural language
3. **Codebase context** — relevant files, imports, and types
4. **Constraints** — e.g., "Do not add new dependencies", "Keep changes minimal"

```text
--- SYSTEM ---
You are a senior software engineer. You will be given a coding task.
Respond with code only. Do not add conversational filler.

--- TASK ---
[Task description]

--- CONTEXT ---
[Relevant source files]
```

### Why No Prompt Engineering?

- We are testing **models**, not prompt-engineering skills.
- A model that requires "jailbreaks" or "magic phrases" to perform is not viable for daily use.
- Our target audience — busy developers — pastes the task and hits Enter. We simulate exactly that.

---

## 3. Output Processing

### Zero Human Edits

> **Model outputs are never edited before scoring.**

This is the most important rule in our methodology. We do not:
- Fix syntax errors
- Rename hallucinated imports
- Strip conversational text (we extract code blocks, but do not modify the code inside)
- Re-run the model if the first attempt fails

### Extraction Logic

If the model wraps code in markdown fences (```python ... ```), we extract the inner code.  
If the model outputs raw code without fences, we use it as-is.  
If the model outputs multiple code blocks, we concatenate them in order.

---

## 4. Evaluation Environment

### Runtime Testing

Every extracted output is:

1. **Placed** into the original codebase snapshot
2. **Executed** against a private test harness
3. **Checked** for:
   - Syntax errors
   - Runtime exceptions
   - Test pass/fail rate
   - Regression failures (previously passing tests that now fail)

### Static Analysis

We also run:

- Linter (e.g., `ruff`, `eslint`) for style violations
- Type checker (e.g., `mypy`, `tsc`) for type safety
- Complexity metrics (cyclomatic complexity, cognitive complexity)

---

## 5. Scoring Process

### Independent Review

Each output is scored by **two reviewers** who do not know which model produced it.

### Consensus

- If scores differ by ≤ 3 points, they are averaged.
- If scores differ by > 3 points, a **third reviewer** adjudicates.

### Final Tag Assignment

Tags (`Junior-dev viable`, `Viable with supervision`, etc.) are assigned based on the **average score across all tasks** for that model, not per-task.

---

## 6. Reproducibility

### Versioning

- Task prompts are versioned (`v1`, `v2`, etc.).
- Codebase snapshots are pinned by commit hash (for public code) or by a SHA-256 hash (for anonymized code).
- Model versions are recorded at the time of testing (e.g., `gpt-oss-120b-2026-05-08`).

### Re-running

Anyone can re-run the tournament:

1. Clone this repo
2. Replace API keys in the runner config
3. Execute `./run_round.sh` (coming soon)

---

## 7. Limitations & Caveats

| Caveat | Explanation |
|---|---|
| 🔮 Single-turn only | We do not test multi-turn refinement. This is a deliberate choice to simulate "quick ask" workflows. |
| 🌡️ Temperature | All models run at `temperature=0.2` (or the provider's closest equivalent) to favor determinism. |
| 📅 Pricing drift | Prices change. We snapshot prices at round time and note the date. |
| 🏢 Private code | Some tasks use anonymized private code. We provide the same context to all models, but you cannot verify it externally. |

---

## 8. Tournament Philosophy

> **The best LLM for coding is the one that reduces your time-to-merge.**

We do not care about:
- Elo scores on chatbot leaderboards
- MMLU or HumanEval rankings
- How "smart" the model sounds in a conversation

We care about:
- Does the code work?
- Does it break existing tests?
- Can a junior dev understand and maintain it?
- How much babysitting does it need?

That is what this arena measures.
