# 📜 Tournament Rules

> *"Same arena. Same prompts. No excuses."*

---

## 1. Bracket Definitions

### 🆓 Free Bracket

A model qualifies for the **Free Bracket** if it meets **ALL** of the following:

| Requirement | Detail |
|---|---|
| 💳 No payment | No credit card required at sign-up |
| 🎁 No trial caps | Not limited to a one-time trial credit pool |
| 🌐 Publicly accessible | Any developer can obtain an API key or use the web UI |
| 📅 Current as of test date | Pricing verified within 7 days of the round |

### 💰 Cheap / Medium Bracket

A model qualifies for the **Cheap / Medium Bracket** if its pricing falls within:

| Type | Limit |
|---|---|
| **Input** | ≤ `$1.00` per 1,000,000 tokens |
| **Output** | ≤ `$3.00` per 1,000,000 tokens |

> 💡 **Note:** Models that appear in both Free and Cheap brackets (e.g., OpenAI gpt-oss-120b, NVIDIA Nemotron 3 Super) are tested separately under each bracket's access tier.

---

## 2. Model Eligibility

- Only models that satisfy **either** the Free or Cheap/Medium criteria may compete.
- **Outside bracket** models (e.g., GPT-4o at $2.50/$10.00, Claude Opus at $15.00/$75.00) **do NOT** participate.
- If a model changes pricing mid-tournament, its existing scores remain valid; future rounds use the updated pricing.

---

## 3. Prompt Rules

| Rule | Description |
|---|---|
| 🔒 Identical prompts | Every model receives the **exact same** task prompt, including system message and context |
| 🚫 No follow-ups | Only the **first** completion is scored. No "try again" or "fix this" prompts |
| 🧊 Frozen context | The codebase snapshot provided in the prompt is identical for all competitors |
| ⏱️ No timeout hacks | If a model times out or truncates, it receives a zero for Speed/Stability and the output is scored as-is |

---

## 4. Output Handling

| Rule | Description |
|---|---|
| ✂️ No editing | Model outputs are **never** manually edited before scoring |
| 📄 Code only | If a model adds conversational filler (e.g., "Sure, here is the code..."), the evaluator extracts only the code block |
| 🐛 No human bugfixes | If the model's code has a syntax error, it is scored down. No one fixes it behind the scenes |

---

## 5. Scoring Integrity

- Each task is scored by **at least two independent reviewers** using the rubric in [`scoring.md`](scoring.md).
- Discrepancies > 3 points are resolved by a third reviewer.
- Reviewers are blinded to which model produced which output until scores are finalized.

---

## 6. Disqualification

A model may be disqualified for:

- 🚫 Refusing to generate code (e.g., safety policy false-positives on benign code)
- 🚫 Outputting non-code for > 50% of the response
- 🚫 Hallucinating files or APIs that do not exist in the provided context

Disqualified models receive a score of `0` for that task and the tag `🔴 Not viable`.

---

## 7. Tournament Philosophy

> We do not optimize for vibes. We optimize for **shipping**.

This tournament is designed to mimic the experience of a real developer pasting a task description into an LLM and dropping the result into a PR. If the model's output requires a senior engineer to rewrite it, it is not "cheap" — it is **expensive** in human time.

Our mission is to find the models that genuinely save developer hours, not just the ones that sound smartest in a chat window.
