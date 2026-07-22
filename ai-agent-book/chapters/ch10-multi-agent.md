# Chapter 10: Multi-Agent Collaboration

## Core Idea
The intelligence of a group can exceed any individual. But multi-agent is not always better — the core criterion is: **does the collaboration introduce new information that a single Agent could not obtain?** Without new information, multi-agent is equivalent to a single Agent with equal compute.

## Frameworks Introduced

- **Two-Dimension Classification**:
  - **Dimension 1**: Shared vs Non-Shared Context `how information passes between Agents`
  - **Dimension 2**: Collaboration Topology `how control and information flow`
  - When to use: Designing any multi-agent system
  - How: 2x3 matrix; shared context mostly degenerates into role switching; focus on non-shared cells

- **Three Topologies** `under non-shared context`:
  1. **Peer Collaboration**: 2-3 Agents as equals, iterative improvement loop
  2. **Manager Pattern**: Centralized Manager coordinates multiple sub-agents
  3. **Decentralized Pattern**: No central controller; peer-to-peer handoff
  - When to use: Choosing collaboration architecture
  - How: Peer for mutual checks; Manager for 5+ sub-tasks; Decentralized for scalability

- **"New Information" Criterion**: Multi-agent is truly better only when collaboration introduces new info
  - Self-review by same model: No new info -> usually ineffective
  - Reviewer uses test results/screenshots/external tools: Yes -> significant improvement
  - When to use: Deciding if multi-agent is worth the cost
  - How: Check if external feedback `execution, rendering, tool verification` enters the loop

- **Proposer-Reviewer Paradigm**: One generates, another reviews with external feedback
  - When to use: Quality-critical tasks
  - How: Reviewer value = NEW information `rendered screenshots, test results`; not re-reading

- **Loop Engineering**: Design loops that keep the Agent running; verifier decides when to stop
  - When to use: Preventing premature termination `lazy fake-done, premature give-up, false success`
  - How: "The bottleneck of the loop is the verifier, not the model"

- **Failure Modes**:
  1. **Concurrency Conflicts**: File-level write conflicts `lost update`; semantic conflicts `cross-file consistency`
  2. **Error Cascading**: One Agent error reinforced by subsequent Agents `telephone game`
  3. **Cost Explosion**: Multi-agent uses several-fold to order-of-magnitude more tokens
  - When to use: Debugging multi-agent systems
  - How: Optimistic locking for file conflicts; cross-validation for error cascading; budgets for cost

## Key Concepts
- **Shared Context**: Subsequent Agent inherits complete trajectory — zero info loss, rapid context expansion
- **Non-Shared Context**: Each Agent maintains independent context — better modularity and isolation
- **Handoff Package**: Task description + confirmed facts/constraints + references to structured artifacts
- **Optimistic Locking**: Version number check on write; fail and re-read if modified by another
- **Working Copy Isolation**: Each Agent gets independent Git branch/worktree; conflicts deferred to merge
- **MAST Taxonomy**: 14 failure modes in 3 groups `design flaws, alignment failures, missing verification`
- **Agent Card** `A2A Protocol`: Metadata document declaring Agent capabilities for cross-org discovery
- **Emergent Behavior**: Collective behaviors arising from many Agents that no one designed
- **Agent Society**: Social, economic, and strategic gameplay emergence at scale

## Mental Models
- **Think of shared context as a team around one table**: Everyone hears everything; zero info loss but context balloons
- **Think of non-shared context as departments collaborating by email**: Each has own workspace; exchange through documents
- **Think of the "new information" criterion as the litmus test**: If no new info enters, multi-agent = single agent with more compute
- **Think of error cascading as the telephone game**: Information becomes increasingly distorted through the chain

## Anti-patterns
- **Same-model debate without external feedback**: Processes identical textual information; can only lose info, not create it
- **Manager as weakest link**: A weak planner is the most critical bottleneck; give strongest model to the Manager
- **No concurrency control in shared file systems**: Lost updates and semantic conflicts corrupt results
- **No verification gate**: Agent claims "completed" but result does not meet requirements

## Code Examples
```
# Error cascading example: Translation system

# Terminology Agent: translates "reasoning" as word-A
#     -> writes to glossary.json
# Translation Agent A: translates Ch 2, reads glossary
#     -> uses word-A for "reasoning tokens"
# Translation Agent B: translates Ch 7
#     -> uses word-A for "inference latency"  WRONG — different concept!
#     -> writes to each chapter
# Proofreading Agent: sees word-A used "consistently"
#     -> concludes translation is high quality  WRONG

# Fix: cross-validation — independent perspective re-examines
# conclusion against original evidence, ignoring reasoning chain.
```
- **What it demonstrates**: A single error gains higher credibility through "consistency" after propagating through multiple Agents. Cross-validation breaks the chain by having an Agent re-examine from an independent perspective.

## Reference Tables

### Shared vs Non-Shared Context Selection
| Criterion | Shared Context | Non-Shared Context |
|-----------|---------------|-------------------|
| Number of sub-tasks | Few `2-3 roles` | Many `parallel needed` |
| Context window | Can accommodate all roles | Single window insufficient |
| Parallelism | Primarily serial | Massively parallel |
| Information isolation | Not needed | Needed `security review` |
| Cost | Tokens accumulate stage by stage | Several times to order of magnitude higher |

### Collaboration Topologies Comparison
| Topology | Complexity | Scalability | Single Point of Failure | Best For |
|----------|-----------|-------------|------------------------|----------|
| **Peer** | Low | 2-3 Agents | No | Mutual checks, iterative improvement |
| **Manager** | Medium | 5+ sub-tasks | Manager is bottleneck | Complex task decomposition |
| **Decentralized** | High | Many Agents | No `but coordination hard` | Scalability, peer-to-peer handoff |

### "New Information" in Collaboration Modes
| Mode | New Info? | Effect |
|------|-----------|--------|
| Self-review `re-reading` | No | Usually ineffective |
| Same-text debate | No | Comparable to single Agent |
| Reviewer + test results | Yes | Significant improvement |
| Reviewer + screenshots | Yes | Significant improvement |
| Reviewer + external tools | Yes | Significant improvement |

## Worked Example
**Book Translation Agent** `Manager Pattern`: Translating a technical book requires terminology consistency across chapters. Single Agent: context overflows, Agent gets lost, terminology drifts. Manager solution: Glossary Agent extracts terms and writes to shared file; Translation Agents `parallel instances` each translate one chapter using the glossary; Proofreading Agent checks consistency. Manager only stores file indexes, not translation content — context grows linearly with sub-tasks, not explosively.

## Key Takeaways
1. **Multi-agent is better only when collaboration introduces new information**: Self-review and same-text debate do not qualify.
2. **Use shared context for few roles with zero info loss; non-shared for many parallel tasks**: Heuristic: if cumulative context exceeds 50% of window, do not share.
3. **Give the strongest model to the Manager/Planner**: A weak planner is the most critical bottleneck.
4. **Proposer-Reviewer works because of new information**: Screenshots, test results, external verification — not re-reading.
5. **Use optimistic locking for file conflicts, working copy isolation for codebases**: Prevent lost updates; defer conflicts to merge.
6. **Cross-validation breaks error cascading**: Independent perspective re-examines conclusion against original evidence.
7. **Cost must justify the overhead**: Multi-agent uses several-fold more tokens; gains must cover the overhead.

## Connects To
- **Ch 1**: Loop Engineering — the fifth paradigm layer; constraints among multiple Agents
- **Ch 2**: Sub-Agent Context Isolation; compression vs isolation trade-off
- **Ch 4**: Collaboration tools; sub-agent context passing strategies; A2A vs MCP
- **Ch 5**: Proposer-Reviewer pattern; Loop Engineering against premature termination
- **Ch 8**: Loop Engineering as self-evolution across runs; "the verifier, not the model, is the bottleneck"
