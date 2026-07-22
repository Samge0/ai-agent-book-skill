---
name: harness-engineering
description: "Trigger when building or debugging a production AI Agent that needs reliability beyond what the raw model provides. Key signals: Agent hallucinating tools, executing wrong actions, failing silently, or user asking how to make an Agent \"production-ready\" or \"reliable.\" Do NOT trigger for single-sh..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 1
tags: [agent-architecture, reliability, safety, production]
related_skills: []
---

# Harness Engineering: Constrain, Verify, Correct

## R -- Reading (Original Quote)
> The core of the Harness is the original formula's "Context + Tools," plus three layers of safeguards: Constrain (what the Agent may and may not do), Verify (whether it did the thing correctly), and Correct (how to recover when it did not)... Context and Tools let the Agent complete tasks. Constrain, Verify, and Correct make sure it does so reliably and safely.
> -- Li Bojie, Chapter 1

## I -- Interpretation (Methodology Skeleton)
Harness Engineering is the practice of building engineering infrastructure around an LLM to ensure reliable task execution. The core formula is: Agent = LLM + [Context + Tools + Constrain + Verify + Correct]. A minimal Agent runs on LLM + Context + Tools alone (the "demo view"). To run reliably in production, you need three additional layers: Constrain sets behavioral boundaries (fail-safe defaults -- everything off until explicitly enabled); Verify automatically checks correctness of tool execution results (input isolation -- check structured data, not free-form text); Correct recovers from failures (silent retries, circuit breakers, fallback to human). As models commoditize, these layers -- not the model -- become the competitive advantage.

## A1 -- Past Application (Book Examples)
### Example 1: Claude Code
- **Problem**: A Coding Agent that writes, edits, and tests code needs to avoid catastrophic failures (deleting critical files, infinite loops).
- **Method application**: The vast majority of Claude Code Harness code does Constrain (permission classification -- every tool requires user authorization by default), Verify (linter checks, type systems), and Correct (error recovery -- rollback to last stable state, circuit breaker after repeated failures), not just Context and Tools.
- **Conclusion**: The tools themselves (file read/write, command execution) are a small part; the safeguards built around them are the true core.
- **Result**: Claude Code achieves reliable long-running coding tasks through layered defense.

### Example 2: Pine AI (Financial Negotiation Agent)
- **Problem**: Agent negotiates real telecom bill reductions over the phone -- a single mistake means real financial loss.
- **Method application**: System prompt specifies 7-day refund policy (Context), calls query_order and process_refund tools (Tools), checks refund amount does not exceed order total (Constrain), confirms against database that refund went through (Verify), auto-retries on API timeout (Correct).
- **Conclusion**: Same model, but with Harness engineering the result is dramatically different -- the task completes reliably.
- **Result**: Pine AI reliably handles sensitive, long-horizon tasks involving money without human intervention.

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. Building an Agent that calls real APIs (payments, database writes, email sending) where errors have consequences
2. An Agent that "works in the demo" but fails unpredictably in production
3. Designing the architecture for an Agent that runs long tasks (dozens of tool calls, multi-session)
4. User asks "how do I make my Agent reliable" or "my Agent keeps doing the wrong thing"

### Language signals (user says things like)
- "My Agent works sometimes but fails unpredictably"
- "How do I make this production-ready?"
- "The Agent hallucinated a tool that doesn't exist"
- "My Agent gets stuck in infinite loops"
- "I need guardrails for my Agent"

## E -- Execution Steps
1. **Map the five Harness functions** -- List what Context, Tools, Constrain, Verify, and Correct look like for your specific Agent. Completion criteria: Each function has at least one concrete implementation identified.
2. **Implement Constrain with fail-safe defaults** -- All capabilities off by default; each must be explicitly enabled. Assign risk levels (low/medium/high) to tools. Completion criteria: No tool can execute a high-risk action without explicit authorization.
3. **Implement Verify with input isolation** -- Verification checks structured data (JSON from tools), not free-form model text. Use linters, type systems, result validation. Completion criteria: Every tool result is checked before the Agent acts on it.
4. **Implement Correct with graceful degradation** -- Silent retries for transient failures, circuit breaker after N consecutive failures, fallback to human. Completion criteria: No single failure mode causes the Agent to hang, loop, or produce a wrong result silently.
5. **Test with adversarial scenarios** -- Inject prompt injection, simulate tool timeouts, feed ambiguous inputs. Completion criteria: Agent degrades gracefully in all tested failure modes.

## B -- Boundary (When NOT to use)
- **Single-shot LLM calls**: If the task is "take this prompt, return one response," no Harness is needed -- just use the API directly.
- **Pure prompt optimization**: If the problem is output quality on a well-defined task, improve the prompt first; the Harness addresses reliability, not capability.
- **Author blindspot**: The author acknowledges the "Bitter Lesson" tension -- much of the Harness may eventually be internalized by models. Do not over-invest in Harness components that the model can already handle natively.

## Related Skills
- depends-on: context-compression-strategy
- contrasts-with: engineering-paradigm-evolution
