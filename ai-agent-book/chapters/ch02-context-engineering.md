# Chapter 2: Context Engineering

## Core Idea
Context sets the ceiling of Agent capability. What you show the model and how you organize it matters more than the model itself. The key principle: **in-context learning is more retrieval than reasoning** — pre-compute conclusions, compress raw records, and use explicit code for aggregation.

## Frameworks Introduced

- **KV Cache 3 Commandments**:
  1. Once system prompt and tool definitions are finalized, do NOT change them
  2. Always append dynamic information to the END of context, never modify the prefix
  3. Use standard API message format — never manually concatenate messages
  - When to use: Any production Agent system
  - How: Cache reuse depends on byte-for-byte prefix stability; one character change invalidates all downstream cache

- **Chat Template**: The "envelope format" that converts structured API messages into a linear token stream
  - When to use: Understanding why standard API formats must be used
  - How: Special tokens `<|im_start|>`, `<|im_end|>` mark role/boundary; different model families use different formats

- **Agent Status Bar**: Framework-injected meta-information at the end of context `like a phone status bar`
  - When to use: When the model needs runtime state it cannot reliably infer `tool counts, TODO lists, environment state`
  - How: Inject as user-role message at end of context; maintain with code, not LLM

- **6 Context Compression Strategies**: No Compression, Individual Summarization, Combined Summarization, Context-Aware Compression, Context-Aware with Citations, Adaptive Windowing
  - When to use: When context approaches window limits or information density drops
  - How: Batch compress near threshold; context-aware compression reduces tokens by 75%+

- **Context Distillation**: Pre-compute conclusions into explicit knowledge the model can directly retrieve
  - When to use: Any time implicit state is scattered across the trajectory
  - How: Status bar maintained by code achieves near-constant reasoning cost regardless of context length

## Key Concepts
- **KV Cache**: Stores intermediate key-value states so later computation can reuse them; prerequisite is prefix stability
- **Prompt Cache**: Cross-request cache built on KV Cache; providers charge ~1/10 price for cached prefixes
- **Attention Sink**: First token absorbs abnormally high attention weight `70%+`; a mathematical consequence of softmax
- **Position Bias**: Higher recall for beginning/end of context; middle information easily overlooked
- **Context Rot**: Context fits but cannot be found — quality quietly deteriorates as length increases
- **Progressive Disclosure**: Skills metadata is shown first; full content loaded only when the Agent judges it is needed
- **Agent Skills**: Composable units of domain capability loaded on demand `e.g., SKILL.md`
- **Sub-Agent Context Isolation**: Delegate bulky intermediate work to sub-agents; only summaries return to main context

## Mental Models
- **Think of KV Cache as a shared memo**: Changing one word at the top forces everyone after it to rewrite their notes
- **Think of the Status Bar as a dashboard**: Pre-computed readings the model reads directly instead of scanning raw logs
- **Think of compression as understanding**: Good compression requires deep semantic understanding, not just truncation
- **Use code for counting, attention for lookup**: The model excels at retrieving facts but cannot aggregate statistics in one forward pass

## Anti-patterns
- **Dynamic system prompt `e.g., timestamp in system prompt`**: Invalidates entire KV Cache; TTFT jumps from 0.5s to 3-5s
- **Sliding window conversation history**: Breaks prefix consistency AND discards critical tool results causing infinite loops
- **Text formatting `"USER: ... ASSISTANT: ..."`**: Deviates from training format; model must infer role boundaries from weaker signals
- **Asking an LLM to summarize the status bar**: A 20-line regex function beats a frontier model at maintaining status accuracy
- **Dynamic sorting of tool definitions**: Reordering tools by frequency invalidates the entire cache; fixed order has almost no effect on accuracy

## Code Examples
```xml
<agent_status>
Current State:
- Tool call summary: 'phone_call' has been invoked 3 times `Xfinity: 3/3 max`
- Constraint check: Maximum calls to Xfinity reached `3/3`
- Current time: 2025-09-14 10:30:45
- TODO: [1] Cancel plan `in_progress`
</agent_status>
```
- **What it demonstrates**: The status bar is injected as a user-role message at the END of context, near the tokens the model is about to generate. It converts implicit state `scattered call records` into explicit, directly-retrievable knowledge.

## Reference Tables

### 6 Compression Strategies Comparison
| Strategy | Compression Ratio | Token Usage | Key Trait |
|----------|------------------|-------------|-----------|
| No Compression | N/A | Overflow at ~165K tokens | Fails outright |
| Individual Summarization | 10.9% | 276,608 | Information fragmentation |
| Combined Summarization | 4.3% | 93,449 | Truncation risk on long input |
| **Context-Aware** | **3.0%** | **40,157** | Dynamically adjusts focus by query intent |
| Context-Aware + Citations | 4.1% | 222,992 | Adds source URLs for verification |
| Adaptive Windowing | Variable | 174,601 | Preserves original until 80% threshold |

### Status Bar: Two Implementation Modes
| Mode | How | Cache Cost | When to Use |
|------|-----|------------|-------------|
| **Replace each round** | Remove old status, append new | Invalidates cache after status position | Short trajectory or large status message |
| **Persistent append** | Never delete; append new each round | Fully cache-friendly | Frequent updates + long trajectory |

### Production-Grade 5-Layer Compression `Claude Code`
1. Tool Result Budget Control `large outputs to disk, preview only`
2. Direct Noise Deletion `remove low-value content without summarizing`
3. API-Level Micro-Compression `server removes specific tool results`
4. Archival Summarization `structured, round-by-round like git log`
5. Full Compression `last resort, two-stage with circuit breaker`

## Worked Example
**KV Cache production incident**: A team's customer service Agent handled 100K conversations/day. An engineer added `Current time: {{now}}` to the system prompt. Next day: TTFT jumped from 0.5s to 3-5s, monthly inference bill nearly doubled. The timestamp invalidated the KV Cache on every request. Fix: append time as a user message at the END, or get it via tool call when needed.

## Key Takeaways
1. **Never modify the system prompt after deployment**: Any change, even one space, invalidates the entire cache and multiplies latency.
2. **Maintain the status bar with code, not LLM**: A 20-line regex function achieves ground-truth accuracy; a frontier model summarizing history produces incorrect entries.
3. **The model unconditionally trusts the status bar**: If it says "called 3 times," the model accepts it. Monitor status accuracy as a first-line production metric.
4. **Compress near the threshold, not every round**: Frequent compression breaks cache repeatedly. Batch compress when approaching 80% of window.
5. **Isolation beats compression**: Delegate bulky searches to sub-agents — noise never enters the main context.
6. **Identifiers must survive compression**: UUIDs, hashes, IP addresses, PR numbers — preserve exactly or tool calls fail.
7. **Retain architectural decisions during compression**: Never summarize constraints, key change records, verification status, or unresolved TODOs.

## Connects To
- **Ch 1**: Agent formula — context is one of three core components
- **Ch 3**: Memory and knowledge — persistent knowledge across sessions
- **Ch 4**: Tool definitions as a static prefix component
- **Ch 5**: Context isolation in sub-agents `Claude Code Task tool`
- **Ch 8**: Sleep consolidation — offline memory integration as periodic compression
