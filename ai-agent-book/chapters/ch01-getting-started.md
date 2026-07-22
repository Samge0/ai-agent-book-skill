# Chapter 1: Getting Started with AI Agents

## Core Idea
A modern Agent is **LLM + Context + Tools**, but production-grade reliability requires a **Harness** — engineering layers of Constrain, Verify, and Correct built around context and tools. Harness Engineering is the competitive advantage as models commoditize.

## Frameworks Introduced

- **Agent Formula**: Agent = LLM + Context + Tools
  - When to use: Always — this is the foundational mental model
  - How: LLM = reasoning engine `policy`, Context = working info set `observation space`, Tools = action interfaces `action space`

- **Production Formula**: Agent = Model + Harness, where Harness = [Context + Tools + Constrain + Verify + Correct]
  - When to use: Production systems, not demos
  - How: Context/Tools = core capability; Constrain = behavioral boundaries; Verify = correctness checking; Correct = error recovery

- **ReAct Loop**: Reason then Act then Observe, repeat until done
  - When to use: The core execution loop of any autonomous Agent
  - How: Each LLM call receives static prefix `system prompt + tool defs` plus cumulative trajectory `message history`

- **Five Engineering Paradigm Layers**: Software Eng, then Prompt Eng, then Context Eng, then Harness Eng, then Loop Eng
  - When to use: Choosing your engineering scope
  - How: Each layer is a nested superset — start simple, widen scope only as needed

- **Three Learning Paradigms**: Post-training `permanent, months` OR In-Context Learning `session-only, instant` OR Externalized Learning `persistent + interpretable, hours`
  - When to use: Deciding how an Agent acquires new capability
  - How: Post-training = foundational; In-context = rapid adaptation; Externalized = reliability + efficiency

## Key Concepts
- **Harness Engineering**: All infrastructure outside the model — context management, tool interfaces, safety constraints, verification, and correction mechanisms
- **Context**: Working set of information at each decision point `not just text fed to model`
- **Tools**: Full set of action interfaces — predefined calls, Skills on demand, code generation, sub-agent delegation
- **Tool Calling / Function Calling**: Model decides whether to call, which tool, and what arguments
- **Zero-shot Generalization**: Handling unseen tasks by recombining existing knowledge
- **Few-shot Adaptation**: Learning a new task pattern from 2-3 examples in the prompt
- **Internal Reasoning**: Planning before acting — a distinctive LLM Agent capability from pre-training
- **ACI `Agent-Computer Interface`**: Designing tool interfaces from the Agent perspective, not the programmer perspective
- **Poka-yoke**: Error-prevention design philosophy — make mistakes impossible, like a USB connector
- **Bitter Lesson `Sutton`**: General methods that scale with compute/data eventually outperform hand-crafted domain knowledge

## Mental Models
- **Use the formula as a diagnostic**: When an Agent fails, ask: Is it a model problem `reasoning`, a context problem `missing info`, or a tools problem `cannot act`?
- **Think of Context as the working memory of a decision-maker**: They can only use what is in front of them at decision time
- **Think of the Harness as training wheels that also prevent crashes**: The model is the rider; the Harness keeps the ride safe and on course
- **Think of paradigm layers as nested circles**: Prompt is subset of Context is subset of Harness is subset of Loop

## Anti-patterns
- **Building an autonomous Agent before trying a single LLM call or workflow**: Adds complexity without proving necessity
- **Designing tools from the programmer perspective, not the Agent perspective**: Vague interfaces get amplified into systemic error
- **Treating security as a post-launch patch**: Guardrails must be designed from line one — security spans five levels
- **Choosing a model based only on leaderboards**: Always evaluate on your own tasks; tool-calling ability varies widely

## Code Examples
```
Tool Calling at the API Level — 4 steps

Step 1: Declare tools                  Step 2: Model decides to call
tools: [{                              assistant: {
  name: "get_weather",                   tool_calls: [{
  parameters: {                            function: "get_weather",
    city: "string"                         arguments: {city: "Beijing"}
  }                                       }]
}                                     }

Step 3: Result appended to context     Step 4: Model responds based on result
tool: {                                assistant: {
  tool_call_id: "call_1",                content: "Today in Beijing: 28C, sunny."
  content: '{"temp":28,"sky":"clear"}' }
}                                      }
```
- **What it demonstrates**: The model decides whether/which/what; the developer only defines tools and executes.

## Reference Tables

### Five Harness Functions
| Function | Responsibility | Principle | Chapter |
|----------|---------------|-----------|---------|
| **Context** | Provide relevant information | Information Sufficiency | Ch 2, 3 |
| **Tools** | Provide action interfaces | Clear Interface `ACI` | Ch 4 |
| **Constrain** | Set behavioral boundaries | Fail-Safe Defaults | Ch 4 |
| **Verify** | Judge correctness of results | Input Isolation `check structured data, not free text` | Ch 5, 6 |
| **Correct** | Recover from failures | Do not expose intermediate states until unrecoverable | Ch 2, 5 |

### Orchestration Patterns
| Pattern | Path | When to Use | Trade-off |
|---------|------|-------------|-----------|
| **Workflow** | Predefined, deterministic | Fixed sub-tasks, compliance requirements | Strict control + security; lacks flexibility |
| **Autonomous Agent** | Runtime, dynamic | Open-ended, unpredictable steps | Flexible; higher cost, errors compound |

## Worked Example
**Refund without vs. with Harness**: User asks to refund a 3-day-old order.
- **Without Harness**: Model has no refund policy `no context`, does not know the API `no tools`, fabricates a refund result `no verification`, user discovers nothing happened `no correction`.
- **With Harness**: System prompt specifies 7-day policy `context`, Agent calls query_order and process_refund `tools`, framework checks refund does not exceed order total `constrain`, confirms against database `verify`, auto-retries on timeout `correct`.
- **Lesson**: Same model, substantially different results — the Harness is the differentiator.

## Key Takeaways
1. **Start simple, escalate complexity only when needed**: Single LLM call, then workflow, then autonomous Agent.
2. **Context is the ceiling of Agent capability**: A moderately capable model with well-organized context outperforms a stronger model with insufficient context.
3. **Harness Engineering is the competitive advantage**: The constrain/verify/correct layers determine who wins.
4. **Design tool interfaces from the Agent perspective `ACI`**: Poka-yoke — prevent mistakes by design.
5. **Security is architectural, not a patch**: Design guardrails from line one; use defense in depth.
6. **Most Agents need a reasoning model**: Multi-step decisions and dynamic tool selection require reasoning capability.
7. **Consider output speed and multimodal support when choosing models**: 20-round tasks at 2s slower/round means 40s extra latency.

## Connects To
- **Ch 2**: Context Engineering — the most central Harness component
- **Ch 4**: Tool Design — ACI principles, MCP standard, tool categories
- **Ch 5**: Harness Engineering in practice — Claude Code safeguards
- **Ch 7**: Post-training — the RL roots of the Agent concept
- **Ch 8**: Loop Engineering — the fifth paradigm layer
- **Ch 10**: Multi-agent — constraints and corrections among multiple Agents
