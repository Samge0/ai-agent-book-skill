---
name: externalized-learning-three-products
description: "Trigger when an Agent needs to accumulate experience without changing model weights. Classifies learnings into three products: knowledge base entries (facts), code tools (procedures), and skill documents (strategies). Do NOT trigger for model fine-tuning decisions or RAG-only knowledge management..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 8
tags: [self-evolution, externalized-learning, knowledge-management, tools]
related_skills: [sft-memorizes-rl-generalizes, proposer-reviewer-new-information]
---

# Externalized Learning: Three Products

## R - Reading
> "A customer service Agent helping a client with a refund might learn three things: a specific rule ('Company A's refunds require verifying the last four digits of the credit card') - factual knowledge for the knowledge base; a general-purpose tool ('use API X to query order status') - a stable, reusable operation frozen into a code tool; a job manual for the refund process - a Skill document. Rule of thumb: purely factual information goes in the knowledge base; frequently used, parameter-heavy procedures become code; frequently changing processes with strategic judgment become documents."
> - Li Bojie, Chapter 8

## I - Interpretation
Learning does not happen automatically - the attention mechanism retrieves but does not distill. An Agent must explicitly compress experience into reusable artifacts. The three products serve different purposes and have different lifecycles. Knowledge base entries are searchable facts that may go stale. Code tools are deterministic, testable, and permanent - they eliminate rethinking. Skill documents capture evolving strategies that require human-like judgment. The key insight: rather than expecting the model to remember everything, spend extra compute after each task to summarize, compress, and structure experience into the appropriate external carrier. This is faster and more interpretable than parameter learning.

## A1 - Past Application
### Example 1: GAIA Experience Strategy Summary
- **Problem**: Agent successfully completes a GAIA task but the experience is lost when the session ends
- **Method application**: After each successful task, the system captures the complete trajectory and uses an LLM to reflect and summarize: what methods were used, what pitfalls were encountered, key steps. Summary is vectorized and stored.
- **Conclusion**: When encountering similar problems, the Agent retrieves relevant past experiences from the knowledge base and injects them as success examples into the system prompt
- **Result**: Self-evolving system on positive feedback - more tasks solved means richer experience and stronger capabilities

### Example 2: Voyager Lifelong Skill Library
- **Problem**: How does an Agent accumulate skills continuously in an open world (Minecraft)?
- **Method application**: Every successful action sequence is distilled into executable code and stored in a hierarchical, composable skill library. "Craft a wooden pickaxe" calls foundational skills like "chop a tree."
- **Conclusion**: New tasks are solved by retrieving and combining existing skills, sparing the LLM from thinking from scratch. Skills are permanent code tools, not transient context.
- **Result**: 3.3x more unique items discovered than baseline methods; technology tree milestones reached significantly faster

## A2 - Future Trigger
### Scenarios
1. Your Agent solves a problem through 15 minutes of exploration, and you want it to remember the solution for next time
2. Your Agent repeatedly performs the same multi-step procedure (query API, process data, format report) and you want to automate it
3. Your customer service Agent discovers a new business rule that isn't in any documentation
### Language signals
- "How should the Agent remember this experience?"
- "The Agent keeps re-solving the same problem from scratch"
- "Can the Agent build its own tools?"

## E - Execution Steps
1. **Classify the learning** - Is it a fact (specific rule/data)? A procedure (repeatable operation sequence)? A strategy (complex, evolving workflow)? Criteria: classification determines the storage format.
2. **Distill into the appropriate product** - Facts: structured knowledge base entry with semantic tags. Procedures: code tool with parameters and error handling. Strategies: Skill document with decision trees and edge cases. Criteria: each artifact is self-contained and retrievable.
3. **Store with metadata** - Tag with source task, creation date, confidence level, and expiration conditions. Criteria: every entry is traceable and can be evicted when stale.
4. **Apply on retrieval** - When a new task arrives, search for relevant past learnings. Inject knowledge base entries as context, call code tools directly, load Skill documents on demand. Criteria: retrieval precision is measured and optimized.
5. **Review and refresh** - Periodically review stored artifacts for accuracy and freshness. Outdated entries should be updated or evicted. Criteria: knowledge freshness mechanism prevents obsolete information from causing errors.

## B - Boundary
- Externalized learning cannot teach the model new reasoning patterns - those require post-training (Ch7)
- Knowledge base entries can become stale as business rules change - need a freshness mechanism
- Code tools created by the Agent may have quality issues at edge cases - need testing before reuse
- Supply chain risk: tools created from open-source libraries may contain malicious code (sandbox and scan)
- Memory/experience poisoning: content touched during tasks may carry prompt injection that becomes persistent

## Related Skills
- depends-on: sft-memorizes-rl-generalizes
- complements: proposer-reviewer-new-information
