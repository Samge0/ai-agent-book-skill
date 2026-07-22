# Chapter 3: User Memory and Knowledge Base

## Core Idea
Persistent memory makes an Agent a "personal assistant who knows you" `user memory` and a "domain expert" `knowledge base`. Memory is not a transcript — it is an active, continuous learning process that builds a concise predictive model of the user.

## Frameworks Introduced

- **Three-Level Memory Evaluation**:
  1. **Basic Recall**: Accurately store/retrieve directly-provided structured info `"My membership number is 12345"`
  2. **Multi-Session Retrieval**: Gather and reason over scattered info across sessions, parties, and times
  3. **Proactive Service**: Synthesize old information to offer predictive help `passport expiring, tax documents`
  - When to use: Evaluating any memory system before designing it
  - How: 20 test cases per level; LLM-as-judge scoring against reference answers

- **Four Storage Formats** `progressive complexity`:
  1. **Simple Notes**: Minimal indivisible facts; O(1) operations; loses associations
  2. **Enhanced Notes**: Paragraph with complete context; rich semantics; storage redundancy
  3. **JSON Cards**: Three-level nested `Category/Subcategory/Key-Value`; supports partial updates
  4. **Advanced JSON Cards**: Adds backstory, person, relationship, timestamp; solves disambiguation
  - When to use: Choosing memory representation
  - How: Advanced JSON Cards for critical low-volume data; Simple Notes for large-volume non-critical facts

- **RAG Pipeline**: Chunking, then embedding, then retrieval, then generation
  - When to use: Building any knowledge acquisition system
  - How: Dense `semantic` + Sparse `BM25 keyword` = Hybrid retrieval; best of both worlds

- **Contextual Retrieval**: Add context to each chunk before embedding
  - When to use: When chunks lose meaning without surrounding context
  - How: LLM generates 50-100 token context prefix per chunk; reduces retrieval failure rate by ~49%

- **Agentic RAG**: Agent decides when to search, what query, and how many rounds
  - When to use: Complex queries requiring multi-hop reasoning
  - How: Search becomes a tool call; the Agent iterates until it has enough information

## Key Concepts
- **Trajectory**: Complete historical record of a single Agent run — append-only, never modified
- **User Long-Term Memory**: Persistent storage across sessions, bound to user ID via key-value pairs
- **LoCoMo**: Long-term Conversational Memory benchmark — ~300 turns across up to 35 sessions
- **Dense Embedding**: Maps text to high-dimensional vectors for semantic similarity `Word2Vec to context-aware`
- **Sparse Embedding**: BM25 — keyword-based exact match; term frequency × inverse document frequency
- **Structured Indexing**: Organize knowledge with hierarchical directory structures, not flat text
- **Filesystem Paradigm**: Organize knowledge like a filesystem — directories as semantic categories
- **User as Code**: Memory as executable Python objects with typed constraints `write-ahead log + checkpoint`

## Mental Models
- **Think of memory as a predictive model, not a transcript**: You do not remember every conversation, but you form a vivid mental model of each person
- **Think of Advanced JSON Cards as contacts in a CRM**: Each card has context `backstory`, identity `person`, relationship — not just raw facts
- **Use Advanced JSON Cards for critical relationships, Simple Notes for transient facts**: Most production systems use a hybrid

## Anti-patterns
- **Treating memory as a raw transcript**: The Agent must select, abstract, and structure — not store everything
- **Using only dense retrieval**: Misses exact keyword matches; always combine with sparse/BM25
- **Flat knowledge base without structure**: Semantic retrieval struggles when knowledge is unorganized
- **Ignoring memory conflicts**: New information contradicting old must trigger update/merge/prune, not blind append

## Code Examples
```python
# Memory extraction example
# User says: "Help me book a flight to Tokyo. I prefer window seats and I am vegetarian."

# After conversation ends, framework calls LLM to extract:
# - User prefers window seats `preference`
# - User is vegetarian, needs special meals `dietary restriction`
# - User has travel plans to Tokyo `recent activity`

# Selectivity: transient info `"3 search results"` is NOT stored
# Abstraction: "I prefer window seats" becomes general preference
# Structure: each memory tagged with type for retrieval
```
- **What it demonstrates**: Memory extraction requires selectivity `not everything is stored`, abstraction `general patterns, not specific instances`, and structure `typed tags for retrieval`.

## Reference Tables

### Four Storage Formats Trade-off
| Format | Simplicity | Expressiveness | Update Cost | Best For |
|--------|-----------|----------------|-------------|----------|
| Simple Notes | Highest | Lowest | O(1) | Large-volume transient facts |
| Enhanced Notes | Medium | Medium | Rewrite paragraph | Nuanced understanding |
| JSON Cards | Medium | High | Partial update | Cleanly categorizable data |
| Advanced JSON Cards | Lowest | Highest | Complex | Critical disambiguation data |

### Dense vs Sparse vs Hybrid Retrieval
| Type | Strength | Weakness | Example |
|------|----------|----------|---------|
| Dense `semantic` | Understands synonyms, concepts | Misses exact keywords | "car" matches "automobile" |
| Sparse `BM25` | Exact keyword match | No semantic understanding | "J/Q/K" matches literally |
| **Hybrid** | **Both** | Slightly more complex | **Best of both — production default** |

## Worked Example
**User books a flight**: Conversation reveals preferences. After the session, the framework extracts: window seat preference, vegetarian dietary restriction, United MileagePlus number. Next time the user books, the Agent does not ask about seats or meals — this info is already in memory. The memory system turned scattered conversation details into a predictive model of the user.

## Key Takeaways
1. **Set evaluation criteria before designing memory**: Use the three-level framework to know what "good" means.
2. **Memory is selective, abstract, and structured**: Not a transcript — a predictive model built through active learning.
3. **Use hybrid retrieval `dense + sparse`**: Pure dense misses keywords; pure sparse misses semantics.
4. **Contextual Retrieval dramatically improves chunk relevance**: Adding 50-100 token context per chunk reduces failure by ~49%.
5. **Agentic RAG turns search into a tool call**: The Agent decides when/what/how-many to search, enabling multi-hop reasoning.
6. **Choose storage format by data criticality**: Advanced JSON Cards for critical low-volume; Simple Notes for large-volume transient.
7. **Knowledge bases need governance**: Timeliness, conflict resolution, and privacy protection `log sanitization` are essential.

## Connects To
- **Ch 2**: Context engineering — memory is persistent context across sessions
- **Ch 4**: Perception tools — search and retrieval as tool calls
- **Ch 6**: Evaluation — three-level framework drives the evaluation design
- **Ch 8**: Self-evolution — sleep consolidation as autonomous memory evolution
