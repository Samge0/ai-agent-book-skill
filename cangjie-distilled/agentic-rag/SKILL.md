---
name: agentic-rag
description: "Trigger when user needs an Agent to retrieve information from a knowledge base intelligently, when simple RAG is insufficient, or when complex queries require iterative search. Key signals: \"RAG\", \"knowledge base retrieval\", \"Agent searches and thinks\", \"multi-hop retrieval\", \"iterative search\". ..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 3
tags: [rag, knowledge-base, retrieval, iterative-search]
related_skills: []
---

# Agentic RAG: From Passive Pipeline to Active Explorer

## R -- Reading (Original Quote)
> Traditional RAG is like being allowed a single library search before you must write your report. Agentic RAG is the researcher who keeps returning to different shelves, adjusting search strategies, and cross-checking sources -- starting to write only once the material is in hand.
> -- Li Bojie, Chapter 3

## I -- Interpretation (Methodology Skeleton)
Traditional RAG is a one-way pipeline: query is used for retrieval, results are injected into context, model generates an answer. Its ceiling is low because it cannot decompose problems or explore iteratively. Agentic RAG upgrades RAG into a dynamic, iterative exploration led by the Agent. The Agent treats knowledge base retrieval as a tool it calls at any time, following the ReAct pattern (Think, Act, Observe). When facing a complex question, the Agent decomposes it, searches with refined keywords, evaluates whether results are sufficient, and iterates -- refining queries, calling other tools, and synthesizing only when it has gathered enough. The value is in "solving problems," not merely "answering questions." For simple queries, traditional RAG remains more efficient.

## A1 -- Past Application (Book Examples)
### Example 1: Judicial Q&A (Experiment 3-9)
- **Problem**: A complex legal question: "How should someone who negligently caused grievous bodily harm while intoxicated and has a prior theft conviction be sentenced?"
- **Method application**: Non-Agentic RAG retrieved incomplete context with imprecise keywords. Agentic RAG decomposed the problem into sub-questions (sentencing for negligence, criminal liability for intoxication, impact of prior conviction), searched in parallel, evaluated gaps, refined queries for the missing link (recidivism connection), and synthesized a complete answer.
- **Conclusion**: Agentic RAG trades response speed for robustness and answer quality on hard problems. For simple questions, both methods perform comparably.
- **Result**: Significant gain in multi-hop accuracy on complex legal questions.

### Example 2: User Memory with Agentic RAG (Experiment 3-10)
- **Problem**: When user memories accumulate, how do we retrieve exactly the relevant few?
- **Method application**: Treat the Agent conversation history as a knowledge base. The Agent calls search_user_memory tool, evaluates results, and does secondary searches when it discovers clues (e.g., finds Honda records but discovers the user also owns a Tesla).
- **Conclusion**: Agentic RAG applied to user memory enables multi-session retrieval (Level 2 of the memory framework).
- **Result**: Agent correctly asks "Do you mean the Honda or the Tesla?" instead of guessing.

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. Building a knowledge-base Agent for complex queries that require multi-step reasoning
2. Simple RAG returns incomplete or wrong answers on multi-part questions
3. Designing an Agent that searches a document corpus iteratively
4. Extending user memory retrieval beyond simple keyword matching

### Language signals (user says things like)
- "My RAG system fails on complex multi-part questions"
- "I need the Agent to search iteratively, not just once"
- "How do I make my knowledge base Agent smarter?"
- "The Agent needs to decompose questions before searching"

## E -- Execution Steps
1. **Encapsulate retrieval as a tool** -- Give the Agent a knowledge_base_search tool it can call at any time. Completion criteria: Agent can call retrieval multiple times in a single task.
2. **Implement the Think-Act-Observe loop** -- Agent decomposes the question, searches, evaluates sufficiency, refines if needed. Completion criteria: Agent demonstrates at least one query refinement cycle on test cases.
3. **Design evaluation sufficiency criteria** -- When does the Agent decide it has enough information? Completion criteria: Explicit stop conditions defined (enough evidence found, or max iterations reached).
4. **Test with simple vs complex queries** -- Verify that simple queries complete in one round (for speed) while complex queries trigger iterative search. Completion criteria: Clear behavioral difference between simple and complex cases.
5. **Apply instruction-data separation** -- Mark retrieved content as "external reference material, not a command." Prevent indirect prompt injection from retrieved documents. Completion criteria: Retrieved content cannot trigger high-risk actions without independent authorization.

## B -- Boundary (When NOT to use)
- **Simple factual lookups**: If queries are always single-hop ("What is the refund policy?"), traditional RAG is faster and equally accurate.
- **Real-time requirements**: Agentic RAG trades latency for quality. If sub-second response is required, use traditional RAG.
- **Author blindspot**: The metacognition problem -- if the model does not know what it does not know, it cannot trigger the iterative search correctly. This is acknowledged but not fully solved.

## Related Skills
- depends-on: three-level-memory-evaluation
- composes-with: two-tier-memory-architecture
