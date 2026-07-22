---
name: ai-agent-book
description: Structured knowledge from "Deep Understanding of AI Agents" by Li Bojie — frameworks for building production-grade LLM Agent systems (Harness Engineering, Context Engineering, ReAct loop, post-training, evaluation, self-evolution, multi-agent collaboration).
version: 1.0.0
---

# AI Agent Book Skill

> Source: "Deep Understanding of AI Agents" by Li Bojie (Chief Scientist of Pine AI), 2025-2026. English translation.

## How to Use This Skill

This skill distills a 10-chapter technical book into actionable frameworks for building production-grade AI Agent systems. Read SKILL.md for the mental model, then dive into specific chapters/ files when facing a concrete problem.

**For quick decisions:** Check cheatsheet.md for trade-off matrices and if/then rules.
**For implementation patterns:** Check patterns.md for concrete techniques.
**For terminology:** Check glossary.md for term definitions with chapter references.

**Voice convention:** This skill uses practitioner voice — "Use X when Y", not "The book explains X."

## Core Frameworks and Mental Models

### 1. The Agent Formula

**Agent = LLM + Context + Tools**

At the production level: **Agent = Model + Harness**, where Harness = [Context + Tools + Constrain + Verify + Correct]

- **LLM** = reasoning engine (policy in RL terms)
- **Context** = working set of information at each decision point (observation space)
- **Tools** = action interfaces (action space)
- **Constrain** = what the Agent may and may not do
- **Verify** = whether it did the thing correctly
- **Correct** = how to recover when it did not

**Harness Engineering is the competitive advantage.** As models commoditize, the engineering outside the model — constrain, verify, correct — determines who wins. In production Agent systems, the vast majority of code goes into these safeguards, not into context and tools alone.

### 2. The ReAct Loop

The core execution loop: **Reason -> Act -> Observe -> repeat until done.**

Each LLM call receives: static prefix (system prompt + tool definitions) + trajectory (message history).

The trajectory is cumulative — every call appends results, giving the model a global view. The trajectory is structured (user messages, assistant reasoning + tool_calls, tool results), making it interpretable and debuggable.

### 3. Context Engineering

Context sets the ceiling of Agent capability — a moderately capable model with well-organized context outperforms a stronger model with insufficient context.

**Three KV Cache commandments:**
1. Once system prompt and tool definitions are finalized, do NOT change them.
2. Always append dynamic information to the END of context, never modify the prefix.
3. Use standard API message format — never manually concatenate messages.

**Context is more retrieval than reasoning.** Attention excels at lookup but cannot do inductive statistics in a single forward pass. This means: pre-compute conclusions (Agent Status Bar), compress raw records into distilled knowledge, and use explicit code for counting/aggregation.

### 4. The Five Engineering Paradigm Layers

Software Engineering > Prompt Engineering > Context Engineering > Harness Engineering > Loop Engineering

Each layer widens scope: from single prompts, to full context management, to system-level safeguards, to sustained autonomous operation across runs.

### 5. Three Learning Paradigms

| Paradigm | Cost | Speed | Persistence | Chapter |
|----------|------|-------|-------------|---------|
| **Post-training** (RL/SFT into weights) | High | Months | Permanent | Ch 7 |
| **In-Context Learning** (prompt examples) | Low | Instant | Session-only | Ch 2 |
| **Externalized Learning** (knowledge bases, tools, Skills) | Medium | Hours | Persistent + interpretable | Ch 8 |

### 6. SFT Memorizes, RL Generalizes

- **SFT** optimizes for "how much it looks like the standard answer" -> memorizes fixed mappings, fails under distribution shift. Ceiling = quality of demonstrator data.
- **RL** optimizes for "how good the result is" -> learns transferable strategies, discovers novel solutions. Ceiling = the task itself (what rewards can recognize).

**Rule: lay the foundation with SFT, then climb with RL.** SFT establishes "form" (format, structure), RL pursues "spirit" (strategy, generalization).

## Chapter Index

| Ch | File | Title | Key Framework |
|----|------|-------|---------------|
| 01 | ch01-getting-started | Getting Started with AI Agents | Agent formula, ReAct loop, Harness Engineering, 5-layer paradigm evolution |
| 02 | ch02-context-engineering | Context Engineering | KV Cache design, prompt engineering, Agent Status Bar, compression strategies |
| 03 | ch03-memory-knowledge | User Memory and Knowledge Base | Three-level memory framework, four storage formats, RAG pipeline, Contextual Retrieval |
| 04 | ch04-tools | Tools | Five tool categories, ACI design, Proposer-Reviewer, Sidecar, event-driven architecture |
| 05 | ch05-coding-agent | Coding Agent and Code Generation | Seven core tools, code as meta-capability, three-tier safeguard, Proposer-Reviewer iteration |
| 06 | ch06-evaluation | Evaluating Agents | Three-level evaluation system, LLM-as-Judge, model swap experiment, ablation infrastructure |
| 07 | ch07-post-training | Model Post-Training | Three-stage pipeline (Pretrain->SFT->RL), SFT vs RL comparison, reward design |
| 08 | ch08-self-evolution | Agent Self-Evolution | Externalized learning, three products, tool creation, sleep consolidation |
| 09 | ch09-multimodal | Multimodal and Real-Time Interaction | Three voice paradigms, fast-slow thinking, Computer Use, VLA robotics |
| 10 | ch10-multi-agent | Multi-Agent Collaboration | Two-dimension classification (context x topology), three topologies, failure modes |

## Topic Index

- **Architecture:** Agent formula (Ch1), Harness Engineering (Ch1), event-driven async (Ch4), coding core (Ch5), multi-agent topology (Ch10)
- **Context:** KV Cache (Ch2), prompt engineering (Ch2), Agent Status Bar (Ch2), compression (Ch2), Skills (Ch2)
- **Memory and Knowledge:** User memory formats (Ch3), RAG pipeline (Ch3), Contextual Retrieval (Ch3), Agentic RAG (Ch3)
- **Tools:** Five categories (Ch4), MCP protocol (Ch4), ACI (Ch4), Proactive Tool Discovery (Ch4)
- **Code:** Seven core tools (Ch5), code as thinking tool (Ch5), Proposer-Reviewer (Ch5)
- **Evaluation:** Evaluation environment (Ch6), datasets (Ch6), LLM-as-Judge (Ch6), observability (Ch6)
- **Training:** Pre-training (Ch7), SFT (Ch7), RL (Ch7), RLHF (Ch7), reward design (Ch7)
- **Evolution:** Externalized learning (Ch8), Skills creation (Ch8), tool creation (Ch8), Voyager (Ch8)
- **Multimodal:** Voice pipeline (Ch9), Computer Use (Ch9), VLA robotics (Ch9)
- **Security:** Prompt injection (Ch2,4), Lethal Triad (Ch5), Sidecar (Ch4), sandbox isolation (Ch4,5)

## Supporting Files

- [glossary.md](glossary.md) — Key terms alphabetically, with chapter references
- [patterns.md](patterns.md) — Concrete design patterns and algorithms
- [cheatsheet.md](cheatsheet.md) — Decision rules, trade-off matrices, thresholds

## Scope and Limits

**What this skill covers:** Production-grade AI Agent system design — architecture, context engineering, tool design, evaluation methodology, model post-training, self-evolution, multimodal interaction, and multi-agent collaboration.

**What it does NOT cover:**
- Specific model API details (these change too fast — consult provider docs)
- Deployment infrastructure (GPU provisioning, k8s, etc.)
- Regulatory compliance specifics by jurisdiction
- The full mathematical derivations of RL algorithms (Ch7 is summarized at the decision level)

**Conventions:** Exact author terminology is preserved (e.g., "Harness Engineering", "Context Engineering", "KV Cache-Friendly Context Design", "Agent Status Bar", "Progressive Disclosure"). Star ratings indicate depth/difficulty of experiments in the original text.
