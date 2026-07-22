---
name: sleep-learning-memory-consolidation
description: "Trigger when building an Agent whose user memory should improve autonomously over time, or when the user asks how to make an Agent \"remember and learn from past conversations\" across sessions. Implements offline memory consolidation (sleep learning). Do NOT trigger for within-session context comp..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 8
tags: [self-evolution, user-memory, cross-session, consolidation]
related_skills: [externalized-learning-three-products, three-level-memory-evaluation, two-tier-memory-architecture]
---

# Sleep Learning: Autonomous Evolution of User Memory

## R -- Reading (Original Quote)
> The day's scattered experiences are reorganized during sleep, stripped of redundancy, woven into the existing knowledge network, and emerge as long-term memory that is more compact and easier to recall... the most natural object of this offline consolidation is the Agent's memory of the user -- who you are, what you prefer, what facts you've mentioned.
> -- Li Bojie, Chapter 8

## I -- Interpretation (Methodology Skeleton)
Sleep Learning is an offline consolidation process for user memory. During active sessions, an Agent accumulates raw interaction data -- scattered, redundant, sometimes contradictory. Just as human memory consolidates during sleep, the Agent needs a periodic offline phase where raw memories are reorganized: stripped of redundancy, resolved of conflicts, merged with existing knowledge, and emerged as compact long-term memory. The key distinction: this is about USER MEMORY (a model of who the user is), not a shared knowledge base (domain documents loaded via RAG). The consolidation must happen autonomously -- without user intervention -- so that the Agent "comes to know you better over time."

## A1 -- Past Application (Book Examples)
### Example 1: Claude Code's Markdown User Memory
- **Problem**: Claude Code stores user preferences in markdown files that grow messy over sessions.
- **Method application**: A sleep consolidation pass reads all raw markdown notes, identifies duplicates, resolves conflicts (e.g., "user said they use Python in March, then said Go in May"), and rewrites a clean consolidated memory file.
- **Conclusion**: The consolidated memory is more compact and accurate than the raw accumulation.
- **Result**: Next session, the Agent retrieves preferences faster and with fewer contradictions.

### Example 2: Hermes Memory Evolution
- **Problem**: Hermes accumulates user memory entries across sessions; some become stale.
- **Method application**: During idle periods, a consolidation process clusters related entries, detects stale facts, and merges them into a coherent user profile.
- **Conclusion**: The memory grows more accurate with use, not just larger.
- **Result**: User does not need to repeat preferences; the Agent "remembers."

### Example 3: Conflict Versioning
- **Problem**: User says "I use Visual Studio Code" in January, then "I switched to Neovim" in March.
- **Method application**: Sleep consolidation detects the temporal conflict, keeps the latest as active, archives the old as historical.
- **Conclusion**: The memory reflects current state, not a contradictory accumulation.
- **Result**: Agent recommends Neovim plugins, not VS Code extensions.

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. Building an Agent that should "get to know the user better" over multiple sessions
2. Designing a memory system where raw notes need periodic cleanup
3. An Agent whose memory has grown stale, redundant, or contradictory
4. Implementing cross-session personalization

### Language signals (user says things like)
- "How do I make my Agent remember things from past conversations?"
- "My Agent keeps asking me the same questions -- can it learn?"
- "How does Hermes/Claude Code store user memory?"
- "I want my Agent to get smarter the more I use it"

## E -- Execution Steps
1. **Identify consolidation triggers** -- Completion criteria: define WHEN consolidation runs (idle periods, session end, threshold-based when raw entries exceed N)
2. **Detect redundancy and conflicts** -- Completion criteria: clustering algorithm groups related entries; temporal logic identifies contradictions (newest wins)
3. **Merge and compress** -- Completion criteria: consolidated memory is smaller than raw accumulation; no information loss on active facts
4. **Archive history** -- Completion criteria: superseded facts are archived with timestamps, not deleted
5. **Validate** -- Completion criteria: three-level memory evaluation (recall, evolution, redundancy) shows improvement

## B -- Boundary (When NOT to use)
- Within-session memory (use context compression instead)
- Domain knowledge bases loaded via RAG (those are batch-loaded, not user-specific)
- One-time fact ingestion (no consolidation needed for a single entry)
- Real-time learning during task execution (that is in-context learning or externalized learning, not sleep consolidation)

## Related Skills
- depends-on: three-level-memory-evaluation (defines how to measure if consolidation improved memory)
- complements: externalized-learning-three-products (sleep consolidation is the "KB entry" evolution path)
- contrasts-with: two-tier-memory-architecture (that defines storage structure; this defines the evolution process)
