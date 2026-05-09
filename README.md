<p align="center">
  <img src="https://img.shields.io/badge/status-Round%201%20Live-brightgreen?style=for-the-badge" alt="Status: Round 1 Live">
  <img src="https://img.shields.io/badge/models-24%20competing-blue?style=for-the-badge" alt="24 Models">
  <img src="https://img.shields.io/badge/license-MIT-yellow?style=for-the-badge" alt="License: MIT">
  <img src="https://img.shields.io/badge/coverage-Python%20%7C%20FastAPI-purple?style=for-the-badge" alt="Coverage">
</p>

<h1 align="center">
  <br>
  🏆 cheap-llm-coding-arena
  <br>
</h1>

<h3 align="center">
  The Open Tournament for Cheap & Free LLMs on Real Code
</h3>

<p align="center">
  <b>Same arena. Same prompts. No excuses.</b><br>
  No benchmarks. No cherry-picking. Just raw code, real bugs, and honest scores.
</p>

<p align="center">
  <a href="#tournament-philosophy">Philosophy</a> •
  <a href="#brackets">Brackets</a> •
  <a href="#leaderboards">Leaderboards</a> •
  <a href="#match-format">Match Format</a> •
  <a href="#evaluation-methodology">Methodology</a> •
  <a href="#transparency">Transparency</a> •
  <a href="#roadmap">Roadmap</a>
</p>

---

## 📌 Table of Contents

- [Why This Project Exists](#why-this-project-exists)
- [Tournament Philosophy](#tournament-philosophy)
- [Current Brackets](#current-brackets)
  - [Free Bracket](#-free-bracket)
  - [Cheap / Medium Bracket](#-cheap--medium-bracket)
- [Match Format](#match-format)
- [Leaderboards](#leaderboards)
- [Evaluation Methodology](#evaluation-methodology)
- [Scoring System](#scoring-system)
- [Transparency & Reproducibility](#transparency--reproducibility)
- [Models](#models)
- [Tasks](#tasks)
- [Results & Match History](#results--match-history)
- [Hall of Shame](#hall-of-shame)
- [Roadmap & Future Plans](#roadmap--future-plans)
- [Community & Contributing](#community--contributing)

---

## Why This Project Exists

> *Most LLM leaderboards measure trivia, math puzzles, or synthetic benchmarks. We measure **code**.*

The goal of **cheap-llm-coding-arena** is to answer one practical question:

> **Which cheap (or free) LLM can you actually trust to ship production code?**

Every day, developers paste task descriptions into LLMs and copy the output into their codebases. Some models produce gold. Others produce silently broken code that passes review but crashes in production. This tournament tells you which is which — with **verifiable evidence**, not vibes.

We run every model through identical real-world coding scenarios — bug fixes, feature additions, test writing, and refactoring — then score them on **correctness, safety, and quality**.

- No human edits.
- No prompt engineering tricks.
- No hidden context.
- Just raw outputs evaluated honestly.

If you are a solo developer, a startup on a budget, or just curious whether that free-tier model can handle your next PR, this repo is for you.

---

## Tournament Philosophy

> **We do not optimize for vibes. We optimize for shipping.**

This tournament mimics the real developer workflow:

1. You have a Jira ticket.
2. You paste it into an LLM.
3. You copy the output into a PR.
4. A reviewer (or CI) checks it.

If the model's output requires a senior engineer to rewrite it, it is not "cheap" — it is **expensive** in human time. Our mission is to find models that genuinely reduce time-to-merge, not just the ones that sound smartest in a chat window.

### What We Value

| Principle | What It Means |
|-----------|---------------|
| 🔬 **Evidence over claims** | Every score is backed by raw outputs and written justification |
| ⚖️ **Fairness over convenience** | Same prompts, same context, same evaluation for every model |
| 🏗️ **Production over theory** | Real bugs, real codebases, real merge conflicts |
| 🌍 **Open over closed** | Prompts, outputs, and scorecards are public |

---

## Current Brackets

### 🆓 Free Bracket

Models that are **100% free** — no credit card, no trial tokens, no hidden costs.

| # | Model | Provider | Access | Status |
|---|-------|----------|--------|--------|
| 1 | **Owl Alpha** | Owl | Free tier | 🔄 Queued |
| 2 | **Qwen3 Coder 480B A35B** | Alibaba Cloud | Free tier | 🔄 Queued |
| 3 | **Poolside Laguna M.1** | Poolside | Free preview | 🔄 Queued |
| 4 | **Google Gemma 4 31B** | Google | Free | 🔄 Queued |
| 5 | **NVIDIA Nemotron 3 Super** | NVIDIA | Free | 🔄 Queued |
| 6 | **NVIDIA Nemotron 3 Nano 30B A3B** | NVIDIA | Free | 🔄 Queued |
| 7 | **OpenAI gpt-oss-120b** | OpenAI | Free tier | 🔄 Queued |
| 8 | **Z.ai GLM 4.5 Air** | Z.ai | Free tier | 🔄 Queued |

📄 [Full specs →](models.md#free-bracket) | 🏆 [Leaderboard →](leaderboards/free.md)

### 💰 Cheap / Medium Bracket

Models priced within **input ≤ $1.00 / 1M tokens** and **output ≤ $3.00 / 1M tokens**.

| # | Model | Provider | Input | Output | Status |
|---|-------|----------|-------|--------|--------|
| 1 | **OpenAI gpt-oss-120b** | OpenAI | Paid | Paid | 🔄 Queued |
| 2 | **Qwen3 Coder 30B A3B** | Alibaba Cloud | Paid | Paid | 🔄 Queued |
| 3 | **Tencent Hy3 Preview** | Tencent | Paid | Paid | 🔄 Queued |
| 4 | **Z.ai GLM 4.7 Flash** | Z.ai | Paid | Paid | 🔄 Queued |
| 5 | **Z.ai GLM 4.7** | Z.ai | Paid | Paid | 🔄 Queued |
| 6 | **NVIDIA Nemotron 3 Super** | NVIDIA | Paid | Paid | 🔄 Queued |
| 7 | **Mistral Small 4** | Mistral AI | Paid | Paid | 🔄 Queued |
| 8 | **OpenAI GPT-5 Mini** | OpenAI | Paid | Paid | 🔄 Queued |
| 9 | **MiniMax M2.7** | MiniMax | Paid | Paid | 🔄 Queued |
| 10 | **DeepSeek V4 Pro** | DeepSeek | Paid | Paid | 🔄 Queued |
| 11 | **Xiaomi MiMo-V2.5-Pro** | Xiaomi | Paid | Paid | 🔄 Queued |
| 12 | **xAI Grok 4.1 Fast** | xAI | Paid | Paid | 🔄 Queued |
| 13 | **Google Gemini 2.5 Flash** | Google | Paid | Paid | 🔄 Queued |
| 14 | **xAI Grok 4.3** | xAI | Paid | Paid | 🔄 Queued |
| 15 | **StepFun Step 3.5 Flash** | StepFun | Paid | Paid | 🔄 Queued |
| 16 | **MoonshotAI Kimi K2.5** | MoonshotAI | Paid | Paid | 🔄 Queued |

📄 [Full specs →](models.md#cheapmedium-bracket) | 🏆 [Leaderboard →](leaderboards/cheap_medium.md)

---

## Match Format

Each **match** is a 4-task gauntlet. Every model faces the exact same challenges:

```text
┌─────────────────────────────────────────────────────────┐
│  MATCH FORMAT — Round 1                                 │
├─────────────────────────────────────────────────────────┤
│  Task 1: Bug Fix          (10 min limit)  →  /35 pts  │
│  Task 2: Feature Add      (15 min limit)  →  /35 pts  │
│  Task 3: Test Writing     (10 min limit)  →  /35 pts  │
│  Task 4: Refactor         (15 min limit)  →  /35 pts  │
├─────────────────────────────────────────────────────────┤
│  FINAL SCORE = Average across 4 tasks                   │
│  TAG    = Assigned by average score bracket             │
└─────────────────────────────────────────────────────────┘
```

### Match Rules

| Rule | Detail |
|------|--------|
| 🔒 **Frozen Prompts** | Every model receives the identical prompt, system message, and context |
| 🚫 **No Retries** | First completion only. No "please fix this" follow-ups |
| ✂️ **No Editing** | Raw outputs are scored as-is. No human bugfixes behind the scenes |
| 📤 **Public Outputs** | All model responses are stored in `model_outputs/` for verification |
| 📝 **Written Justification** | Every score includes a written explanation |

📄 [Read the full rules →](rules.md) | 📄 [Evaluation rules →](evaluation/evaluation_rules.md)

---

## Leaderboards

### 🆓 Free Bracket — Current Standings

| Rank | Model | Avg Score / 35 | Tag | Trend |
|------|-------|----------------|-----|-------|
| 🥇 | — | — | — | ➖ |
| 🥈 | — | — | — | ➖ |
| 🥉 | — | — | — | ➖ |
| 4-8 | — | — | — | ➖ |

📊 [Full leaderboard →](leaderboards/free.md)

### 💰 Cheap / Medium Bracket — Current Standings

| Rank | Model | Avg Score / 35 | Tag | Trend |
|------|-------|----------------|-----|-------|
| 🥇 | — | — | — | ➖ |
| 🥈 | — | — | — | ➖ |
| 🥉 | — | — | — | ➖ |
| 4-16 | — | — | — | ➖ |

📊 [Full leaderboard →](leaderboards/cheap_medium.md)

> 🔔 **Round 1 is in progress.** Leaderboards will be populated as matches are completed.

---

## Evaluation Methodology

### How a Match Runs

1. **Prompt Preparation**  
   The exact same prompt is constructed for all models. It includes:
   - System message
   - Task description
   - Codebase context
   - Constraints (e.g., "no new dependencies")

2. **Model Execution**  
   Each model generates a single completion. Temperature is fixed at `0.2`.

3. **Output Extraction**  
   Code blocks are extracted automatically. No human rewrites.

4. **Runtime Validation**  
   The code is injected into the original codebase and tested:
   - Syntax check
   - Test pass/fail
   - Regression detection

5. **Scoring**  
   Two independent reviewers score each output against a fixed rubric. Discrepancies > 3 points trigger a third reviewer.

6. **Publication**  
   Scorecards, raw outputs, and reviewer notes are published in the repo.

📄 [Full methodology →](methodology.md) | 📄 [Scoring guide →](evaluation/scoring_guide.md) | 📄 [Judging process →](evaluation/judging_process.md)

---

## Scoring System

**Maximum score per task:** `35 points`

| Category | Points | What It Measures |
|----------|--------|------------------|
| ✅ Correctness | 10 | Does the code solve the stated problem? |
| 🛡️ Regression Safety | 5 | Does it break anything that previously worked? |
| 🧠 Context Understanding | 5 | Does it respect existing patterns and architecture? |
| 🎨 Code Quality | 5 | Is it clean, idiomatic, and maintainable? |
| 🧪 Tests / Edge Cases | 5 | Does it include tests or handle edge cases? |
| ⚡ Speed / Stability | 3 | Was the response fast and complete? |
| 🔧 Manual Fixes Needed | 2 | How much human intervention was required? |

### Viability Tags

| Tag | Avg Score | Meaning |
|-----|-----------|---------|
| 🟢 **Junior-dev viable** | ≥ 28 / 35 | Safe to assign with minimal review |
| 🟡 **Viable with supervision** | 20-27 / 35 | Needs senior eye before merge |
| 🟠 **Task-specific only** | 12-19 / 35 | Works for some tasks, fails on others |
| 🔴 **Not viable** | < 12 / 35 | Unreliable for production use |

📄 [Detailed rubric →](scoring.md) | 📄 [Scoring guide →](evaluation/scoring_guide.md)

---

## Transparency & Reproducibility

> **"Your test was unfair."** — We heard you before you said it.

This project is designed to withstand skepticism. Here is how:

| Concern | Our Answer |
|---------|------------|
| *"Prompts were different"* | 🔒 All prompts are frozen and published in [`prompts/`](prompts/). Identical for every model. |
| *"Answers were edited"* | ✂️ Zero human edits. Raw outputs live in [`model_outputs/`](model_outputs/). |
| *"Scores are subjective"* | 📝 Every score includes written justification. Two reviewers + third-party adjudication. |
| *"Provider settings varied"* | ⚙️ We document temperature, top-p, and max tokens for every run. Same settings when possible. |
| *"You only ran once"* | 🔄 Single-shot is intentional — it simulates real developer workflows. No retries unless the rules explicitly allow them. |
| *"I can't verify"* | 🌍 All prompts, outputs, and scorecards are in this repo. Clone it and check yourself. |

### Reproducibility Checklist

- [x] Prompts versioned and frozen
- [x] Codebase snapshots pinned by commit hash
- [x] Model versions recorded at test time
- [x] Raw outputs stored publicly
- [x] Scores include written justification
- [x] Evaluation scripts open-sourced (coming in Round 2)

---

## Models

Complete model tables with pricing, context windows, architecture details, and provider links.

📄 [models.md](models.md)

---

## Tasks

| # | Task | Type | Description | Difficulty |
|---|------|------|-------------|------------|
| 1 | [Bug Fix](tasks/01_bugfix.md) | 🐛 Bug Fix | Fix a `KeyError` in a FastAPI serializer when `profile` is missing | Medium |
| 2 | [Feature](tasks/02_feature.md) | ✨ Feature Add | Implement `POST /projects` with auth, validation, and conflict handling | Medium |
| 3 | [Tests](tasks/03_tests.md) | 🧪 Tests | Write comprehensive `pytest` tests for a pagination utility | Medium |
| 4 | [Refactor](tasks/04_refactor.md) | 🔧 Refactor | Convert a callback-hell async module to `async/await` + type hints | Medium-Hard |

📄 [Browse all tasks →](tasks/) | 📄 [Browse all prompts →](prompts/)

---

## Results & Match History

### 🆓 Free Bracket

📊 [Round 1 Results →](results/free/round_1.md)  
⚔️ [Match History →](match_history/free/)

### 💰 Cheap / Medium Bracket

📊 [Round 1 Results →](results/cheap_medium/round_1.md)  
⚔️ [Match History →](match_history/cheap_medium/)

---

## Hall of Shame

Even the best models have bad days. We document catastrophic failures honestly — because developers deserve to know what can go wrong.

📄 [View the Hall of Shame →](hall_of_shame/catastrophic_failures.md)

> *Coming after Round 1 results are published.*

---

## Roadmap & Future Plans

### Current Round

```text
[✅] Tournament design & rules
[✅] Model list & brackets defined
[✅] Scoring rubric & evaluation process finalized
[✅] Task descriptions & prompts drafted
[🔄] Round 1 execution & scoring (in progress)
[⬜] Publish Round 1 leaderboards & scorecards
[⬜] Publish match history & head-to-head comparisons
[⬜] Publish Hall of Shame
```

### Future Rounds

| Round | Theme | Planned Additions |
|-------|-------|-------------------|
| **Round 2** | Larger codebases | 3,000+ LOC tasks, multi-file refactors |
| **Round 3** | Language expansion | TypeScript, Go, Rust tasks |
| **Round 4** | CI/CD & DevOps | Dockerfile fixes, GitHub Actions, Terraform |
| **Round 5** | Security | Vulnerability patching, SQL injection fixes |

### Community Wishes

- [ ] Community vote on next task types
- [ ] Open-source evaluation runner (`run_round.sh`)
- [ ] Automated leaderboard updates via GitHub Actions
- [ ] "Dark Horse" feature — hidden models tested blindly

---

## Community & Contributing

We welcome contributions that make this benchmark more transparent, fair, and useful.

### How to Contribute

| Type | How |
|------|-----|
| 🐛 Report a bug | Open an issue with the label `bug` |
| 💡 Suggest a model | Open an issue with the label `model-request` |
| 📝 Propose a task | Open an issue with the label `task-proposal` |
| 🔍 Review a scorecard | Open a PR with corrections and evidence |
| 🏗️ Improve evaluation scripts | Open a PR — we love automation |

### Contribution Workflow

1. Fork the repo
2. Create your feature branch (`git checkout -b feature/amazing-idea`)
3. Commit your changes (`git commit -m 'Add some amazing idea'`)
4. Push to the branch (`git push origin feature/amazing-idea`)
5. Open a Pull Request

### Code of Conduct

- Be respectful. We are all here to learn.
- Back claims with evidence. Vibes are not arguments.
- Celebrate good models. Criticize bad outputs, not people.

---

## License

This project is licensed under the [MIT License](LICENSE).

---

<p align="center">
  <sub>Built with ⚡ by developers who are tired of overpriced AI hype.</sub><br>
  <sub>Star ⭐ this repo if you want more rounds.</sub>
</p>
