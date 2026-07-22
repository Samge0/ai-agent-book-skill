---
title: "Skill Index - Chapters 6-10"
book: "Deep Understanding of AI Agents"
chapters: 6-10
---

# Skill Index: Chapters 6-10

## Skill Catalog

| # | Skill Slug | Chapter | Core Framework | Trigger Signal |
|---|-----------|---------|---------------|----------------|
| 1 |  | Ch6 | Environment -> Methods -> Decisions | "How do I know my Agent got better?" |
| 2 |  | Ch6 | Model swap (where bottleneck) vs Ablation (which component) | "Is this a model problem or a system problem?" |
| 3 |  | Ch6 | Pass@k (capability) vs Pass^k (reliability) | "Is our Agent reliable enough for production?" |
| 4 |  | Ch7 | SFT (mass-covering, offline) vs RL (mode-seeking, online) | "Should I use SFT or RL for this task?" |
| 5 |  | Ch7 | Prior > Environment > Algorithm priority order | "Which RL algorithm should I use?" |
| 6 |  | Ch7 | Verifying is easier than generating -> RLVR | "We can check answers but can't write examples" |
| 7 |  | Ch7 | Process (dense, constrained) vs Outcome (sparse, free) | "Should we reward intermediate steps?" |
| 8 |  | Ch8 | Knowledge Base / Code Tool / Skill Document | "How should the Agent remember this?" |
| 9 |  | Ch9 | Fast (real-time) + Slow (reasoning) via text/latent bridge | "We need fast response AND deep thinking" |
| 10 |  | Ch10 | Multi-agent justified only if new information introduced | "Do we really need multiple Agents?" |

## Reference Graph



## Cross-Skill Dependencies

### Evaluation Cluster (Ch6)
-  is the root skill
-  depends on it (diagnostic within the evaluation loop)
-  is a metric choice within the methods level
-  (referenced, Ch6) provides scoring methodology

### Post-Training Cluster (Ch7)
-  is the foundational principle
-  depends on understanding SFT/RL tradeoffs
-  explains WHY RL can exceed SFT
-  is the reward design layer on top of SFT/RL

### Evolution Cluster (Ch8)
-  bridges Ch7 (parametric) and Ch8 (non-parametric)
- Depends on understanding when NOT to change weights (from )

### Interaction Cluster (Ch9)
-  applies the fast/slow paradigm to real-time architectures
- Connects to Ch10's multi-agent division of labor

### Collaboration Cluster (Ch10)
-  is the gating decision for multi-agent systems
- Builds on Ch9's fast-slow separation (multiple models with different capabilities)
- Connects to Ch5's Proposer-Reviewer paradigm (referenced in Ch10)

## Chapter Coverage Map

### Chapter 6 (Evaluation) - 3 skills
All three evaluation skills form a complete pipeline: design the system (skill 1), diagnose issues (skill 2), measure reliability (skill 3).

### Chapter 7 (Post-Training) - 4 skills
The SFT/RL pipeline from principle (skill 4) to priorities (skill 5) to mechanism (skill 6) to reward design (skill 7).

### Chapter 8 (Self-Evolution) - 1 skill
Externalized learning as the complement to parametric learning. Voyager and tool creation are sub-topics within this skill.

### Chapter 9 (Multimodal/Real-Time) - 1 skill
Fast-slow separation is the cross-cutting architectural principle. Voice paradigms, Computer Use, and robotics all apply this principle to different modalities.

### Chapter 10 (Multi-Agent) - 1 skill
The new information criterion is the master decision rule. Topology selection (Peer/Manager/Decentralized) follows only after this criterion is satisfied.

## Verification Status (Triple Verification)

| Skill | V1: Cross-domain (2+ sections) | V2: Predictive power | V3: Uniqueness |
|-------|-------------------------------|---------------------|----------------|
| three-level-evaluation-system | YES (env + methods + decisions across 6 chapters) | Predicts model switch decisions | Not common sense |
| model-swap-vs-ablation | YES (Ch6 + Ch10 planner bottleneck) | Predicts where to invest improvement effort | Counterintuitive: bigger model != better |
| pass-at-k-vs-pass-k-k | YES (tau-bench + general metrics) | Predicts production reliability | Mathematical insight easily confused |
| sft-memorizes-rl-generalizes | YES (GeneralPoints + V-IRL + SimpleVLA) | Predicts OOD performance | Counterintuitive: SFT can hurt OOD |
| data-environment-over-algorithms | YES (Anthropic recipe + AWorld + OpenAI history) | Predicts RL training success | Highly counterintuitive |
| verification-generation-asymmetry | YES (RLVR + RLVP + ReTool) | Predicts when RL is viable | Deep structural insight |
| process-vs-outcome-reward | YES (V-IRL + SimpleVLA + RLVP) | Predicts RL convergence speed | Trade-off not obvious |
| externalized-learning-three-products | YES (GAIA + Voyager + browser-use) | Predicts knowledge carrier choice | Classification not obvious |
| fast-slow-thinking-separation | YES (voice + Computer Use + robotics + gaming) | Predicts latency-quality tradeoffs | Architectural pattern across modalities |
| multi-agent-new-information-criterion | YES (RLEF + WebGen + CRITIC + debate) | Predicts multi-agent ROI | Resolves academic vs. practice tension |
