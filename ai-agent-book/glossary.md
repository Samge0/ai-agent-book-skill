# Glossary

> Key terms from "Deep Understanding of AI Agents," alphabetically organized with chapter references.

## A

- **A2A Protocol** — Agent2Agent protocol by Google for cross-organizational Agent collaboration; uses Agent Cards for capability discovery, opaque collaboration, and task lifecycle management `Ch 10`
- **Ablation Infrastructure** — Systematically removing/restoring features to measure their true contribution to performance `Ch 6`
- **ACI** `Agent-Computer Interface` — Designing tool interfaces from the Agent perspective, not the programmer perspective; analogous to HCI `Ch 1, 4`
- **Action Chunking** — Model generates a short sequence of future actions per inference; control thread replays at high frequency to bridge the VLA frequency gap `Ch 9`
- **Adaptive Windowing** — Compression strategy that preserves original info until 80% of context window, then batch-compresses `Ch 2`
- **Advanced JSON Cards** — Memory storage format with backstory, person, relationship, and timestamp; solves disambiguation `Ch 3`
- **Agent Status Bar** — Framework-injected meta-information at end of context; converts implicit state into explicit, directly-retrievable knowledge `Ch 2`
- **Agentic RAG** — Agent decides when to search, what query, and how many rounds; search becomes a tool call `Ch 3`
- **Attention Sink** — First token of sequence absorbs abnormally high attention weight `70%+`; mathematical consequence of softmax `Ch 2`
- **A/B Testing** — Distinguishing mechanism `what changed` from goal `what improved` in Agent systems `Ch 6`

## B

- **Bitter Lesson** `Sutton` — General methods that scale with compute/data eventually outperform hand-crafted domain knowledge `Ch 1`
- **BM25** — Sparse embedding method for keyword-based exact match retrieval; term frequency times inverse document frequency `Ch 3`

## C

- **Cascaded Pipeline** — Voice architecture: VAD then ASR then LLM then TSS in serial; 0.9-2s latency `Ch 9`
- **Chat Template** — The "envelope format" converting structured API messages into linear token stream using special tokens `Ch 2`
- **Code as Meta-Capability** — Code generation as the ability to create new tools and capabilities dynamically at runtime `Ch 5`
- **Context Distillation** — Pre-computing conclusions into explicit knowledge the model can directly retrieve `Ch 2`
- **Context Engineering** — Systematic management of the model working context `system prompt, tool defs, history, knowledge` `Ch 1, 2`
- **Context Rot** — Context fits in window but cannot be found; quality quietly deteriorates as length increases `Ch 2`
- **Contextual Retrieval** — Adding 50-100 token context prefix to each chunk before embedding; reduces retrieval failure by ~49% `Ch 3`
- **Covariate Shift** — Offline imitation error grows as T-squared; student deviates from demonstrations and enters unseen states `Ch 7`

## D

- **DPO** `Direct Preference Optimization` — Offline method turning preference pairs into classification loss with implicit reward `Ch 7`
- **Dense Embedding** — Maps text to high-dimensional vectors for semantic similarity matching `Ch 3`

## E

- **Elo Rating** — Ranking system quantifying relative ability through pairwise matchups; based on Bradley-Terry model `Ch 6`
- **Event-Driven Architecture** — Unifying all inputs as an event stream; Agent responds to events instead of polling `Ch 4`
- **Externalized Learning** — Capturing knowledge in knowledge bases, tools, and Skills; persistent + interpretable; cost = hours `Ch 1, 8`

## F

- **Fast-Slow Thinking Separation** — Decoupling real-time interaction from deep reasoning; fast model maintains conversation, slow model reasons in background `Ch 9`
- **Full-Duplex Voice** — Model listens and speaks simultaneously; eliminates turn-taking entirely `Ch 9`

## G

- **Goodhart Law** — When a metric becomes an optimization target, it ceases to be a good metric `Ch 6`
- **GRPO** `Group Relative Policy Optimization` — RL algorithm sampling N trajectories per question; uses intra-group relative comparison; no value network `Ch 7`

## H

- **Handoff Package** — What is passed between Agents without shared context: task description + confirmed facts + artifact references `Ch 10`
- **Harness Engineering** — All infrastructure outside the model: context management, tool interfaces, safety constraints, verification, correction `Ch 1`
- **Hooks/Cron/Heartbeat** — Three automation mechanisms for event-driven Agents: lifecycle events, scheduled tasks, periodic checks `Ch 4`
- **Hybrid Retrieval** — Combining dense `semantic` and sparse `BM25 keyword` retrieval for best of both worlds `Ch 3`

## I

- **In-Context Learning** — Adapting on the fly through pattern retrieval within context; fast but transient; more retrieval than reasoning `Ch 1, 2`

## K

- **KV Cache** — Stores intermediate key-value states so later computation can reuse them; prerequisite is prefix stability `Ch 2`

## L

- **Latent Bridge** — Small parameter bridge projecting slow model hidden states into fast model input; bypasses text round-trip `Ch 9`
- **Lethal Triad** — Three security elements forming complete attack loop: private data access + untrusted content + external communication `Ch 5`
- **LLM-as-Judge** — Using a language model to evaluate output quality based on expert-defined Rubric `Ch 6`
- **Loop Engineering** — Designing loops that keep the Agent running; verifier decides when to stop; "the bottleneck is the verifier, not the model" `Ch 1, 10`

## M

- **Manager Pattern** — Centralized Manager Agent coordinates multiple sub-agents; Agents modeled as tools the Manager invokes `Ch 10`
- **MCP** `Model Context Protocol` — Open standard for Agent-tool communication; universal socket standard for AI tools `Ch 4`
- **MGRD** `Modality-Grounded Reasoning Distillation` — Training model to reason from acoustic features, not text transcripts `Ch 9`
- **Mode-seeking vs Mass-covering** — RL concentrates probability on highest-reward modes `mode-seeking`; SFT spreads across all modes `mass-covering` `Ch 7`
- **MPS** `Mind-Paced Speaking` — Dual-brain architecture: Formulation Brain thinks, Articulation Brain speaks in parallel `Ch 9`

## O

- **Omni Model** — End-to-End Omnimodal voice model; single model listens, thinks, speaks; still assumes turn-taking `Ch 9`
- **On-Policy vs Off-Policy** — On-policy uses only current policy data; off-policy uses data from other/older policies `Ch 7`
- **Optimistic Locking** — Concurrency control: check version on write; fail and re-read if modified by another Agent `Ch 10`

## P

- **Pass@k** — Probability that at least one of k attempts succeeds; measures capability ceiling `Ch 6`
- **Pass^k** — Probability that all k attempts succeed; measures stability/reliability `Ch 6`
- **Peer Collaboration** — 2-3 Agents as equals forming iterative improvement loop; Proposer-Reviewer is the classic form `Ch 10`
- **Permission-Embedded Data Objects** — Each data entity carries declarative permission rules enforced on every write `Ch 5`
- **Poka-yoke** — Error-prevention design philosophy from Toyota Production System; make mistakes impossible `Ch 1`
- **Position Bias** — Judging model systematically favors candidate in a certain position `Ch 6`
- **Post-training** — Encoding experience into model parameters through SFT and RL; permanent but expensive `Ch 1, 7`
- **PPO** `Proximal Policy Optimization` — RL algorithm using clipping to limit update magnitude; uses value network `Ch 7`
- **Principal Loyalty** — Agent must be absolutely loyal to the principal, prudent toward external interacting parties `Ch 5`
- **Process vs Outcome Rewards** — Process = step-by-step feedback `dense, expensive`; Outcome = single end signal `sparse, easy` `Ch 7`
- **Progressive Disclosure** — Skills metadata shown first; full content loaded only when Agent judges it is needed `Ch 2`
- **Prompt Cache** — Cross-request cache built on KV Cache; providers charge ~1/10 price for cached prefixes `Ch 2`
- **Prompt Injection** — Attacker manipulates model behavior indirectly through external data `Ch 2, 4`
- **Proposer-Reviewer** — One Agent generates, another independently reviews with external feedback; value = new information `Ch 5, 10`

## R

- **ReAct Loop** — Reason then Act then Observe, repeat until done; core execution loop of autonomous Agents `Ch 1`
- **Reflexion** — After failure, reflect in natural language, store in episodic memory, avoid repeating mistake `Ch 8`
- **Reward Hacking** — Agent finds shortcut to high scores without actually completing the task `Ch 6, 7`
- **RLHF** `Reinforcement Learning from Human Preferences` — From human preferences to reward models to RL training `Ch 7`
- **RLVR** `Reinforcement Learning from Verifiable Rewards` — Rule-based verification instead of learned reward model `Ch 7`
- **Rubric** — Expert-defined scoring criteria for LLM-as-Judge; four principles: expert-based, comprehensive, weighted, self-contained `Ch 6`

## S

- **Sessionless Design** — Agent always online; file system state inherently persistent across messages `Ch 5`
- **SFT** `Supervised Fine-Tuning` — Optimizes "how much it looks like the standard answer"; memorizes fixed mappings; ceiling = demonstrator quality `Ch 7`
- **Sidecar Mechanism** — Independent review outside the main Agent context; speculative execution makes checks invisible `Ch 4`
- **Sim2Real** — Training in simulation, deploying in reality; uses domain randomization and environment alignment `Ch 9`
- **Sleep Consolidation** — Offline memory reorganization analogous to human sleep; strips redundancy, weaves into knowledge network `Ch 8`
- **Sparse Embedding** — Keyword-based exact match retrieval `BM25` `Ch 3`
- **Speculative Execution** — Display progress hint + run security check in parallel; security without sacrificing UX `Ch 5`
- **Strategy Summary** — Condensing successful problem-solving into structured experience notes; criterion = transferability `Ch 8`
- **Sub-Agent Context Isolation** — Delegate bulky intermediate work to sub-agents; only summaries return to main context `Ch 2`

## T

- **Three Learning Paradigms** — Post-training `permanent, months`, In-Context `session-only, instant`, Externalized `persistent, hours` `Ch 1, 8`
- **Three-Level Memory Evaluation** — Basic Recall, Multi-Session Retrieval, Proactive Service `Ch 3`
- **Tool Calling / Function Calling** — Model decides whether to call, which tool, and what arguments `Ch 1`
- **Trajectory** — Complete historical record of a single Agent run; append-only, never modified `Ch 1, 3`

## V

- **VAD** `Voice Activity Detection` — Determines when user finished speaking; 500-800ms silence threshold `Ch 9`
- **Verification-Generation Asymmetry** — Verifying correctness is easier than generating correct answers; powers RLVR `Ch 7`
- **VLA** `Vision-Language-Action model` — Outputs robot actions based on camera images and language instructions `Ch 9`
- **Voyager** — Open-world Agent architecture systematizing explore-verify-store-reuse cycle in Minecraft `Ch 8`
