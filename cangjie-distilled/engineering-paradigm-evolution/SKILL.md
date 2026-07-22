---
name: engineering-paradigm-evolution
description: "Trigger when user asks about the evolution of AI engineering, when to use prompts vs workflows vs autonomous Agents, or which orchestration pattern to choose. Key signals: \"should I use a workflow or an Agent\", \"how complex should my architecture be\", \"what level of engineering do I need\". Do NOT..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 1
tags: [architecture, orchestration, evolution, decision-making]
related_skills: []
---

# Five-Layer Engineering Paradigm Evolution

## R -- Reading (Original Quote)
> Software Engineering is the foundation... Prompt Engineering was the first wave... Context Engineering was the second wave... Harness Engineering was the third wave... The newest wave is Loop Engineering... These five stages are not replacements but nested layers: Prompt Engineering is a subset of Context Engineering, which is a subset of Harness Engineering, which is a subset of Loop Engineering.
> -- Li Bojie, Chapter 1

## I -- Interpretation (Methodology Skeleton)
AI application engineering has evolved through five nested layers, each widening the engineer scope of concern: (1) Software Engineering (traditional systems), (2) Prompt Engineering (refining natural-language instructions), (3) Context Engineering (managing the full working context: system prompts, tool definitions, conversation history, external knowledge), (4) Harness Engineering (all infrastructure outside the model: constraint, verification, feedback loops, error recovery), (5) Loop Engineering (sustained autonomous operation: who discovers work, when to verify, when done). The key insight: each layer is a superset, not a replacement. Use the simplest layer that solves the problem. Progress only when the current layer proves insufficient.

## A1 -- Past Application (Book Examples)
### Example 1: Choosing Orchestration Pattern
- **Problem**: When building an LLM application, which pattern should you use?
- **Method application**: The book prescribes a progression: Start with a single LLM call. If better prompts and examples solve it, stop. If multiple steps are needed and decompose cleanly, use a workflow. Use an autonomous Agent only when you need dynamic decisions and flexible execution paths.
- **Conclusion**: Prompts first, workflows second, autonomous Agents last -- this ordering minimizes unexpected behavior.
- **Result**: The most successful implementations use simple, composable patterns rather than complex frameworks.

### Example 2: LangChain Terminal Bench 2.0
- **Problem**: A Coding Agent benchmark score needed improvement.
- **Method application**: LangChain improved from 52.8% to 66.5% -- jumping from outside top 30 to top 5. What changed was not the model but the Harness: having the Agent check its own execution results, detect repetitive loops, and refine reasoning strategy.
- **Conclusion**: Competitive advantage shifted from model capability to engineering outside the model.
- **Result**: Significant benchmark improvement through Harness-layer engineering alone.

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. Deciding whether to build a workflow or an autonomous Agent for a new task
2. Understanding why a "simple prompt" approach failed and what layer to move to
3. Choosing the right level of complexity for an Agent system
4. Evaluating whether a team is over-engineering or under-engineering their Agent

### Language signals (user says things like)
- "Should I use a workflow or an autonomous Agent?"
- "My Agent is too complex, can I simplify?"
- "What level of engineering does my use case need?"
- "Do I need an Agent framework or just API calls?"

## E -- Execution Steps
1. **Start with the simplest layer** -- Can a single LLM call with good prompting solve this? Completion criteria: Document why prompting alone is or is not sufficient.
2. **Escalate to Context Engineering if needed** -- Does the task need managed conversation history, tool definitions, external knowledge? Completion criteria: Context components identified and designed.
3. **Escalate to Harness Engineering for production** -- Does the Agent need constraint, verification, error recovery? Completion criteria: All five Harness functions addressed.
4. **Escalate to Loop Engineering for autonomy** -- Does the Agent need to discover work, persist across sessions, or self-verify completion? Completion criteria: Task discovery and completion criteria defined.

## B -- Boundary (When NOT to use)
- **When you already know the right layer**: If you are deep in implementation, this framework is for architectural decisions, not implementation details.
- **For model-level decisions**: This framework does not address model selection or fine-tuning -- those are orthogonal.
- **Author blindspot**: The 5-layer model is descriptive, not prescriptive. Real systems may blur boundaries between layers.

## Related Skills
- depends-on: harness-engineering
- composes-with: coding-agent-meta-capability
