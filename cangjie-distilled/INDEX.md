# INDEX: Atomic Methodology Skills

**Source:** Deep Understanding of AI Agents -- Li Bojie, Chapters 1-5
**Pipeline:** RIA-TV++ (Reading-Interpretation-Application + Triple Verification)
**Total Skills:** 12

---

## Skills by Theme

### Theme 1: Agent Architecture and Engineering Paradigm (Chapter 1)

| # | Skill | Description | Priority |
|---|-------|-------------|----------|
| 1 | [harness-engineering](harness-engineering/SKILL.md) | Constrain/Verify/Correct -- production reliability layers | P1 |
| 2 | [engineering-paradigm-evolution](engineering-paradigm-evolution/SKILL.md) | 5-layer nested engineering: Software > Prompt > Context > Harness > Loop | P2 |

### Theme 2: Context Engineering (Chapter 2)

| # | Skill | Description | Priority |
|---|-------|-------------|----------|
| 3 | [kv-cache-friendly-context](kv-cache-friendly-context/SKILL.md) | Static prefix / dynamic suffix context layout for cost and latency | P1 |
| 4 | [progressive-disclosure-skills](progressive-disclosure-skills/SKILL.md) | Load instructions only when context demands (Skills mechanism) | P1 |
| 5 | [context-compression-strategy](context-compression-strategy/SKILL.md) | 5-layer hierarchical compression: budget > noise deletion > micro > archival > full | P1 |

### Theme 3: Memory and Knowledge (Chapter 3)

| # | Skill | Description | Priority |
|---|-------|-------------|----------|
| 6 | [three-level-memory-evaluation](three-level-memory-evaluation/SKILL.md) | Basic Recall > Multi-Session Retrieval > Proactive Service | P1 |
| 7 | [agentic-rag](agentic-rag/SKILL.md) | Agent-driven iterative retrieval replacing passive pipeline | P1 |
| 8 | [two-tier-memory-architecture](two-tier-memory-architecture/SKILL.md) | Resident structured facts + on-demand contextual retrieval | P1 |

### Theme 4: Tools (Chapter 4)

| # | Skill | Description | Priority |
|---|-------|-------------|----------|
| 9 | [tool-design-principles](tool-design-principles/SKILL.md) | ACI: granularity, generality, description art, parameter fidelity | P1 |
| 10 | [proactive-tool-discovery](proactive-tool-discovery/SKILL.md) | Dynamic tool loading at scale (MCP-Zero / Skills approaches) | P2 |

### Theme 5: Coding Agent (Chapter 5)

| # | Skill | Description | Priority |
|---|-------|-------------|----------|
| 11 | [coding-agent-meta-capability](coding-agent-meta-capability/SKILL.md) | Code generation as the core of general-purpose Agents | P1 |
| 12 | [proposer-reviewer-pattern](proposer-reviewer-pattern/SKILL.md) | Verification -- not model feeling -- decides task completion | P1 |

---

## Skill Relationship Graph

**depends-on** (A depends on B means B is prerequisite), **composes-with** (synergy), **contrasts-with** (different angles on similar problems):

**Depends-on relationships:**
- engineering-paradigm-evolution depends-on harness-engineering
- progressive-disclosure-skills depends-on kv-cache-friendly-context
- context-compression-strategy depends-on kv-cache-friendly-context
- two-tier-memory-architecture depends-on three-level-memory-evaluation
- agentic-rag depends-on three-level-memory-evaluation
- two-tier-memory-architecture depends-on agentic-rag
- tool-design-principles depends-on harness-engineering
- proactive-tool-discovery depends-on tool-design-principles
- proactive-tool-discovery depends-on kv-cache-friendly-context
- coding-agent-meta-capability depends-on tool-design-principles
- coding-agent-meta-capability depends-on harness-engineering
- proposer-reviewer-pattern depends-on harness-engineering

**Composes-with relationships:**
- progressive-disclosure-skills composes-with proactive-tool-discovery
- agentic-rag composes-with two-tier-memory-architecture
- coding-agent-meta-capability composes-with engineering-paradigm-evolution
- coding-agent-meta-capability composes-with proposer-reviewer-pattern

**Contrasts-with relationships:**
- harness-engineering contrasts-with engineering-paradigm-evolution
- kv-cache-friendly-context contrasts-with context-compression-strategy
- two-tier-memory-architecture contrasts-with context-compression-strategy

---

## Recommended Learning Order

Based on the book structure and dependency relationships:

### Phase 1: Foundations (Chapter 1)
1. **harness-engineering** -- The mental model for all production Agent work
2. **engineering-paradigm-evolution** -- When to use which level of engineering

### Phase 2: Context Engineering (Chapter 2)
3. **kv-cache-friendly-context** -- How to structure context for cost/latency
4. **progressive-disclosure-skills** -- How to modularize capabilities
5. **context-compression-strategy** -- How to manage growing context

### Phase 3: Memory and Knowledge (Chapter 3)
6. **three-level-memory-evaluation** -- What makes memory good
7. **agentic-rag** -- How to retrieve intelligently
8. **two-tier-memory-architecture** -- How to build proactive memory

### Phase 4: Tools (Chapter 4)
9. **tool-design-principles** -- How to design Agent-friendly tools
10. **proactive-tool-discovery** -- How to handle tool scale

### Phase 5: Coding Agent (Chapter 5)
11. **coding-agent-meta-capability** -- The unifying architecture
12. **proposer-reviewer-pattern** -- Verification for reliable completion

---

## Triple Verification Summary

| Skill | V1: Cross-domain | V2: Predictive power | V3: Uniqueness | Status |
|-------|------------------|---------------------|----------------|--------|
| harness-engineering | Supported across Ch1,2,4,5 | Answers how to make any Agent reliable | Not common sense -- specific 5-layer framework | PASS |
| engineering-paradigm-evolution | Ch1 + throughout | Predicts which architecture to choose | Nested-layer model is specific | PASS |
| kv-cache-friendly-context | Ch2 + Ch4 | Predicts cost/latency impact of context changes | Specific to LLM inference mechanics | PASS |
| progressive-disclosure-skills | Ch2 + Ch4 | Predicts context bloat solutions | Reference-book analogy is specific | PASS |
| context-compression-strategy | Ch2 + Ch3 | Predicts reasoning quality from compression | Retrieval-not-reasoning insight is non-obvious | PASS |
| three-level-memory-evaluation | Ch3 throughout | Predicts memory system adequacy | 3-level progressive framework is specific | PASS |
| agentic-rag | Ch3 + Ch2 | Predicts when iterative search beats one-shot | Active-explorer analogy is specific | PASS |
| two-tier-memory-architecture | Ch3 (Experiments 3-10, 3-12) | Predicts proactive service feasibility | Resident+retrieval combination is specific | PASS |
| tool-design-principles | Ch4 + Ch1 | Predicts tool selection accuracy from design | ACI + fidelity principles are specific | PASS |
| proactive-tool-discovery | Ch4 + Ch2 | Predicts performance at tool scale | Declare-match-inject + Skills approaches are specific | PASS |
| coding-agent-meta-capability | Ch5 + throughout | Predicts architecture for open-ended tasks | 7-tool meta-capability framework is specific | PASS |
| proposer-reviewer-pattern | Intro + Ch1,4,5 | Predicts premature completion prevention | Verification-not-feeling principle is specific | PASS |

All 12 skills pass V1 (supported by 2+ independent sections), V2 (can answer novel questions), and V3 (not common sense).

