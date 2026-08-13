# AI Agent Book Skill

[English](./README.md) · [简体中文](./README.zh-CN.md) 


> 🌐 **[在线宣传页](https://samge0.github.io/ai-agent-book-skill/)** — 可视化了解功能特性与工作流程

> A collection of ready-to-use AI Agent skills distilled from *"Deep Understanding of AI Agents"* by Li Bojie (Bojie Li).
>
> 将李博杰所著《深入理解 AI Agent》蒸馏为一组可直接调用的 AI Agent 技能。

Read the book online: [https://bojieli.com/ai-agent-book/](https://github.com/bojieli/ai-agent-book)

---

## What Is This

This repository uses two open-source skill extraction tools to distill the **English edition** (`book-en/`, 10 chapters + introduction) of *"Deep Understanding of AI Agents"* into **1 comprehensive skill + 25 atomic methodology skills**.

Each atomic skill follows the **RIA++ structure** (Reading original quote → Interpretation → A1 past application → A2 future trigger → Execution steps → Boundary conditions), accompanied by a `test-prompts.json` test suite (with decoy tests and cross-skill confusion tests). These skills can be directly loaded by AI Agent tools such as Hermes, Claude Code, and GitHub Copilot CLI.

### Extraction Tools Used

| Tool | Repository | Output |
|------|-----------|--------|
| **[cangjie-skill](https://github.com/kangarooking/cangjie-skill)** | kangarooking/cangjie-skill | 25 atomic methodology skills (RIA-TV++ pipeline: Adler reading → parallel extraction → triple verification → RIA++ construction → Zettelkasten linking → pressure testing) |
| **[book-to-skill](https://github.com/virgiliojr94/book-to-skill)** | virgiliojr94/book-to-skill | 1 comprehensive skill (with 10 chapter files + glossary + patterns + cheatsheet) |

---

## Coverage

All **100 section-level segments** across the book's 10 chapters were individually audited:

| Status | Coverage | Description |
|--------|----------|-------------|
| ✅ Fully covered | **89%** | 89/100 sections — methodology, frameworks, and principles extracted as skills |
| ⚠️ Partially covered | 11% | 11/100 sections — API mechanics (e.g., Chat Template tokenization), frontier experiment parameters, and other non-methodology content |

Three core claims were **verified word-by-word** against the original English text:

- ✅ **Harness = Constrain + Verify + Correct** (Chapter 1)
- ✅ **SFT memorizes, RL generalizes** (Chapter 7)
- ✅ **When the score difference is smaller than the noise bandwidth, do not switch** (Chapter 6)

---

## Directory Structure

```
ai-agent-book-skill/
│
├── ai-agent-book/                    ← Comprehensive skill (book-to-skill output)
│   ├── SKILL.md                      Master: core frameworks + chapter index + topic index
│   ├── chapters/
│   │   ├── ch01-getting-started.md   Ch 1: Getting Started with AI Agents
│   │   ├── ch02-context-engineering.md Ch 2: Context Engineering
│   │   ├── ch03-memory-knowledge.md  Ch 3: User Memory and Knowledge Base
│   │   ├── ch04-tools.md             Ch 4: Tools
│   │   ├── ch05-coding-agent.md      Ch 5: Coding Agent and Code Generation
│   │   ├── ch06-evaluation.md        Ch 6: Evaluating Agents
│   │   ├── ch07-post-training.md     Ch 7: Model Post-Training
│   │   ├── ch08-self-evolution.md    Ch 8: Agent Self-Evolution
│   │   ├── ch09-multimodal.md        Ch 9: Multimodal and Real-Time Interaction
│   │   └── ch10-multi-agent.md       Ch 10: Multi-Agent Collaboration
│   ├── glossary.md                   Glossary (70+ terms)
│   ├── patterns.md                   Design patterns (19 patterns)
│   └── cheatsheet.md                 Decision cheatsheet (trade-off matrices + thresholds + if/then rules)
│
├── cangjie-distilled/                ← Ch 1-5 atomic skills (cangjie-skill output)
│   ├── BOOK_OVERVIEW.md              Adler analysis
│   ├── INDEX.md                      Skill catalog + dependency graph
│   ├── GLOSSARY.md                   Shared glossary
│   ├── DIGEST.md                     Reader-facing synthesis (~5,400 words)
│   ├── harness-engineering/          One directory per skill
│   │   ├── SKILL.md                  RIA++ six-section structure
│   │   └── test-prompts.json         Test cases (3 positive + 2 decoy + 1 edge)
│   ├── kv-cache-friendly-context/
│   └── ... (13 skills total)
│
├── cangjie-ch6-10/                   ← Ch 6-10 atomic skills (cangjie-skill output)
│   ├── BOOK_OVERVIEW_CH6_10.md
│   ├── INDEX_CH6_10.md
│   ├── GLOSSARY_CH6_10.md
│   ├── DIGEST_CH6_10.md
│   ├── sft-memorizes-rl-generalizes/
│   │   ├── SKILL.md
│   │   └── test-prompts.json
│   └── ... (12 skills total)
│
├── .gitignore
├── LICENSE                           MIT
├── README.md                         Chinese README
└── README.en.md                      This file (English)
```

---

## Extracted Skills

### Comprehensive Skill (book-to-skill output)

| Skill | Content |
|-------|---------|
| **ai-agent-book** | All 10 chapters as structured knowledge: core frameworks overview + on-demand chapter files + glossary + patterns + decision cheatsheet |

### Atomic Methodology Skills (cangjie-skill output, 25 total)

#### Chapters 1-5 (Architecture & Infrastructure)

| Skill | Title | Chapter |
|-------|-------|---------|
| `harness-engineering` | Harness Engineering: Constrain, Verify, Correct | Ch1 |
| `engineering-paradigm-evolution` | Five-Layer Engineering Paradigm Evolution | Ch1 |
| `kv-cache-friendly-context` | KV Cache-Friendly Context Design | Ch2 |
| `progressive-disclosure-skills` | Progressive Disclosure via Agent Skills | Ch2 |
| `context-compression-strategy` | Context Compression Strategy Selection | Ch2 |
| `agent-status-bar` | Agent Status Bar: Managing Trajectories with Meta-Information | Ch2 |
| `three-level-memory-evaluation` | Three-Level Memory Evaluation Framework | Ch3 |
| `agentic-rag` | Agentic RAG: From Passive Pipeline to Active Explorer | Ch3 |
| `two-tier-memory-architecture` | Two-Tier Memory Architecture | Ch3 |
| `tool-design-principles` | Tool Design Principles (Agent-Computer Interface) | Ch4 |
| `proactive-tool-discovery` | Proactive Tool Discovery | Ch4 |
| `coding-agent-meta-capability` | Coding Agent as Meta-Capability | Ch5 |
| `proposer-reviewer-pattern` | Proposer-Reviewer Pattern | Ch1/4/5 |

#### Chapters 6-10 (Evaluation, Training, Evolution, Multimodal, Multi-Agent)

| Skill | Title | Chapter |
|-------|-------|---------|
| `three-level-evaluation-system` | Three-Level Evaluation System | Ch6 |
| `model-swap-vs-ablation` | Model Swap vs. Ablation: Diagnosing the Bottleneck | Ch6 |
| `pass-at-k-vs-pass-k-k` | Pass@k vs Pass^k: Capability Ceiling vs. Reliability | Ch6 |
| `statistical-significance-eval` | Statistical Significance: Don't Switch on Noise | Ch6 |
| `sft-memorizes-rl-generalizes` | SFT Memorizes, RL Generalizes | Ch7 |
| `data-environment-over-algorithms` | Data and Environment Matter More Than Algorithms | Ch7 |
| `process-vs-outcome-reward` | Process vs. Outcome Reward Design | Ch7 |
| `verification-generation-asymmetry` | Verification-Generation Asymmetry | Ch7 |
| `externalized-learning-three-products` | Externalized Learning: Three Products | Ch8 |
| `sleep-learning-memory-consolidation` | Sleep Learning: Autonomous Evolution of User Memory | Ch8 |
| `fast-slow-thinking-separation` | Fast-Slow Thinking Separation for Real-Time Agents | Ch9 |
| `multi-agent-new-information-criterion` | Multi-Agent New Information Criterion | Ch10 |

---

## How to Use These Skills

### Option 1: Let your AI tool install automatically (Recommended)

Share the repository URL with your AI Agent (Hermes / Claude Code / GitHub Copilot CLI, etc.) and let it clone and install:

> **Repository URL: https://github.com/Samge0/ai-agent-book-skill**

Example prompt:
```
Please clone the skill repository from https://github.com/Samge0/ai-agent-book-skill,
install ai-agent-book/ as a comprehensive skill, and install the subdirectories under
cangjie-distilled/ and cangjie-ch6-10/ as individual skills.
```

### Option 2: Manual install for Hermes

```bash
# Clone the repository
git clone https://github.com/Samge0/ai-agent-book-skill.git

# Copy skill directories to Hermes skills directory
# Comprehensive skill
cp -r ai-agent-book-skill/ai-agent-book ~/.hermes/skills/

# Atomic skills (each subdirectory is an independent skill)
cp -r ai-agent-book-skill/cangjie-distilled/harness-engineering ~/.hermes/skills/
cp -r ai-agent-book-skill/cangjie-distilled/kv-cache-friendly-context ~/.hermes/skills/
# ... copy other skills as needed
```

Hermes skills directory locations:
- **Linux / macOS / WSL**: `~/.hermes/skills/` or `~/AppData/Local/hermes/skills/`
- **Windows**: `C:\Users\<username>\AppData\Local\hermes\skills\`

### Option 3: Manual install for Claude Code

```bash
git clone https://github.com/Samge0/ai-agent-book-skill.git

# Copy/link skill directories to Claude Code skills directory
cp -r ai-agent-book-skill/ai-agent-book ~/.claude/skills/
cp -r ai-agent-book-skill/cangjie-distilled/harness-engineering ~/.claude/skills/
# ... copy other skills as needed
```

### Option 4: Manual install for GitHub Copilot CLI

```bash
git clone https://github.com/Samge0/ai-agent-book-skill.git

# Copilot CLI skills directory
cp -r ai-agent-book-skill/ai-agent-book ~/.copilot/skills/
```

### Usage Examples

Once installed, your AI Agent will automatically load the relevant skill based on your question. For example:

- You ask *"My Agent keeps looping infinitely, how do I make it reliable?"* → loads `harness-engineering`
- You ask *"Should I use SFT or RL for this task?"* → loads `sft-memorizes-rl-generalizes`
- You ask *"The new model scored 3% higher, should I switch?"* → loads `statistical-significance-eval`
- You ask *"How do I make my Agent remember user preferences across sessions?"* → loads `sleep-learning-memory-consolidation`

---

## Internal Structure of Each Skill (RIA++)

Each atomic skill's `SKILL.md` follows the six-section structure below:

| Section | Meaning |
|---------|---------|
| **R — Reading** | Original quote from the book (max 100 words), with chapter citation |
| **I — Interpretation** | Methodology skeleton in the author's own words (not a copy of the text) |
| **A1 — Past Application** | Book examples: how the author (or the industry) applied this framework |
| **A2 — Future Trigger** | Trigger scenarios: when users need this skill + language signals |
| **E — Execution Steps** | Actionable steps, each with explicit completion criteria |
| **B — Boundary** | Boundary conditions: when **NOT** to use this skill |

Each skill also includes `test-prompts.json` (darwin-skill compatible format):
- 3 `should_trigger` positive cases
- 2 `should_not_trigger` decoys (at least 1 cross-skill confusion test)
- 1 `edge_case` boundary case

---

## Acknowledgments

- **Book author**: [Li Bojie (Bojie Li)](https://github.com/bojieli), Chief Scientist of Pine AI
- **Original book repository**: [bojieli/ai-agent-book](https://github.com/bojieli/ai-agent-book) (multilingual editions)
- **Extraction tool 1**: [cangjie-skill](https://github.com/kangarooking/cangjie-skill) by kangarooking — RIA-TV++ distillation pipeline
- **Extraction tool 2**: [book-to-skill](https://github.com/virgiliojr94/book-to-skill) by virgiliojr94 — document-to-skill converter

---

## License

[MIT](LICENSE)
