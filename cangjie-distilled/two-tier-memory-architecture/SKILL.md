---
name: two-tier-memory-architecture
description: "Trigger when designing a persistent Agent memory system that needs both global overview and precise details, or when Agent memory works for simple recall but fails on proactive service. Key signals: \"Agent memory\", \"cross-session memory\", \"proactive service\", \"structured facts plus retrieval\", \"A..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 3
tags: [memory, architecture, personalization, knowledge-management]
related_skills: []
---

# Two-Tier Memory Architecture

## R -- Reading (Original Quote)
> The Two-Tier Memory Architecture -- Advanced JSON Cards structuring a small number of key facts and keeping them resident in the context as an always-visible overview, Contextual Retrieval fetching details on demand from the vast pool of raw conversations -- is exactly where the two technology lines intersect... Stacked together, the two tiers sharply improve cross-session recall accuracy and conflict resolution.
> -- Li Bojie, Chapter 3

## I -- Interpretation (Methodology Skeleton)
The highest level of Agent memory (Proactive Service) cannot be achieved by any single technology. Resident structured facts alone lose details to capacity limits. Retrieval alone misses hidden cross-session connections for want of a global view. The Two-Tier Architecture solves this: Tier 1 is a small set of key facts stored as structured cards (Advanced JSON Cards with entity, relationship, backstory, and timestamp fields) kept resident in the Agent context as an always-visible overview. Tier 2 is the vast pool of raw conversation history indexed with Contextual Retrieval -- each chunk prefixed with an LLM-generated summary of its context. The Agent uses Tier 1 for global awareness ("User has a Tokyo trip, passport expires in February") and Tier 2 for detail verification ("Let me check the exact passport expiry date and flight details from the original conversations"). One supplies the overview, the other supplies the details; only together do they form the memory core of an assistant that truly knows you.

## A1 -- Past Application (Book Examples)
### Example 1: Travel Coordination (Experiment 3-12)
- **Problem**: Agent needs to warn user that passport is expiring when an international flight is booked months later.
- **Method application**: Tier 1 (Resident): Advanced JSON Cards contain "User Jessica passport expires Feb 18, 2025" and "Tokyo trip booked." Tier 2 (Retrieval): Contextual Retrieval fetches original conversations about passport and flight tickets to confirm details.
- **Conclusion**: The Agent first reviews Tier 1 facts (overview), discovers the risk (flight date near passport expiry), then uses Tier 2 to verify details before proactively warning the user.
- **Result**: Agent proactively suggests "Your passport is about to expire; I strongly recommend expedited renewal."

### Example 2: Contradictory Financial Instructions
- **Problem**: Wife sets up a transfer, husband modifies it, wife modifies it again across three separate calls. Which instruction is valid?
- **Method application**: Tier 2 with Contextual Retrieval adds prefixes to each chunk: "[Wife Patricia is setting up initial transfer]", "[Husband James is modifying previous transfer]", "[Wife is modifying after husband change]". These prefixes provide time, person, and intent context for judging instruction priority.
- **Conclusion**: Without context prefixes, three contradictory chunks are indistinguishable. With them, the Agent can determine which modification is ultimately valid.
- **Result**: Conflict resolution becomes feasible because context metadata disambiguates conflicting records.

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. Building an Agent that needs proactive service (Level 3 memory) -- warning users about risks they have not asked about
2. Agent memory works for simple recall but fails when information from different sessions conflicts
3. Designing the architecture for a personal assistant that "truly knows you"
4. Need both a global user overview and precise detail retrieval

### Language signals (user says things like)
- "My Agent remembers facts but cannot connect them proactively"
- "How do I handle conflicting information across sessions?"
- "I want my Agent to warn users about problems before they ask"
- "How should I structure user memory for a personal assistant?"

## E -- Execution Steps
1. **Design Tier 1 (Resident Structured Facts)** -- Create Advanced JSON Cards for critical user information (identity, key relationships, time-sensitive items). Each card has: fact, person/entity, relationship to user, backstory, timestamp. Completion criteria: Cards cover all critical information; total size fits in context budget (under ~2000 tokens).
2. **Keep Tier 1 resident in context** -- Load structured facts into the system prompt or status bar at session start. Completion criteria: Agent always has global overview available.
3. **Design Tier 2 (Contextual Retrieval)** -- Index raw conversation history with context prefixes (LLM generates a summary prefix for each chunk before embedding). Completion criteria: Each chunk has context metadata (time, person, intent).
4. **Provide search_user_memory tool** -- Agent can call retrieval on Tier 2 for detail verification. Completion criteria: Agent successfully retrieves and disambiguates conflicting records.
5. **Test proactive service scenarios** -- Verify the Agent can discover non-obvious connections (e.g., passport expiry + flight booking) using both tiers. Completion criteria: At least 3 proactive service test cases pass.

## B -- Boundary (When NOT to use)
- **Single-session applications**: If the Agent does not persist memory across sessions, no memory architecture is needed.
- **Simple key-value storage needs**: If users only need "remember my membership number," Tier 1 alone suffices.
- **Author blindspot**: The Advanced JSON Cards format has "higher generation and maintenance overhead." For high-volume, non-critical data, Simple Notes is more cost-effective. The architecture explicitly supports a hybrid: Advanced JSON Cards for critical data, Simple Notes for everything else.

## Related Skills
- depends-on: three-level-memory-evaluation
- depends-on: agentic-rag
- contrasts-with: context-compression-strategy
