---
name: sft-memorizes-rl-generalizes
description: "Trigger when deciding between SFT and RL for Agent post-training, or when diagnosing why a fine-tuned model fails on unseen situations. SFT memorizes fixed input-output mappings; RL learns transferable strategies. Do NOT trigger for choosing between pre-training approaches or general ML model sel..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 7
tags: [post-training, sft, reinforcement-learning, generalization]
related_skills: [data-environment-over-algorithms, process-vs-outcome-reward]
---

# SFT Memorizes, RL Generalizes

## R - Reading
> "SFT optimizes for 'how much it looks like the standard answer.' The goal is to maximize the probability of the labeled answer. Given a question, there is only one 'correct' output. The model is pulled toward this single path... RL optimizes for 'how good the result is.' Given a question, any output that achieves a high reward is good. The model explores multiple paths on its own, reinforcing the ones that yield good results... SFT is mass-covering; RL is mode-seeking via reverse KL divergence."
> - Li Bojie, Chapter 7

## I - Interpretation
The distinction is not about which technique is "better" but about what each one fundamentally learns. SFT bakes a fixed mapping into parameters - it teaches format, style, and protocol. RL discovers strategies - it learns what process leads to good outcomes. This is why SFT models collapse when the environment changes (they apply the memorized answer), while RL models adapt (they re-solve using the learned strategy). The practical pipeline is "form first, spirit second": SFT establishes format stability, then RL shapes strategy. Skipping SFT leads to RL training failure because the reward signal cannot be computed from unparseable output.

## A1 - Past Application
### Example 1: GeneralPoints Card Game Experiment
- **Problem**: Does SFT or RL generalize better to unseen card values?
- **Method application**: Trained with J/Q/K=10 during training, tested with J/Q/K=11/12/13 (rule OOD) and different suit colors (visual OOD)
- **Conclusion**: On rule OOD, RL improved +3.5% while SFT *decreased* 8.1%. On visual OOD, RL improved +17.6% while SFT *decreased* 9.9%. SFT overfit to token patterns, neglecting visual token learning.
- **Result**: Clear experimental validation that SFT memorizes training distribution specifics while RL learns transferable strategies

### Example 2: SimpleVLA-RL "Pushcut" Discovery
- **Problem**: Can RL discover superior strategies absent from human demonstrations?
- **Method application**: Trained robot VLA with RL using only binary outcome rewards on LIBERO benchmark
- **Conclusion**: The model autonomously discovered a "pushcut" action pattern (approach-grasp-keep low-horizontal push) never seen in human demonstrations (which used approach-grasp-vertical lift-horizontal move)
- **Result**: 97.6% success rate; RL with only 1 trajectory per task for SFT cold start achieved 91.7% (+74.4pp, ~430% relative improvement)

## A2 - Future Trigger
### Scenarios
1. Your fine-tuned Agent works well on training-like inputs but fails when the deployment environment changes
2. You're deciding whether to invest in collecting demonstrations (SFT) or building a reward function (RL)
3. Your SFT model performance plateaus and adding more demonstrations no longer helps on new scenarios
### Language signals
- "Our fine-tuned model works in testing but fails in production"
- "Should we use supervised fine-tuning or reinforcement learning?"
- "Adding more training examples isn't improving generalization"

## E - Execution Steps
1. **Diagnose the problem type** - Fixed format/protocol: SFT. Generalization to unseen situations: RL. Criteria: does the deployment distribution drift from training?
2. **Start with SFT** - Use 1,000-10,000 high-quality demonstrations to stabilize output format. Criteria: model produces parseable output (>80% format success rate).
3. **Evaluate SFT generalization** - Test on out-of-distribution examples. Criteria: if SFT performance drops significantly on OOD while maintaining in-distribution, SFT has memorized rather than generalized.
4. **Add RL when SFT is insufficient** - Design reward function from evaluation environment. Apply RL on top of SFT-initialized model. Criteria: RL should improve OOD performance without degrading in-distribution.
5. **Balance SFT duration** - Stop SFT when "format is stable and basic capabilities are present." Over-training SFT causes collapse onto training distribution, limiting RL's optimization space.

## B - Boundary
- SFT is not suitable for injecting large amounts of factual knowledge - use continued pre-training or RAG instead
- RL requires a reliable reward signal - without verifiable rewards or a good reward model, RL cannot learn
- RL is 10-100x more expensive than SFT - if the distribution is stable, SFT may suffice
- A sufficiently strong base model may skip SFT (DeepSeek-R1-Zero demonstrated direct RL), but output readability suffers

## Related Skills
- depends-on: data-environment-over-algorithms
- complements: process-vs-outcome-reward
