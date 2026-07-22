---
name: progressive-disclosure-skills
description: "Trigger when an Agent system prompt is too long, tool list is growing unmanageably, or user needs to modularize Agent capabilities. Key signals: \"system prompt is too long\", \"too many tools in context\", \"Agent skills\", \"dynamic instruction loading\", \"modular capabilities\". Do NOT trigger when the..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 2 (reinforced Chapter 4)
tags: [context-engineering, modularization, scalability, skills]
related_skills: []
---

# Progressive Disclosure via Agent Skills

## R -- Reading (Original Quote)
> Like consulting a reference book or Wikipedia. Nobody reads a handbook or all of Wikipedia cover to cover; you follow the index and the table of contents, looking up exactly the entry you need, when you need it. Tool definitions likewise need not live permanently in the context... At startup the Agent sees only a thin catalog -- each skill name and description, a few hundred tokens in total.
> -- Li Bojie, Chapter 2

## I -- Interpretation (Methodology Skeleton)
Progressive Disclosure is a context engineering pattern where detailed instructions are loaded only when the current context genuinely demands them. The mechanism: (1) At startup, the Agent sees only a thin catalog -- each skill name and description (a few hundred tokens total). (2) When the current task calls for a capability, the model reads the corresponding skill file. (3) The skill file may contain references to deeper sub-documents, enabling layer-by-layer drill-down. This solves two problems simultaneously: context bloat (too many instructions in the prompt) and tool explosion (too many tool definitions in context). The Agent uses general file-reading ability to browse the skill directory -- no vector index, no semantic retrieval infrastructure needed. It is how humans actually use reference material: look up what you need, when you need it.

## A1 -- Past Application (Book Examples)
### Example 1: Dynamic Prompt Loading at Pine AI
- **Problem**: As the Agent gained more capabilities, the system prompt grew to contain all operational instructions for all tasks, causing context bloat and degrading model performance.
- **Method application**: Long before "Skill" became a buzzword, Pine AI used dynamic prompt loading -- detailed instructions for specific tasks are stored as files, loaded only when the Agent encounters a task that needs them.
- **Conclusion**: This rein in runaway prompt growth while maintaining capability coverage.
- **Result**: The Agent handles diverse tasks without carrying all instructions in context at all times.

### Example 2: Tool Discovery via Skills (Chapter 4)
- **Problem**: When tools number in the hundreds or thousands, injecting all tool schemas into the system prompt overwhelms both context and model capability.
- **Method application**: Skills inverts the tool discovery paradigm -- instead of embedding-index-plus-semantic-matching infrastructure, the Agent uses general file-reading to browse a skill directory organized like a filesystem. At startup only skill names and descriptions are visible.
- **Conclusion**: The "embedding index + semantic matching" infrastructure disappears entirely. The Agent needs nothing beyond general file-reading ability.
- **Result**: A more modern, lower-maintenance way to discover tools -- 98% token savings on ~2800 tools reported by MCP-Zero.

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. System prompt exceeding 2000+ tokens with instructions for tasks that are rarely all needed at once
2. Tool list growing past 50-100 tools, degrading model selection accuracy
3. Need to add new capabilities without bloating the base context
4. Building a modular Agent where different deployments need different capability sets

### Language signals (user says things like)
- "My system prompt is too long and keeps growing"
- "I have too many tools for the model to pick the right one"
- "How do I modularize my Agent capabilities?"
- "Can I load instructions dynamically?"

## E -- Execution Steps
1. **Catalog existing capabilities** -- List all instructions, tools, and knowledge in your current system prompt. Completion criteria: Complete inventory with usage frequency.
2. **Create skill files** -- Move detailed instructions into separate SKILL.md files, each with name, description, and full instructions. Completion criteria: Each skill file is self-contained and individually useful.
3. **Build the thin catalog** -- Replace detailed instructions in the system prompt with a one-line name+description for each skill. Completion criteria: System prompt reduced to under 1000 tokens of skill catalog.
4. **Provide file-reading tools** -- Ensure the Agent can read files (grep, read_file) to access skill details. Completion criteria: Agent can discover and load any skill on demand.
5. **Test trigger accuracy** -- Give the Agent tasks that require different skills; verify it loads the right one. Completion criteria: Agent correctly identifies needed skills in 80%+ of test cases.

## B -- Boundary (When NOT to use)
- **Small, stable capability set**: If your Agent has fewer than ~15 tools and a system prompt under 2000 tokens, the overhead of skill files is not justified.
- **Real-time/ultra-low-latency**: Progressive Disclosure adds one file-read round-trip per skill lookup. If millisecond latency matters, keep everything inline.
- **Author blindspot**: The "metacognition problem" -- if the model does not know what it does not know, it cannot correctly trigger loading a skill. This is acknowledged but not fully solved.

## Related Skills
- depends-on: kv-cache-friendly-context
- composes-with: proactive-tool-discovery
