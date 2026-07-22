---
name: three-level-memory-evaluation
description: "Trigger when designing or evaluating an Agent memory system, when user asks how to measure memory quality, or when building user personalization. Key signals: \"how good is my memory system\", \"evaluate memory\", \"user preferences\", \"personalization\", \"Agent forgets things\". Do NOT trigger for conte..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 3
tags: [memory, evaluation, personalization, agent-design]
related_skills: []
---

# Three-Level Memory Evaluation Framework

## R -- Reading (Original Quote)
> Level 1: Basic Recall -- accurately store and retrieve information directly provided by the user... Level 2: Multi-Session Retrieval -- gather and reason over all relevant information even when scattered across sessions... Level 3: Proactive Service -- synthesize information across many sessions to offer predictive help, finding deep connections between memories that look unrelated.
> -- Li Bojie, Chapter 3

## I -- Interpretation (Methodology Skeleton)
Before building a memory system, define what "good" means. This framework decomposes memory capability into three progressive levels. Level 1 (Basic Recall): Can the Agent accurately store and retrieve directly-provided structured information? (e.g., "My membership number is 12345"). Level 2 (Multi-Session Retrieval): Can the Agent gather and reason over information scattered across sessions, times, and sources? (e.g., linking flight and hotel bookings for a "Los Angeles trip" composite event). Level 3 (Proactive Service): Can the Agent synthesize across many sessions to offer predictive help, finding deep connections? (e.g., warning that a passport expires soon when an international flight is booked months later). Each level is strictly harder and requires different technology. Most systems stop at Level 1; reaching Level 3 requires structured knowledge management combined with contextual retrieval.

## A1 -- Past Application (Book Examples)
### Example 1: Level 1 -- Basic Recall (Flight Preferences)
- **Problem**: Agent needs to remember user preferences across booking sessions.
- **Method application**: After a conversation about booking a Tokyo flight, the system extracts: "User prefers window seats (preference), User is vegetarian (dietary restriction), United MileagePlus number 12345678 (loyalty program)."
- **Conclusion**: Level 1 ensures basic reliability -- structured facts are stored with tags for retrieval.
- **Result**: Next booking, the Agent does not need to re-ask about seat or meal preferences.

### Example 2: Level 3 -- Proactive Service (Passport Expiry)
- **Problem**: Agent needs to warn users about risks they have not asked about.
- **Method application**: When a user books an international flight in January, the system cross-references with passport information stored months ago, notices the passport expires in February, and proactively recommends expedited renewal.
- **Conclusion**: Level 3 is the acid test of whether an Agent has reached assistant grade -- it requires both global overview and precise details simultaneously.
- **Result**: This is only feasible with the Two-Tier Memory Architecture (resident structured facts + on-demand contextual retrieval).

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. Designing a new Agent memory system and need evaluation criteria
2. Existing memory system "works" but user satisfaction is low -- why?
3. Deciding what memory storage format to use (Simple Notes vs JSON Cards)
4. Benchmarking memory system quality against competitors

### Language signals (user says things like)
- "How do I evaluate if my Agent memory is good enough?"
- "My Agent remembers facts but does not connect them"
- "I want my Agent to proactively help users"
- "What level of memory do I need for my use case?"

## E -- Execution Steps
1. **Classify your use case by level** -- Determine which level(s) your users need. Completion criteria: Document specific Level 1/2/3 scenarios for your domain.
2. **Build Level 1 test cases** -- 20+ cases of direct fact storage and retrieval. Completion criteria: Agent retrieves 95%+ correctly.
3. **Build Level 2 test cases** -- Multi-session scenarios requiring cross-referencing. Completion criteria: Agent links related information across sessions.
4. **Build Level 3 test cases** -- Proactive service scenarios requiring synthesis. Completion criteria: Agent surfaces non-obvious connections without prompting.
5. **Evaluate with LLM-as-a-Judge** -- Use a separate LLM to score Agent responses against reference answers. Completion criteria: Scoring methodology documented and reproducible.

## B -- Boundary (When NOT to use)
- **Single-session applications**: If the Agent does not need to remember anything across sessions, memory evaluation is irrelevant.
- **Simple key-value storage**: If you only need to store and retrieve flat key-value pairs, Level 1 is sufficient -- the full framework is overkill.
- **Author blindspot**: The three levels are progressive but not always cleanly separable. Some Level 2 tasks may require Level 3 capability, and vice versa.

## Related Skills
- depends-on: two-tier-memory-architecture
- composes-with: agentic-rag
