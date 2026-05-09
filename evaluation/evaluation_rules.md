# ⚖️ Evaluation Rules

> *"Trust, but verify. Then verify again."*

---

## 1. Prompt Integrity

### All Models Receive Identical Prompts

Every model in the tournament is given the **exact same prompt** for every task. This includes:

- System message
- Task description
- Codebase context
- Constraints and success criteria

The frozen prompts are published in [`prompts/`](../prompts/) and versioned (e.g., `v1`, `v2`).

| Prompt Component | Frozen? | Public? |
|------------------|---------|---------|
| System message | ✅ Yes | ✅ Yes |
| Task description | ✅ Yes | ✅ Yes |
| Codebase context | ✅ Yes | ✅ Yes |
| Constraints | ✅ Yes | ✅ Yes |
| Temperature | ✅ Yes | ✅ Yes |
| Max tokens | ✅ Yes | ✅ Yes |

---

## 2. No Hidden Prompts

There are **no hidden prompts**, no provider-specific optimizations, and no "jailbreaks." If a model requires special prompting to perform well, that is a weakness of the model — not a feature.

We do not:
- Tune the system message per provider
- Add "You are an expert programmer" for underperforming models
- Use chain-of-thought prompts unless the task explicitly requires them
- Modify the prompt after seeing a model's first response

---

## 3. No Answer Editing Before Scoring

> **Raw model outputs are scored exactly as generated.**

We never:
- Fix syntax errors
- Rename hallucinated imports
- Add missing returns or semicolons
- Reformat indentation
- Strip conversational text beyond basic code-block extraction

### Code Extraction Logic

| Scenario | Action |
|----------|--------|
| Code inside ` ```python ... ``` ` | Extract inner code as-is |
| Raw code without fences | Use as-is |
| Multiple code blocks | Concatenate in order |
| No code blocks present | Score output as text (likely 0 for correctness) |

---

## 4. Same Provider Settings When Possible

We document and standardize generation parameters:

| Parameter | Value | Rationale |
|-----------|-------|-----------|
| **Temperature** | `0.2` | Favors deterministic, focused outputs |
| **Top-p** | `0.95` (or provider default) | Sensible default |
| **Max tokens** | Task-dependent (2048-4096) | Sufficient for all tasks |
| **Presence penalty** | `0` | No bias against repeated terms |
| **Frequency penalty** | `0` | No bias against repeated terms |

> ⚠️ **Note:** Some providers do not expose all parameters (e.g., Anthropic does not have `temperature` in the same way). We document the closest equivalent and note any deviation in the scorecard.

---

## 5. No Retries Unless Explicitly Allowed

- **First completion only.** We do not send follow-up prompts like "fix this error" or "try again."
- If a model times out, truncates, or returns garbage, that output is scored as-is.
- The only exception: if a provider's API itself errors (HTTP 5xx, rate limit), we retry once after a 60-second delay and document it.

---

## 6. Raw Outputs Are Stored Publicly

Every model response is saved to [`model_outputs/`](../model_outputs/) in the following structure:

```text
model_outputs/
├── free/
│   ├── owl_alpha/
│   │   ├── 01_bugfix_v1.json
│   │   ├── 02_feature_v1.json
│   │   └── ...
│   └── ...
└── cheap_medium/
    ├── qwen3_coder_30b/
    │   ├── 01_bugfix_v1.json
    │   └── ...
    └── ...
```

Each file contains:
- Full raw response text
- Timestamp
- Provider parameters used
- Token usage (if available)

---

## 7. Scores Must Include Written Justification

Every score in every scorecard includes:

1. **The numeric score** for each category
2. **A written justification** (1-3 sentences) explaining why that score was given
3. **Evidence** (e.g., "Line 42 introduced a regression by removing the null check")

A score without written justification is **invalid** and will not be published.

### Example Justification

```text
Correctness: 8/10
Justification: The fix correctly handles missing profiles by returning a default
dict, but it missed the edge case where profile is explicitly set to None
rather than absent.
```

---

## 8. Blind Scoring

Reviewers are **blinded** to which model produced which output until scores are finalized. Outputs are labeled with random IDs during review.

| Reviewer | Knows Model Identity? |
|----------|----------------------|
| Reviewer 1 | ❌ No |
| Reviewer 2 | ❌ No |
| Adjudicator (if needed) | ❌ No |
| Public (after publication) | ✅ Yes |

---

## 9. Disqualification & Rematches

A model may be disqualified for:

- 🚫 Refusing to generate code (safety policy false-positive on benign code)
- 🚫 Outputting non-code for > 50% of the response
- 🚫 Hallucinating files or APIs that do not exist in the provided context

Disqualified models receive a score of `0` for that task and the tag `🔴 Not viable`.

**No rematches are granted.** A single bad output is data — it tells us how the model behaves under pressure.

---

## 10. Transparency Manifesto

| Claim | Proof |
|-------|-------|
| Prompts are identical | [`prompts/`](../prompts/) |
| Outputs are unedited | [`model_outputs/`](../model_outputs/) |
| Scores are justified | [`scorecards/`](../scorecards/) |
| Rules are fixed | [`rules.md`](../rules.md) |
| Method is documented | [`methodology.md`](../methodology.md) |
