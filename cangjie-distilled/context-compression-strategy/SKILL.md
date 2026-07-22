---
name: context-compression-strategy
description: "Trigger when Agent context is growing too large, tool results are bloating the conversation, or reasoning quality degrades in long sessions. Key signals: \"context window full\", \"Agent forgets early information\", \"too many tokens\", \"context rot\", \"how to summarize conversation history\". Do NOT tri..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 2
tags: [context-engineering, compression, optimization, memory]
related_skills: []
---

# Context Compression Strategy Selection

## R -- Reading (Original Quote)
> Compression is Understanding... Effective compression requires deep semantic understanding -- capturing the core meaning of the context with more refined expression... Do not make the model search passively through vast amounts of raw material; provide refined, structured knowledge instead.
> -- Li Bojie, Chapter 2

## I -- Interpretation (Methodology Skeleton)
Context compression has two distinct motivations: (1) controlling length and cost (obvious), and (2) improving reasoning quality (non-obvious but deeper). The key insight is that in-context learning is essentially retrieval, not reasoning -- the attention mechanism is good at looking up existing content but not at computing aggregate summaries in a single forward pass. Therefore, summarizing raw data into structured conclusions makes the model smarter, not just faster. The strategy hierarchy from cheapest to most aggressive: (1) Tool Result Budget Control (store full output on disk, show preview), (2) Direct Noise Deletion (remove low-value content without summarizing), (3) API-Level Micro-Compression (server-side removal of specific tool results), (4) Archival Summarization (round-by-round structured summaries), (5) Full Compression (LLM-driven complete compression as last resort). Context-aware compression -- incorporating current query intent -- reduces tokens by over 75% while preserving key information.

## A1 -- Past Application (Book Examples)
### Example 1: Six Compression Strategies Compared (Experiment 2-9)
- **Problem**: A research task tracking OpenAI co-founders generated 367K characters across 7 tool calls, overflowing a 128K context window.
- **Method application**: Six strategies tested: No Compression (task failed at iteration 5), Individual Summarization (12 iterations, 276K tokens), Combined Summarization (10 iterations, 93K tokens), Context-Aware Compression (7 iterations, 40K tokens, 3% ratio), Context-Aware with Citations (222K tokens but verifiable), Adaptive Windowing (174K tokens but preserves early detail).
- **Conclusion**: Context-Aware Compression was optimal -- it incorporates the current query intent into the compression decision, dynamically adjusting focus.
- **Result**: Over 75% token reduction while completing the task successfully.

### Example 2: Claude Code Hierarchical Compression
- **Problem**: Production Coding Agent needs to manage context across long coding sessions without losing critical information.
- **Method application**: Five-layer hierarchy: Tool Result Budget Control, Direct Noise Deletion, API-Level Micro-Compression, Archival Summarization (like git log, not git squash), Full Compression with circuit breaker. Compression preserves architectural decisions, modified file lists, verification status, and TODOs above all else.
- **Conclusion**: Different information types have different lifecycles -- match the compression strategy to the information value.
- **Result**: Production-grade context management that maintains task coherence across long sessions.

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. Agent context approaching or exceeding the model window limit
2. Agent reasoning quality degrading in long multi-turn sessions (Context Rot)
3. Tool results (web pages, API responses, file contents) bloating context with raw data
4. API costs for long sessions are too high

### Language signals (user says things like)
- "My Agent runs out of context after a few tool calls"
- "The Agent forgets what it learned earlier in the conversation"
- "Tool results are eating up my entire context window"
- "The Agent gets confused in long sessions"

## E -- Execution Steps
1. **Identify compression targets** -- Find the largest context consumers (typically tool results). Completion criteria: List of tool results over 1000 tokens each.
2. **Apply Tool Result Budget Control** -- Store large outputs externally, show only previews. Completion criteria: No single tool result exceeds your budget (e.g., 2000 tokens).
3. **Delete direct noise** -- Remove low-value content (navigation bars, footers, duplicate information) without summarizing. Completion criteria: Irrelevant content removed.
4. **Apply context-aware summarization** -- Use LLM to summarize tool results with the current query as context. Completion criteria: Summaries preserve key entities, facts, and identifiers (UUIDs, hashes, URLs must remain exact).
5. **Define retention priorities** -- Architectural decisions, modified file lists, verification status, unresolved TODOs must NEVER be compressed away. Completion criteria: Explicit retention priority list documented.

## B -- Boundary (When NOT to use)
- **Short conversations**: If the session is under 10 turns and context is well within limits, compression adds cost without benefit.
- **When precision matters more than efficiency**: Some tasks require exact verbatim retention (legal, medical). Compression introduces loss.
- **Author blindspot**: Compression most easily loses early architectural decisions, reasoning behind constraints, and failed paths. The book recommends explicit retention priorities but acknowledges this is a best-effort heuristic.

## Related Skills
- depends-on: kv-cache-friendly-context
- contrasts-with: two-tier-memory-architecture
