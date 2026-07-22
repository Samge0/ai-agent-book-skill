---
title: "Glossary - Chapters 6-10"
book: "Deep Understanding of AI Agents"
chapters: 6-10
---

# Glossary: Key Terms from Chapters 6-10

## Chapter 6: Evaluating Agents

**Ablation Experiment** - A diagnostic method that disables one component of the Harness at a time to measure its contribution. Unlike model swap, ablation keeps the model fixed and varies the Harness.

**Agent Status Bar** - A dynamic meta-information injection mechanism (from Chapter 2) where code deterministically maintains key conclusions in the context. Used in fast-slow thinking architectures to pass information from slow to fast models.

**Best@k** - The score of the best of k attempts, measuring the quality ceiling given enough opportunities. Used for open-ended tasks with continuous scoring.

**Bradley-Terry Model** - A statistical model that abstracts each model as a latent "strength score" and determines the probability of one beating another from their score difference. The mathematical foundation of Elo ratings.

**Cohen's Kappa** - A statistical measure of inter-annotator agreement that discounts chance agreement. Used to calibrate LLM judges against human gold standards (threshold: kappa > 0.7).

**Domain Randomization** - A technique for narrowing the sim-to-real gap by introducing wide random variations in physical parameters, visual appearance, and sensor noise during simulation training.

**Dual-Control Environment** (tau2-bench) - An evaluation design where both the Agent and the user simulator can operate on the same shared environment, better matching real technical support scenarios.

**Elo Rating** - A ranking system originally for chess that quantifies relative ability through pairwise matchups. Used in Chatbot Arena for model ranking.

**Goodhart's Law** - "When a metric becomes an optimization target, it ceases to be a good metric." Explains reward hacking and reward model over-optimization.

**LLM-as-a-Judge** - Using a large language model to evaluate the output quality of open-ended tasks based on expert-defined scoring criteria (Rubric).

**McNemar's Test** - A statistical test for paired nominal data, used in evaluation to judge whether the difference between two configurations on the same task set is significant.

**Model Swap Experiment** - Fixing the Harness and swapping in a stronger/weaker model to determine whether the bottleneck is the model or the Harness. If a stronger model doesn't raise the score, the bottleneck is the Harness.

**Multi-armed Bandit (A/B Testing)** - An A/B testing design with multiple progressive variants rather than simple binary comparison, revealing dose-response relationships.

**Pass@k** - The probability that at least one of k attempts succeeds, answering "Can the Agent do it?" Measures capability ceiling.

**Pass^k** - The probability that all k attempts succeed, answering "Is the Agent stable and reliable?" Measures stability/reliability.

**Progressive Information Disclosure** - The principle that simulated users in HCI evaluation should not reveal all information at once, but gradually disclose it based on the Agent's questions. The fundamental difference between HCI evaluation and traditional benchmarks.

**Reward Hacking** - The Agent finding a shortcut to high scores without actually completing the task. Goodhart's Law in action.

**Rubric** - Structured scoring criteria for LLM-as-a-Judge evaluation. Four principles: (1) Based on Expert Guidance, (2) Comprehensive Coverage, (3) Standard Importance Weights (Essential/Important/Optional/Pitfall), (4) Self-Contained Evaluation.

**User Simulation** - Using another LLM to play the user role in HCI evaluation, conversing with the Agent according to predefined instructions with progressive information disclosure.

**Veto Mechanism** - A Rubric design where certain dimensions (e.g., hallucination) automatically zero the total score regardless of other dimensions' performance.

## Chapter 7: Model Post-Training

**Advantage** - In RL, how much better an action is compared to the average. The core signal for policy gradient updates.

**Behavioral Cloning** - Learning "see something, do something" directly from human demonstrations. Essentially SFT for robotics.

**Catastrophic Forgetting** - Forgetting old skills after learning new ones during fine-tuning. Mitigated by LoRA (freezing base weights).

**Credit Assignment** - In multi-turn tasks, determining which step in a multi-step sequence contributed most to the final outcome.

**DPO (Direct Preference Optimization)** - An offline preference optimization method that turns preference pairs into a classification loss with an implicit reward, skipping explicit reward model training.

**GRPO (Group Relative Policy Optimization)** - An RL algorithm that eliminates the value network and uses intra-group relative comparison to estimate advantages. Samples N trajectories for the same problem; advantage = relative performance within the group.

**KL Divergence** - A measure of the difference between two probability distributions. Used as a penalty in RLHF to prevent the policy from drifting too far from the reference model. Reverse KL (current policy first) is mode-seeking.

**LoRA (Low-Rank Adaptation)** - A parameter-efficient fine-tuning method that attaches a small low-rank matrix "patch" to learn the task, keeping original weights frozen. Parameter count is 1-5% of the original.

**Mass-Covering vs. Mode-Seeking** - SFT's maximum likelihood is mass-covering (tries to cover all modes in demonstrations). RL with reverse KL is mode-seeking (concentrates on few high-reward modes, discards the rest).

**On-Policy Distillation** - A training method combining on-policy (student generates own trajectories) with dense signal (teacher provides per-token distributions). Matches pure RL in ~1/10 the steps.

**On-Policy vs. Off-Policy** - On-policy methods only use data newly sampled by the current policy. Off-policy methods can use data from other policies. SFT is off-policy; standard PPO/GRPO are on-policy; DPO is offline.

**PPO (Proximal Policy Optimization)** - An RL algorithm that clips update magnitude per step to prevent the policy from straying too far. Uses a value network for fine-grained advantage estimation.

**Process Reward Model (PRM)** - Scores each intermediate step of reasoning or execution. Provides dense feedback but introduces human design bias.

**Outcome Reward Model (ORM)** - Only evaluates the final result. Rule-based verifiers in RLVR are a special case.

**RLHF (Reinforcement Learning from Human Feedback)** - RL with rewards from a learned reward model trained on human preference data.

**RLVR (Reinforcement Learning with Verifiable Rewards)** - RL with rewards from a rule-based verifier (test passes, answer matches). Natural fit for Agent tasks.

**RLVP (Reinforcement Learning with Verified Penalty)** - "Reward the outcome, penalize the path." Adds verifiable path signals (penalties for violations, partial credit for compliance) to outcome rewards. Breaks the zero-gradient deadlock in GRPO all-fail/all-pass groups.

**SFT (Supervised Fine-Tuning)** - Training on labeled "input-output" demonstration pairs. Mathematically the same as pre-training (next-token prediction) but with different data and loss masking on the response only.

**Thinking as a Special Action** - In LLM-based RL, internal thinking becomes a core component of the action space - it doesn't change the external environment but improves action quality.

**Token-Level Loss** - DAPO's modification that normalizes uniformly across all tokens in the batch instead of per-response, giving long responses gradient contribution commensurate with their length.

## Chapter 8: Self-Evolution

**Externalized Learning** - Distilling knowledge into files, knowledge bases, and tools outside the model. Persistent, interpretable, and modifiable. Complements post-training and in-context learning.

**Minimal Predefinition, Maximal Self-Evolution** - The Alita principle: provide a minimal foundation of foundational tools, let the Agent expand its capability boundary through self-evolution.

**Sleep Consolidation** - Periodic offline processing where the Agent reviews accumulated experience, compresses and reorganizes knowledge for better retrieval.

**Strategy Summary** - A structured experience note condensing a successful problem-solving process: what methods were used, what pitfalls were encountered, key steps.

**Tool Generation** - The higher-order form of externalized learning: externalizing the process itself into precisely executable code, saved for permanent reuse.

**Voyager** - A Minecraft Agent that achieves lifelong learning by distilling every successful experience into executable code tools stored in a growing skill library.

**Workflow Recording and Replay** - Like Excel macros: record the steps the first time, then replay with parameterized inputs. The Agent records DOM element XPaths and action sequences.

## Chapter 9: Multimodal and Real-Time

**Action Chunking** - A technique where the model generates multiple future actions in one inference pass, executed sequentially by a control thread while the GPU generates the next batch. Bridges the gap between low VLA inference frequency and high control frequency.

**Action Space** (Computer Use) - Three tool types: GUI Operation (mouse/keyboard), Command Execution (bash terminal), File Editing (str_replace_editor).

**Cascaded Pipeline** - Voice architecture stringing VAD-ASR-LLM-TTS sequentially. Each stage waits for the previous. 0.9-2s total latency.

**End-to-End Omnimodal (Omni)** - A single model that listens to audio, thinks of a reply, and speaks it. Lower latency, preserves prosody/emotion, but still assumes turn-taking.

**Full-Duplex / Interactive** - The model listens and speaks simultaneously, processing input and output concurrently. Eliminates the turn-taking assumption entirely (Moshi, GPT-Live).

**Latent Bridge** - A small parameter bridge that projects the slow model's hidden layer conclusions into "latent tokens" spliced into the fast model's input, bypassing text round-trips. +26-82% on Atari at ~5ms/step.

**MGRD (Modality-Grounded Reasoning Distillation)** - Step-Audio R1's method: select thought processes genuinely grounded in acoustic features and train on them, teaching the model to "think with its ears."

**MPS Dual-Brain Architecture** (Mind-Paced Speaking) - Step-Audio R1's architecture: Formulation Brain (continuous thinking) and Articulation Brain (speech output) run in parallel for low-latency "thinking while speaking."

**Set-of-Mark (SoM)** - A visual grounding method that overlays numbered markers on candidate regions of a screenshot, turning localization into a multiple-choice problem.

**Sim2Real Transfer** - Transferring policies trained in simulation to real robots, using domain randomization to bridge the gap.

**Streaming Voice Perception** - Replacing VAD + ASR with a streaming approach: audio is continuously processed while the user speaks, with semantic-level turn judgment rather than silence-duration thresholds.

**Thinker-Talker Architecture** - Qwen3-Omni's design separating thinking (understanding/reasoning) from expression (voice generation) into two specialized modules.

**VAD (Voice Activity Detection)** - Continuously monitors audio to determine when the user has finished speaking, typically using a 500-800ms silence threshold. The starting point and latency bottleneck of cascaded pipelines.

**VLA (Vision-Language-Action)** - A model architecture unifying visual perception, language understanding, and action generation. Used in robotics for language-conditioned control.

**VLM (Vision-Language Model)** - A model that understands both images and text. VLA adds action output on top of VLM.

## Chapter 10: Multi-Agent Collaboration

**A2A (Agent2Agent) Protocol** - Google's standardized interoperability protocol for cross-organizational Agent collaboration. Core elements: Agent Card (capability metadata), Task Lifecycle Management, Opaque Collaboration.

**Agent Virtual File System** - A virtual directory tree mounting four area types: Agent-specific workspace (scratchpad), multi-agent shared workspace, mounted external resources, and built-in system resources.

**Budget-Aware Mechanism** - An Agent mechanism that dynamically adjusts strategies based on remaining step budget: broad exploration early, focused exploitation later. Without it, more steps don't guarantee better results.

**Cascading Termination** - When one Agent succeeds, all other parallel Agents are immediately terminated. Race conditions must be handled with locks or idempotent design.

**Data Processing Inequality** - From information theory: multiple Agents processing the same textual information can only lose information through serial transmission, not create it. Explains why debate mode doesn't beat single Agent with equal compute.

**Feature Flag** - A remotely controllable switch that determines whether a function is enabled. Serves experimentation, gradual rollout, and emergency circuit breaking. First-class architectural component, not debugging tool.

**Handoff Package** - In decentralized handoff without shared context: Task Description + Confirmed Facts/Constraints + References to Structured Artifacts. Deliberately excludes the full trajectory.

**Loop Engineering** - "Design a loop that keeps the Agent running - discover the next piece of work, execute, verify, record progress - and let a verifier, not the model itself, decide whether it is truly safe to stop." The bottleneck of the loop is the verifier, not the model.

**MAST (Multi-Agent System Failure Taxonomy)** - 14 failure modes in three groups: System Design Flaws, Inter-Agent Alignment Failures, Missing Task Verification.

**Message Bus** - A communication infrastructure where Agents publish messages to a bus, which forwards them based on subscriptions. Enables asynchronous, non-blocking communication.

**Optimistic Locking** - A concurrency control strategy: each file maintains a version number. On write, check if the version changed since read. If so, reject and retry. Prevents lost updates without blocking readers.

**Premature Termination** - Three forms: (1) lazy fake-done (partial work declared complete), (2) premature give-up (declaring impossible after one blocked path), (3) false success (loop never actually closed). Root cause: "done" is the model's claim, not a proof.

**Proposer-Reviewer Paradigm** - A peer collaboration pattern where one Agent generates and another reviews using external feedback (rendered output, test results). The Reviewer's value comes from new information, not from "thinking again."

**Shared vs. Non-Shared Context** - Shared: subsequent Agent inherits complete conversation history (zero loss, context grows fast). Non-shared: each Agent has independent context (modular, isolated, requires explicit communication).

**Working Copy Isolation** - Assigning each Agent an independent Git branch or worktree for parallel code modification without interference. Conflicts deferred to merge point.
