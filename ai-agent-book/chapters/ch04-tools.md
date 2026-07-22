# Chapter 4: Tools

## Core Idea
Tools are the Agent action interfaces — the bridge from passive observer to active system. Tool quality directly determines what an Agent can reliably accomplish: define interfaces vaguely and the model will misuse them; scope permissions too broadly and one error becomes irreversible.

## Frameworks Introduced

- **Five Tool Categories** `by interaction direction`:
  1. **Perception**: Access info `search engines, file systems, APIs, databases`
  2. **Execution**: Act on systems `code exec, file ops, commands, API calls`
  3. **Collaboration**: Divide work with other Agents/humans `sub-agents, HITL`
  4. **Scheduling/Event Trigger**: External inputs that trigger the Agent `timers, webhooks, cron`
  5. **Security/User Communication**: Channels to communicate with user `IM, email, voice`
  - When to use: Classifying any tool in your system
  - How: The first three are actively invoked; event triggers arrive as external inputs

- **ACI `Agent-Computer Interface`**: Design tools from the Agent perspective, not the programmer perspective
  - When to use: Always — ACI is the universal principle of tool design
  - How: Intuitive names, clear boundaries `what it cannot do`, concrete parameter examples

- **Tool Granularity/Generality/Description Trade-off**:
  - When to use: Designing any toolset
  - How: General tools preferred unless clear security/performance reason; integrate similar tools; describe boundaries not just capabilities

- **Dedicated Tools vs Skills + General Executors**:
  - When to use: Choosing capability expression form
  - How: Complex params/frequent calls = dedicated tools; volatile business rules = Skills + executor

- **Proposer-Reviewer Pattern**: One Agent generates, another independently reviews with external feedback
  - When to use: Quality-critical tasks `code, PPT, security review`
  - How: The Reviewer value comes from NEW information `rendered screenshots, test results`, not from re-reading

- **Sidecar Mechanism**: Independent review outside the main Agent context
  - When to use: Critical operations needing security review
  - How: Sidecar reviews tool calls independently; speculative execution makes checks invisible to user

- **Event-Driven Async Architecture**: Unify all inputs as an event stream
  - When to use: Long-running tasks, concurrent events, user interruption
  - How: Event queue + event loop; no polling; external events push to Agent in real-time

- **Proactive Tool Discovery**: Dynamic tool discovery instead of injecting all definitions at once
  - When to use: Large tool ecosystems `100+ tools`
  - How: Skills turn tool discovery into "on-demand lookup" via Progressive Disclosure

## Key Concepts
- **MCP `Model Context Protocol`**: Open standard for Agent-tool communication — universal socket standard for AI tools
- **Tool Description Art**: Describe "when to use" not just "what it does"; list boundary conditions and negative examples
- **Parameter Fidelity**: No systematic discrepancy between model-perceived world and tool-operated world
- **Silent Input Transformation**: Tool quietly "corrects" model parameters — a dangerous anti-pattern
- **Error-Prevention Design `Poka-yoke`**: Prevent mistakes by design, like a USB connector
- **Hooks/Cron/Heartbeat**: Three automation mechanisms `lifecycle events, scheduled tasks, periodic checks`
- **Virtual Identity**: Agent operates with isolated execution environment and virtual identity
- **Sub-Agent Context Passing**: 4 strategies `minimal, manual filtered, automatic truncated, LLM-generated`

## Mental Models
- **Think of tools as verbs in the Agent vocabulary**: More verbs = more things it can do, but too many verbs confuse
- **Think of ACI as Poka-yoke for AI**: Design interfaces that prevent mistakes, not interfaces that require instructions
- **Think of event-driven architecture as a secretary desk**: Multiple pending items; prioritize by urgency; pause and switch mid-task
- **Use dedicated tools for security-critical paths, general tools for everything else**

## Anti-patterns
- **Vague tool descriptions**: "Search for relevant content" is far less effective than "Use when needing real-time info or unknown facts"
- **Missing negative examples**: If a file search tool only matches filenames not content, say so — otherwise the LLM guesses wrong
- **Silent parameter injection**: Tool appends extra params without model knowledge `e.g., git commit marking`; causes unexplainable failures
- **Keyword blacklists for shell commands**: Shell combinatorial explosion bypasses any static rules `use semantic parsing`

## Code Examples
```
# Good tool description vs Bad tool description

# BAD: "Search for relevant content"
# GOOD: "Use when needing real-time information or finding unknown facts.
#        Cannot search file contents — use read_file for that.
#        Returns JSON array with title, url, snippet fields.
#        Large websites may take 5-10 seconds."

# Parameter with concrete example:
# timestamp: RFC3339 format, e.g., 2024-03-15T14:30:00Z
# phone: E.164 format, e.g., +8613888888888 (China)
```
- **What it demonstrates**: Tool descriptions must cover when-to-use, boundaries `what it cannot do`, return format, and concrete parameter examples. Adding examples improves accuracy from ~72% to ~90%.

## Reference Tables

### Sub-Agent Context Passing Strategies
| Strategy | Privacy | Info Sufficiency | Cost | When to Use |
|----------|---------|------------------|------|-------------|
| Minimal Passing | Best | May starve | Lowest | Simple high-frequency calls |
| Manual Filtered | Good | Flexible | Medium | Known relevant fields |
| Automatic Truncated | Medium | Balanced | Medium | Default for moderate tasks |
| LLM-Generated | Configurable | Highest | Extra LLM call | Complex tasks `reports, customer service` |

### Tool Design Evolution
| Generation | Focus | Example |
|-----------|-------|---------|
| 1st: API Wrappers | Map each API to a tool | Fine granularity, multiple tools per goal |
| 2nd: ACI-based | Tools correspond to Agent goals | Granularity trade-offs, generality, description |
| 3rd: Discovery + Chaining | Dynamic discovery + code orchestration | Example-driven invocation, code orchestrates calls |

## Worked Example
**Cursor curly quotes bug**: The edit tool accepts old_string and new_string for match-and-replace. But the parameter passing layer silently converts Chinese curly quotes to English straight quotes. The model reads the file, sees curly quotes, passes them to old_string. The layer converts them to straight quotes, which do not match the file content. "No match found." The model retries endlessly — it cannot understand why the tool cannot find what it clearly saw. **Lesson**: There must be no systematic discrepancy between the world the model perceives and the world the tool operates on.

## Key Takeaways
1. **Describe "when to use" not just "what it does"**: Most tool selection errors trace to unclear descriptions.
2. **List boundary conditions — what it cannot do**: This is often more important than describing capabilities.
3. **Use concrete parameter examples**: "RFC3339 format, e.g., 2024-03-15T14:30:00Z" beats "RFC3339 format" alone.
4. **Prefer general tools unless clear security/performance reason**: A Python interpreter replaces dozens of calculators.
5. **No silent input transformation**: The model-perceived world and tool-operated world must match exactly.
6. **Use semantic parsing for shell commands**: Keyword blacklists cannot handle combinatorial explosion.
7. **Event-driven architecture enables true proactive service**: The world can actively notify the Agent, not just the Agent polling.

## Connects To
- **Ch 1**: ACI introduced as a core principle for building effective Agents
- **Ch 2**: Tool definitions as a static prefix component of context
- **Ch 5**: Seven core tools of a Coding Agent; code as meta-capability
- **Ch 8**: Proactive tool discovery; Agent creates new tools from experience
- **Ch 10**: Sub-agent design as collaboration tools; multi-agent topology
