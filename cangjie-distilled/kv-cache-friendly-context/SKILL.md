---
name: kv-cache-friendly-context
description: "Trigger when designing Agent context layout for production, optimizing API costs, or when Agent latency/cost is too high. Key signals: \"my Agent is slow\", \"API costs are too high\", \"how to structure context\", \"KV cache\", \"context layout optimization\". Do NOT trigger for single-shot calls or when ..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 2
tags: [context-engineering, performance, cost-optimization, api-design]
related_skills: []
---

# KV Cache-Friendly Context Design

## R -- Reading (Original Quote)
> The KV Cache mechanism allows LLM inference engines to reuse computation results from previous calls... The first three conclusions are: (1) System prompts and tool definitions form a static prefix that can be cached; (2) Conversation history and tool results form a dynamic suffix that cannot; (3) Any change to the prefix invalidates the cache for everything after it.
> -- Li Bojie, Chapter 2

## I -- Interpretation (Methodology Skeleton)
KV Cache is a mechanism where the inference engine reuses computation from previous calls by caching the key-value pairs of the attention mechanism. The design principle: structure context as a static prefix (system prompt + tool definitions) followed by a dynamic suffix (conversation history + tool results). The static prefix is computed once and reused across all calls in a session. Any change to the prefix -- even a single character -- invalidates the entire cache. This means: put unchanging content first, put changing content last, and minimize changes to anything in the prefix. The practical impact: proper context layout can reduce inference cost by up to 90% and latency proportionally.

## A1 -- Past Application (Book Examples)
### Example 1: Agent Status Bar Placement
- **Problem**: The Agent Status Bar needs to inject dynamic information (current time, tool call count, task progress) into context. But putting it in the system prompt would break KV Cache every call.
- **Method application**: The status bar is injected as a user message at the end of the conversation (dynamic suffix), not in the system prompt (static prefix). The system prompt remains unchanged across calls.
- **Conclusion**: Dynamic meta-information must go in the dynamic suffix, even though it is conceptually "instruction-like."
- **Result**: KV Cache stays valid for the static prefix; only the suffix is recomputed each call.

### Example 2: Dynamic Tool Loading
- **Problem**: When tools are loaded dynamically (MCP-Zero, tool discovery), the tool definitions change, which would break the KV Cache prefix.
- **Method application**: Newly discovered tool schemas are appended at the end of the conversation (as user messages), not inserted into the system prompt. The system prompt keeps only a stable tool name list.
- **Conclusion**: Tool definitions no longer must sit at the very front of the context -- they can enter the trajectory on demand.
- **Result**: Major APIs (OpenAI Responses, Claude) now support this pattern natively with deferred loading flags.

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. Agent inference costs are too high or latency is unacceptable
2. Designing the context layout for a new Agent system
3. Adding dynamic information (timestamps, counters, status) to Agent context
4. Building a tool discovery system where tools load dynamically

### Language signals (user says things like)
- "My Agent API calls are too expensive"
- "How should I structure my context?"
- "The Agent is slow because every call recomputes everything"
- "I need to add a status bar / progress tracker to my Agent"

## E -- Execution Steps
1. **Audit your context for prefix/suffix separation** -- List what is static (never changes in a session) vs dynamic (changes every call). Completion criteria: Clear static/dynamic classification of every context component.
2. **Place static content first** -- System prompt, stable tool definitions, persona instructions go in the prefix. Completion criteria: Prefix contains zero dynamic elements.
3. **Place dynamic content last** -- Conversation history, tool results, status bar, dynamically loaded tools go in the suffix. Completion criteria: All changing content is append-only at the end.
4. **Minimize prefix mutations** -- If you must change the prefix (e.g., adding a tool), batch changes rather than doing them one at a time. Completion criteria: No more than one prefix mutation per session.
5. **Verify cache hit rates** -- Monitor API usage to confirm cache hits. Completion criteria: Cache hit rate above 80% for multi-turn sessions.

## B -- Boundary (When NOT to use)
- **Single-shot calls**: KV Cache benefits come from multi-turn reuse. A single API call has no cache to exploit.
- **Unpredictable context changes**: If your context changes randomly every call, the cache will never hit regardless of layout.
- **Author blindspot**: The book acknowledges that as context architectures evolve (editable/composable KV Cache), the rigid prefix-suffix rule may relax. RoPE relocation may allow cache patching in the future.

## Related Skills
- depends-on: progressive-disclosure-skills
- contrasts-with: context-compression-strategy
