# 😵 Hall of Shame: Catastrophic Failures

> *"Even the best models have bad days. We document them honestly."*

---

## About This Hall

The Hall of Shame exists for one reason: **developers deserve to know what can go wrong.**

Every entry here is a real output from a real model on a real task. No cherry-picking. No exaggeration. If a model hallucinates an entire API or deletes critical code, we show it.

> ⚠️ A model appearing here does not mean it is universally bad. It means it produced a dangerous or absurd output on at least one task. Context matters.

---

## Categories

| Category | Icon | Description |
|----------|------|-------------|
| **Catastrophic Hallucination** | 👻 | Invented files, imports, or APIs that do not exist |
| **Broken Refactor** | 💥 | Refactoring that breaks more than it fixes |
| **Dangerous Output** | ☠️ | Code that introduces security vulnerabilities or data loss |
| **Absurd Bug** | 🤡 | Fix that makes the original bug worse or creates a new one |

---

## Entries

> *Hall of Shame will be populated after Round 1 results are published.*

---

### 👻 Catastrophic Hallucinations

> *Models that invented entire modules, functions, or APIs out of thin air.*

| # | Model | Task | What Happened | Severity |
|---|-------|------|---------------|----------|
| — | — | — | *Pending Round 1* | — |

**Example Template (to be filled):**

```text
Model: ExampleModel v1.2
Task: 02_feature
Hallucination: Added "from app.services.magic_helper import auto_validate"
  which does not exist in the codebase.
Impact: Code would immediately fail on import.
Score Impact: Context Understanding = 0
```

---

### 💥 Broken Refactors

> *Refactoring attempts that introduced more bugs than they solved.*

| # | Model | Task | What Happened | Severity |
|---|-------|------|---------------|----------|
| — | — | — | *Pending Round 1* | — |

**Example Template (to be filled):**

```text
Model: ExampleModel v1.2
Task: 04_refactor
Failure: Converted callbacks to async but forgot to await the DB call,
  causing the function to return before the update completed.
Impact: Race condition; data loss in production.
Score Impact: Correctness = 3, Regression Safety = 0
```

---

### ☠️ Dangerous Outputs

> *Code that, if merged, would actively harm the system or its users.*

| # | Model | Task | What Happened | Severity |
|---|-------|------|---------------|----------|
| — | — | — | *Pending Round 1* | — |

**Example Template (to be filled):**

```text
Model: ExampleModel v1.2
Task: 02_feature
Failure: Used raw f-string SQL interpolation instead of parameterized queries.
Impact: SQL injection vulnerability.
Score Impact: Correctness = 2 (it "worked" in the happy path), Code Quality = 0
```

---

### 🤡 Absurd Bugs

> *"Fixes" that are more broken than the original bug.*

| # | Model | Task | What Happened | Severity |
|---|-------|------|---------------|----------|
| — | — | — | *Pending Round 1* | — |

**Example Template (to be filled):**

```text
Model: ExampleModel v1.2
Task: 01_bugfix
Failure: To fix the KeyError on missing `profile`, wrapped the entire
  serializer in a try/except that returns `{"error": "oops"}` for ANY exception.
Impact: All users now return an error dict instead of a user object.
Score Impact: Correctness = 1, Regression Safety = 0
```

---

## Honorable Mentions

> *Near-misses that were funny but not catastrophic.*

| # | Model | Task | What Happened |
|---|-------|------|---------------|
| — | — | — | *Pending Round 1* |

---

## How to Nominate

Think we missed a failure? Open an issue with:

1. The model name and version
2. The task number
3. A quote from the raw output (or link to `model_outputs/`)
4. Why you think it qualifies for the Hall of Shame

We will review and add it if it meets the criteria.

---

## Disclaimer

A model in the Hall of Shame on one task may still be a solid performer overall. This is not a condemnation — it is documentation. Every model has weaknesses. Our job is to find them before you do.
