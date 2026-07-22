# Cheatsheet — AI Agent Book

> Decision rules, trade-off matrices, and heuristics from "Deep Understanding of AI Agents" by Li Bojie.
> This is a reasoning aid — every line helps you *decide* something.

---

## Agent Architecture Decisions

| Decision | Choose | When |
|----------|--------|------|
| Workflow vs Autonomous | **Workflow** (fixed path) | Task steps are predictable and repetitive |
| Workflow vs Autonomous | **Autonomous** (LLM decides) | Task requires dynamic planning, variable steps |
| Single vs Multi-agent | **Single** | Task fits in one context window, no parallelism needed |
| Single vs Multi-agent | **Multi** | Independent sub-tasks that can run concurrently, or need specialization |
| Context shared? | **Shared context** | Agents need to see each other's reasoning |
| Context shared? | **Non-shared context** | Agents are independent; shared context wastes tokens |

---

## Context Engineering Rules

| Rule | Guideline |
|------|-----------|
| Static content first | System prompt + tool definitions go at the TOP (cache-friendly) |
| Dynamic content last | User messages + tool results go at the BOTTOM |
| Cache breakpoint | Place `cache_control` after static prefix (~1024+ tokens) |
| Compression trigger | Compress when context exceeds 60-70% of window |
| Compression choice | **Excerpt** for facts/code, **Summary** for completed tasks, **Distillation** for old history |
| Token budget | Reserve 30% of context for the model's output + reasoning |

**KV Cache 3 Commandments**: suffix-friendly, minimal-edit, explicit-cache.

---

## Tool Design Decisions

| Dimension | Question | Guideline |
|-----------|----------|-----------|
| Granularity | One big tool vs many small? | Prefer small, composable tools — easier to describe and select |
| Generality | Specific vs general? | General tools reduce count but increase description complexity |
| Description | How detailed? | Include: what it does, when to use, input format, expected output |
| ACI | Agent-Computer Interface | Optimize for the *model's* understanding, not human readability |

**Proposer-Reviewer**: Use for high-stakes outputs. Skip for simple tasks.

**Sidecar mechanism**: When an agent needs long-running processes, run them in a sidecar process and communicate via API — don't block the main loop.

---

## Evaluation Decision Tree

```
Need to evaluate an agent?
├── Is there a deterministic ground truth?
│   ├── YES → Use exact match / programmatic check
│   └── NO → Use LLM-as-Judge (pairwise comparison preferred)
├── Is the task stochastic?
│   ├── YES → Run k samples, report Pass@k AND Pass^k
│   └── NO → Single run is sufficient
├── Need to find the bottleneck?
│   └── Run model swap experiment + ablation matrix
└── Need production monitoring?
    └── Build observability: log every step, tool call, token count
```

**Rule of thumb**: If you can't measure it, you can't improve it. Build the eval harness BEFORE optimizing.

---

## Post-Training Method Selection

| Goal | Method | Why |
|------|--------|-----|
| Teach a new format/style | **SFT** | SFT memorizes patterns efficiently |
| Improve capability/reasoning | **RL** | RL generalizes to unseen situations |
| Align with human preferences | **RLHF** | Reward model captures human judgment |
| Tool-calling ability | **RL with tool environments** | Practice in realistic tool-use scenarios |
| Reduce inference cost | **Distillation** | Transfer capability to smaller model |

**SFT vs RL**: SFT is the "what", RL is the "how well". Use SFT first to teach format, then RL to optimize outcomes.

**Process vs Outcome rewards**: Process rewards (per-step) are better for long reasoning chains. Outcome rewards (final answer only) are cheaper but harder to credit-assign.

---

## Memory & Knowledge Base

| Need | Storage Format | Retrieval |
|------|---------------|-----------|
| User preferences/facts | Structured JSON cards | Exact lookup |
| Document chunks | Vector embeddings | Semantic search (dense + sparse hybrid) |
| Conversation history | Raw log + periodic summary | Recency + relevance |
| Procedural knowledge | Code tool / script | Invoke directly |
| Methodology | SKILL.md | Trigger-based loading |

**Three-level memory evaluation**: 1) Can it recall? 2) Does it evolve? 3) Is it redundant?

**Contextual Retrieval rule**: Always prepend chunk context before embedding. One-time indexing cost, permanent precision gain.

---

## Multi-Agent Topology Selection

| Topology | Use When | Failure Mode |
|----------|----------|--------------|
| **Peer-to-peer** | Equal agents, shared goal | Concurrency conflicts, duplicated work |
| **Manager-worker** | Clear division of labor, orchestration needed | Manager becomes bottleneck |
| **Decentralized** | No central control, emergent behavior | Error cascading, hard to debug |

**When multi-agent is NOT better**: If a single agent can do it within one context window, multi-agent adds cost and coordination overhead without benefit.

**Cost warning**: Multi-agent cost scales with (agents × turns × context). Monitor for cost explosion.

---

## Security Thresholds

| Risk | Safeguard |
|------|-----------|
| Code execution | Three-tier: NL rules → checklist → server-side sandbox |
| Prompt injection | Sanitize tool outputs, use separate context for untrusted content |
| Privilege escalation | Run agent with least privilege; block network access in sandbox |
| Data exfiltration | Log all tool calls; block writes to external endpoints |

**Lethal Triad**: Agent + code execution + network access = maximum risk. Never enable all three without a sandbox.

---

## Voice/Multimodal Architecture

| Paradigm | Latency | Quality | Complexity |
|----------|---------|---------|------------|
| **Cascaded** (STT→LLM→TTS) | High | Good | Low |
| **Omni** (end-to-end model) | Medium | Best | Medium |
| **Full-Duplex** (interactive) | Lowest | Good | Highest |

**Fast-slow thinking**: Separate cheap/fast model for routine turns from expensive/slow model for complex reasoning. Route based on complexity detection.

---

## Quick Decision Rules

- **"Should I add RAG?"** → Only if the agent needs information not in its training data AND the information changes frequently.
- **"Should I compress context?"** → If context > 60% of window, or if you observe quality degradation on long conversations.
- **"SFT or RL?"** → SFT to teach format, RL to improve capability. Almost always: SFT first, then RL.
- **"How many skills?"** → Start with 3-5 core skills. Each skill should have a clear, non-overlapping trigger condition.
- **"Shared or non-shared context?"** → Shared if agents need each other's reasoning. Non-shared if they're independent (saves tokens).
- **"Eval before or after building?"** → BEFORE. If you can't measure success, you can't know if you've achieved it.
