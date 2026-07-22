---
name: agent-status-bar
description: "Trigger when building a production Agent that loses track of its own state, enters infinite loops, drifts from goals, or the user asks how to give an Agent \"self-awareness\" of its runtime progress. Inject structured meta-information at the end of context. Do NOT trigger for static system prompt d..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 2
tags: [context-engineering, meta-information, production-agent, state-management]
related_skills: [kv-cache-friendly-context, context-compression-strategy]
---

# Agent Status Bar: Managing Trajectories with Meta-Information

## R -- Reading (Original Quote)
> The Agent Status Bar is a real-time dashboard continuously updated as the task progresses... it is not part of the conversation's primary content, but a state summary injected by the Agent framework at the end of the context: "You have made 3 calls," "Current time is 10:30," "2 TODO items remaining." Each time the model generates a response, it can use this state to make better decisions.
> -- Li Bojie, Chapter 2

## I -- Interpretation (Methodology Skeleton)
The Agent Status Bar is a mechanism for synchronizing dynamic runtime state with the model during execution. Static system prompts tell the model what to do; the status bar tells it what is happening right now. The theoretical basis: in-context learning is more retrieval-like than reasoning-like. The model is good at finding information that already exists in the context, but less reliable at actively summarizing aggregate state during a single forward pass. By injecting structured state summaries (call count, elapsed time, TODO progress, budget remaining) at the end of the context, the framework gives the model explicit signals to make better decisions. This is analogous to the status bar of an operating system -- not the main app content, but a persistent state display.

## A1 -- Past Application (Book Examples)
### Example 1: Claude Code's Tool Counter
- **Problem**: An Agent making many sequential tool calls loses track of how many calls it has made, risking infinite loops.
- **Method application**: The framework injects a status bar showing "call N" at the end of each turn, giving the model awareness of its iteration count.
- **Conclusion**: The model can self-regulate -- if it sees it has made 15 calls, it knows to wrap up rather than continue indefinitely.
- **Result**: Prevents infinite loops without hard-coded limits.

### Example 2: TODO Progress Tracking
- **Problem**: During multi-step tasks, the Agent drifts from the original goal.
- **Method application**: The status bar shows "2 of 5 TODO items remaining" with the current item highlighted.
- **Conclusion**: The model retrieves the pending items from the status bar rather than having to reconstruct the plan from conversation history.
- **Result**: Improved task completion rates and reduced goal drift.

### Example 3: Physical Time Perception
- **Problem**: The model has no concept of how much real time has passed between actions.
- **Method application**: The status bar includes a timestamp: "Current time: 10:30 AM. Session started: 9:00 AM."
- **Conclusion**: The model can infer urgency, recognize timeouts, and avoid redundant waits.

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. Building an Agent that enters infinite loops or repeats actions
2. An Agent that loses track of multi-step task progress
3. A production Agent that needs budget/time limits enforced softly
4. An Agent that drifts from its original goal mid-conversation

### Language signals (user says things like)
- "My Agent keeps looping / repeating itself"
- "The Agent forgets which step it's on"
- "How do I give my Agent self-awareness of its state?"
- "My Agent drifts from the task halfway through"

## E -- Execution Steps
1. **Identify state variables** -- Completion criteria: list of dynamic variables the model needs (call count, TODO items, elapsed time, budget remaining, error count)
2. **Design the status bar format** -- Completion criteria: a compact structured format (JSON or key-value) that fits in <100 tokens and is placed at the END of context
3. **Integrate into the Agent loop** -- Completion criteria: after each model response, the framework updates the status bar before the next model call
4. **Cache-optimization** -- Completion criteria: use cache-friendly append-only updates (append new status as the latest user message) rather than rewriting history

## B -- Boundary (When NOT to use)
- One-shot LLM calls (no state to track)
- Simple chatbots with no multi-step execution
- When the conversation is short enough that the model can self-track from history (< 10 turns)
- Over-engineering: if your Agent completes in 2-3 steps, a status bar adds overhead without benefit

## Related Skills
- depends-on: kv-cache-friendly-context (status bar is placed at context end for cache-friendliness)
- complements: context-compression-strategy (status bar summarizes state so less history is needed)
