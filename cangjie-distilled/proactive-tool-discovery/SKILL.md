---
name: proactive-tool-discovery
description: "Trigger when Agent has too many tools (100+), when tool selection accuracy degrades at scale, or when building dynamic tool loading systems. Key signals: \"too many tools\", \"tool discovery\", \"MCP-Zero\", \"dynamic tool loading\", \"tool search tool\", \"tool explosion\". Do NOT trigger when the Agent has..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 4
tags: [tool-discovery, scalability, mcp, dynamic-loading]
related_skills: []
---

# Proactive Tool Discovery

## R -- Reading (Original Quote)
> The Agent turns from passive recipient into active discoverer: when it hits a capability gap mid-execution, it declares in natural language what capability it needs, and the system matches and injects the tool on the fly... No tool schema is pre-loaded in the system prompt.
> -- Li Bojie, Chapter 4

## I -- Interpretation (Methodology Skeleton)
When tools number in the hundreds or thousands, injecting all schemas into the system prompt breaks both context budget and model selection accuracy. Proactive Tool Discovery inverts the paradigm: the Agent starts with only a few basic tools plus a "tool search tool." When it encounters a capability gap mid-execution, it declares in natural language what it needs (e.g., "I need the ability to query stock prices"), and the system matches and injects the tool on the fly. Two approaches exist: (1) MCP-Zero style: embedding-index semantic matching at two levels (server-level then tool-level), reporting ~98% token savings on ~2800 tools. (2) Skills style: no embedding infrastructure at all -- the Agent browses a skill directory using general file-reading tools (grep, read_file), looking up what it needs when it needs it. Both approaches append newly discovered tools at the end of the conversation (dynamic suffix) to preserve KV Cache, rather than injecting into the system prompt (static prefix).

## A1 -- Past Application (Book Examples)
### Example 1: MCP-Zero
- **Problem**: An Agent with access to ~2800 tools cannot have all schemas in context.
- **Method application**: The Agent emits structured request blocks in its thinking (e.g., "GitHub server: search repositories and return metadata"), and the system routes through two levels of semantic matching (server-level, then tool-level) before injecting the matched tool schema.
- **Conclusion**: Proactive discovery not only helps capable LLMs handle thousands of tools but also keeps small models usable in scenarios with hundreds of tools.
- **Result**: ~98% token savings over full injection; significant improvement in accuracy and task completion for small models (Experiment 4-6).

### Example 2: Skills as Tool Discovery
- **Problem**: The embedding-index infrastructure for MCP-Zero is complex to maintain.
- **Method application**: Skills invert the approach -- the Agent uses general file-reading ability to browse a skill directory. At startup it sees only a thin catalog (name + description). When it needs a capability, it reads the corresponding skill file, which may reference deeper sub-documents.
- **Conclusion**: The Agent needs nothing beyond general file-reading ability -- no vector index, no semantic-retrieval infrastructure. It is how humans use reference material.
- **Result**: A more modern, lower-maintenance way to discover tools at scale.

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. Agent has 100+ tools and selection accuracy is degrading
2. Building an MCP-based system with many servers and tools
3. Context budget is overwhelmed by tool definitions
4. Small model struggling with large tool sets

### Language signals (user says things like)
- "My Agent has too many tools and keeps picking the wrong one"
- "How do I handle 500+ MCP tools?"
- "The tool definitions alone use 50K tokens"
- "I need dynamic tool loading for my Agent"

## E -- Execution Steps
1. **Choose discovery approach** -- MCP-Zero (embedding index, semantic matching) for large-scale structured tool ecosystems; Skills (file-reading) for lower-maintenance discovery. Completion criteria: Approach selected with justification.
2. **Set up the meta-tool** -- Provide a discover_tools or tool_search tool that accepts natural language requests and returns matched tool schemas. Completion criteria: Meta-tool returns 3-5 relevant candidates per query.
3. **Minimize the system prompt tool set** -- Keep only basic tools (web_search, code_interpreter) plus the meta-tool in the system prompt. Completion criteria: System prompt tool definitions under 2000 tokens.
4. **Append discovered tools at conversation end** -- Newly loaded schemas are appended as user messages (dynamic suffix), not inserted into the system prompt. Completion criteria: KV Cache prefix remains stable.
5. **Guide the model to discover proactively** -- When the Agent encounters a capability gap, it should call the discovery tool rather than hallucinating. Completion criteria: Agent proactively discovers tools in 80%+ of capability-gap scenarios.

## B -- Boundary (When NOT to use)
- **Small tool sets**: If the Agent has fewer than ~20 tools, full injection is simpler and more reliable than discovery.
- **Ultra-low-latency**: Discovery adds a round-trip for each tool lookup. If latency is critical, pre-load all tools.
- **Author blindspot**: Weaker models struggle with tool definitions appearing at non-standard positions mid-context, often emitting malformed calls. The book notes this may require dedicated reinforcement learning training.

## Related Skills
- depends-on: tool-design-principles
- depends-on: kv-cache-friendly-context
- composes-with: progressive-disclosure-skills
