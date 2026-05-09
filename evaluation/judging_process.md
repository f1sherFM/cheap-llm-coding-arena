# ⚔️ Judging Process

> *"How a match runs, how a winner is crowned, and how the community can verify everything."*

---

## Match Lifecycle

A **match** is the complete evaluation of one model against all four tasks in a round. Here is the lifecycle of every match:

```text
┌─────────────┐    ┌──────────────┐    ┌─────────────┐    ┌──────────────┐
│   PROMPT    │───▶│   EXECUTE    │───▶│   EXTRACT   │───▶│   VALIDATE   │
│ Preparation │    │    Model     │    │    Code     │    │   Runtime    │
└─────────────┘    └──────────────┘    └─────────────┘    └──────────────┘
                                                          │
                              ┌─────────────┐    ┌────────▼───────┐
                              │  PUBLISH    │◀───│     SCORE      │
                              │  Results    │    │   (2 reviewers)│
                              └─────────────┘    └────────────────┘
```

---

## 1. Prompt Preparation

### Before the Match

1. The frozen prompt for the task is loaded from [`prompts/`](../prompts/).
2. The codebase snapshot is verified against its pinned commit hash.
3. The evaluation environment is reset to a clean state.

### During the Match

The same prompt is sent to the model via its API or local inference endpoint. Generation parameters are logged:

```json
{
  "model": "qwen3-coder-30b-a3b",
  "task": "01_bugfix",
  "prompt_version": "v1",
  "temperature": 0.2,
  "top_p": 0.95,
  "max_tokens": 4096,
  "timestamp": "2026-05-09T14:00:00Z"
}
```

---

## 2. Model Execution

### Single-Shot Rule

Only the **first completion** is recorded and evaluated. No follow-ups.

### Timeout Handling

| Scenario | Action |
|----------|--------|
| Response within time limit | Proceed to extraction |
| Response truncated at token limit | Score as-is; Speed/Stability = 1 or 0 |
| API timeout (no response) | Scorecard marked `DNS` (Did Not Submit); score = 0 |
| API error (5xx, rate limit) | One retry after 60s; documented in scorecard |

---

## 3. Output Extraction

### Automated Extraction

A script extracts code blocks from the raw response:

1. Finds all ` ```python ... ``` ` fences.
2. Extracts inner content.
3. Concatenates multiple blocks in order.
4. Writes extracted code to `model_outputs/{bracket}/{model}/{task}.py`.

### Human Verification

A human reviewer quickly checks that the extraction script did not miss anything (e.g., code outside fences). If the script failed, the reviewer extracts manually and documents why.

---

## 4. Runtime Validation

### The Test Harness

Extracted code is injected into the original codebase snapshot. The harness runs:

```bash
# 1. Syntax check
python -m py_compile extracted.py

# 2. Linter
ruff check extracted.py

# 3. Type checker
mypy extracted.py

# 4. Test suite
pytest tests/ --tb=short

# 5. Regression suite
pytest tests/ --run-regression
```

### Results Logging

| Check | Pass | Fail | Impact |
|-------|------|------|--------|
| Syntax | ✅ | ❌ | Correctness = 0 if fail |
| Linter | ✅ | ⚠️ | Code Quality reduced if fail |
| Type checker | ✅ | ⚠️ | Code Quality reduced if fail |
| Tests | ✅ | ❌ | Correctness reduced if fail |
| Regression | ✅ | ❌ | Regression Safety reduced if fail |

---

## 5. Scoring

### Independent Review

Two reviewers receive:
- The original task description
- The extracted code
- The runtime validation results
- **No indication of which model produced the output**

Each reviewer fills out a scorecard using the rubric in [`scoring_guide.md`](scoring_guide.md).

### Consensus

| Score Difference | Resolution |
|----------------|------------|
| ≤ 3 points | Scores are averaged |
| > 3 points | Third reviewer adjudicates; median score wins |

### Final Scorecard

The finalized scorecard contains:
- Per-category scores with written justifications
- Runtime validation results
- Reviewer names (post-publication)
- Any adjudication notes

---

## 6. Publication

### What Gets Published

For every match, the following are committed to the repo:

| Artifact | Location | Contains |
|----------|----------|----------|
| Raw output | `model_outputs/{bracket}/{model}/{task}.json` | Full API response |
| Extracted code | `model_outputs/{bracket}/{model}/{task}.py` | Code after extraction |
| Scorecard | `scorecards/{bracket}/{model}_round_{n}.md` | Scores + justifications |
| Match summary | `match_history/{bracket}/{model}_matches.md` | Head-to-head results |

### Leaderboard Updates

After all models in a bracket complete a round:

1. Average scores are computed.
2. Leaderboards are updated in [`leaderboards/`](../leaderboards/).
3. Bracket progression charts are generated (if applicable).
4. A release tag is created: `round-1-results`.

---

## 7. How the Winner Is Determined

### Per-Task Winner

The model with the **highest score** on a single task wins that task.

### Per-Round Winner

The model with the **highest average score across all tasks** wins the round.

### Bracket Champion

The bracket champion is the model with the best average score across all completed rounds.

### Overall Champion

There is **no overall champion** across brackets. Free and Cheap/Medium are separate leagues because:
- Free models may be rate-limited or less reliable.
- Paid models have access to better infrastructure.
- Comparing them directly would be unfair.

---

## 8. Community Verification

Anyone can verify our results:

1. Clone the repo
2. Read the prompts in [`prompts/`](../prompts/)
3. Read the raw outputs in [`model_outputs/`](../model_outputs/)
4. Check the scorecards in [`scorecards/`](../scorecards/)
5. Re-run the test harness on the extracted code

If you find an error in scoring, open a PR with:
- The corrected score
- Written justification
- Evidence from the raw output

---

## Process Integrity Checklist

- [x] Prompts frozen and published before any model runs
- [x] Outputs extracted automatically, verified by human
- [x] Runtime validation run on every output
- [x] Two independent reviewers per output
- [x] Blind scoring (reviewers do not know the model)
- [x] Written justification for every score
- [x] Raw outputs stored publicly
- [x] Scorecards published with full transparency
- [x] Community can reproduce and challenge results
