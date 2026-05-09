<p align="center">
  <img src="https://img.shields.io/badge/status-active-brightgreen?style=flat-square" alt="Status: Active">
  <img src="https://img.shields.io/badge/license-MIT-blue?style=flat-square" alt="License: MIT">
  <img src="https://img.shields.io/badge/tournament-Round%201-orange?style=flat-square" alt="Tournament: Round 1">
</p>

<h1 align="center">🏆 cheap-llm-coding-arena</h1>

<p align="center">
  <b>A public tournament comparing cheap & free LLMs on real coding tasks.</b><br>
  No benchmarks. No cherry-picking. Just raw code, real bugs, and honest scores.
</p>

---

## 📌 Table of Contents

- [Why This Project Exists](#why-this-project-exists)
- [Tournament Structure](#tournament-structure)
  - [Free Bracket](#-free-bracket)
  - [Cheap / Medium Bracket](#-cheap--medium-bracket)
- [Scoring System](#scoring-system)
- [Methodology](#methodology)
- [Models](#models)
- [Tasks](#tasks)
- [Results](#results)
- [Roadmap](#roadmap)
- [Contributing](#contributing)

---

## Why This Project Exists

> *Most LLM leaderboards measure trivia, math puzzles, or synthetic benchmarks. We measure **code**.*

The goal of **cheap-llm-coding-arena** is to answer a practical question:

> **Which cheap (or free) LLM can you actually trust to ship production code?**

We run every model through the same real-world coding scenarios — bug fixes, feature additions, test writing, and refactoring — then score them on **correctness, safety, and quality**. No human edits. No prompt engineering tricks. Just raw outputs evaluated honestly.

If you are a solo developer, a startup on a budget, or just curious whether that free tier model can handle your next PR, this repo is for you.

---

## Tournament Structure

### 🆓 Free Bracket

Models that are **100% free** at the time of testing. No credit card required. No trial tokens.

| Model | Provider | Access |
|---|---|---|
| Owl Alpha | Owl | Free tier |
| Qwen3 Coder 480B A35B | Alibaba Cloud | Free tier |
| Poolside Laguna M.1 | Poolside | Free preview |
| Google Gemma 4 31B | Google | Free |
| NVIDIA Nemotron 3 Super | NVIDIA | Free |
| NVIDIA Nemotron 3 Nano 30B A3B | NVIDIA | Free |
| OpenAI gpt-oss-120b | OpenAI | Free tier |
| Z.ai GLM 4.5 Air | Z.ai | Free tier |

📄 [Full free bracket specs →](models.md#free-bracket)

### 💰 Cheap / Medium Bracket

Models priced within:
- **Input:** ≤ `$1.00` / 1M tokens
- **Output:** ≤ `$3.00` / 1M tokens

| Model | Provider | Input / 1M | Output / 1M |
|---|---|---|---|
| OpenAI gpt-oss-120b | OpenAI | Paid | Paid |
| Qwen3 Coder 30B A3B Instruct | Alibaba Cloud | Paid | Paid |
| Tencent Hy3 Preview | Tencent | Paid | Paid |
| Z.ai GLM 4.7 Flash | Z.ai | Paid | Paid |
| Z.ai GLM 4.7 | Z.ai | Paid | Paid |
| NVIDIA Nemotron 3 Super | NVIDIA | Paid | Paid |
| Mistral Small 4 | Mistral AI | Paid | Paid |
| OpenAI GPT-5 Mini | OpenAI | Paid | Paid |
| MiniMax M2.7 | MiniMax | Paid | Paid |
| DeepSeek V4 Pro | DeepSeek | Paid | Paid |
| Xiaomi MiMo-V2.5-Pro | Xiaomi | Paid | Paid |
| xAI Grok 4.1 Fast | xAI | Paid | Paid |
| Google Gemini 2.5 Flash | Google | Paid | Paid |
| xAI Grok 4.3 | xAI | Paid | Paid |
| StepFun Step 3.5 Flash | StepFun | Paid | Paid |
| MoonshotAI Kimi K2.5 | MoonshotAI | Paid | Paid |

📄 [Full cheap/medium bracket specs →](models.md#cheapmedium-bracket)

---

## Scoring System

**Maximum score:** `35 points`

| Category | Max Points | Description |
|---|---|---|
| ✅ Correctness | 10 | Does the code solve the stated problem? |
| 🛡️ Regression Safety | 5 | Does it break anything that previously worked? |
| 🧠 Context Understanding | 5 | Does it use the existing codebase patterns correctly? |
| 🎨 Code Quality | 5 | Is the code clean, idiomatic, and maintainable? |
| 🧪 Tests / Edge Cases | 5 | Does it include tests or handle edge cases? |
| ⚡ Speed / Stability | 3 | Was the response fast and without truncation? |
| 🔧 Manual Fixes Needed | 2 | How much human intervention was required? |

📄 [Detailed scoring rubric →](scoring.md)

### Tags

After scoring, each model receives a **viability tag**:

| Tag | Meaning |
|---|---|
| 🟢 `Junior-dev viable` | Safe to assign to a junior dev with minimal review |
| 🟡 `Viable with supervision` | Good output, but needs a senior eye before merge |
| 🟠 `Task-specific only` | Works for some tasks, fails on others |
| 🔴 `Not viable` | Unreliable for production use |

---

## Methodology

🔬 Every model receives the **exact same prompt** for every task.  
✂️ Outputs are **never edited** before scoring.  
📝 Evaluation is done against a fixed rubric.  
🐛 Tasks are **real bugs and features** from real (anonymized) codebases.

📄 [Read the full methodology →](methodology.md)

---

## Models

Complete model tables, pricing, context windows, and links.

📄 [models.md](models.md)

---

## Tasks

| # | Task | Description |
|---|---|---|
| 1 | [Bug Fix](tasks/01_bugfix.md) | Fix a real production bug in a Python service |
| 2 | [Feature](tasks/02_feature.md) | Implement a new REST endpoint in an existing FastAPI app |
| 3 | [Tests](tasks/03_tests.md) | Write unit tests for an untyped utility module |
| 4 | [Refactor](tasks/04_refactor.md) | Refactor a legacy callback-hell module to async/await |

📄 [Browse all tasks →](tasks/)

---

## Results

### Free Bracket

📊 [Round 1 →](results/free/round_1.md)

### Cheap / Medium Bracket

📊 [Round 1 →](results/cheap_medium/round_1.md)

---

## Roadmap

```text
[✅] Tournament design & rules
[✅] Model list & brackets defined
[✅] Scoring rubric finalized
[✅] Task descriptions drafted
[🔄] Round 1 execution & scoring (in progress)
[⬜] Publish Round 1 results
[⬜] Round 2: larger codebases (3k+ LOC)
[⬜] Add CI/CD evaluation dimension
[⬜] Community vote on next tasks
```

---

## Contributing

Found a mistake? Want to suggest a model or a task? Open an issue or PR.

1. Fork the repo
2. Create your feature branch (`git checkout -b feature/amazing-idea`)
3. Commit your changes (`git commit -m 'Add some amazing idea'`)
4. Push to the branch (`git push origin feature/amazing-idea`)
5. Open a Pull Request

---

## License

This project is licensed under the [MIT License](LICENSE).

---

<p align="center">
  <sub>Built with ⚡ by developers who are tired of overpriced AI hype.</sub>
</p>
