---
name: process-vs-outcome-reward
description: "Trigger when designing reward functions for multi-turn Agent RL training. Process rewards give feedback at each step (faster convergence, may limit exploration); outcome rewards give feedback only at the end (maximum exploration freedom, harder to train). Do NOT trigger for single-turn tasks or n..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 7
tags: [post-training, reward-design, multi-turn, credit-assignment]
related_skills: [sft-memorizes-rl-generalizes, data-environment-over-algorithms]
---

# Process vs. Outcome Reward Design

## R - Reading
> "Process Reward provides immediate feedback for each key step during execution, transforming evaluation from a black box to a white box... Outcome Reward provides feedback only at the end, offering maximum exploration freedom but requiring higher training difficulty and sample demands. When the correctness of intermediate steps is easy to define, process rewards are more efficient; when the optimal path is unknown, outcome rewards have more potential."
> - Li Bojie, Chapter 7

## I - Interpretation
The choice between process and outcome rewards is the central design decision for multi-turn Agent RL. Process rewards are like a teacher grading homework problem by problem - quick feedback but constrains the solution space. Outcome rewards are like only looking at the final exam score - maximum freedom to explore but very slow feedback. The choice depends on task structure: navigation tasks with clear "correct action at each step" benefit from process rewards; tasks where the optimal path is unknown (like the "pushcut" discovery in robotics) benefit from outcome rewards. In practice, the best approach often combines both: "reward the outcome, constrain the process" (RLVP).

## A1 - Past Application
### Example 1: V-IRL-VL Spatial Navigation
- **Problem**: Training a visual navigation Agent across cities with very different architectures
- **Method application**: Process reward: correct action +1, incorrect action -1, landmark recognition error additional -1.5. Dense feedback at every step.
- **Conclusion**: Process rewards reduced credit assignment difficulty. Combined with verification retry mechanism (verify_iter=2), improved sample efficiency and training stability. RL improved visual OOD from 16.7% to 77.8% (+61.1pp).
- **Result**: Dense feedback enables the Agent to learn from navigation errors immediately rather than waiting until task completion

### Example 2: SimpleVLA-RL Outcome Reward Discovery
- **Problem**: Can a robot VLA discover novel manipulation strategies?
- **Method application**: Pure binary outcome reward (success/failure). No process rewards. Dynamic sampling filters all-success/all-fail groups.
- **Conclusion**: The model discovered the "pushcut" action - eliminating the lift step for a more efficient grasp-and-push path. Process rewards would have constrained the model to the demonstration space and prevented this discovery.
- **Result**: Outcome rewards provide greater exploration freedom but require more training to converge

## A2 - Future Trigger
### Scenarios
1. You're designing a reward function for a multi-step customer service Agent and deciding whether to score each turn or only the final outcome
2. Your RL training converges slowly with outcome-only rewards and you want to add intermediate signals
3. Your process rewards constrain the Agent to suboptimal strategies and you want more exploration
### Language signals
- "Should we reward intermediate steps or just the final outcome?"
- "Our RL training is too slow with only final rewards"
- "The Agent keeps following suboptimal paths"

## E - Execution Steps
1. **Analyze task structure** - Are intermediate steps clearly definable as correct/incorrect? Criteria: if yes, process rewards accelerate convergence; if no, outcome rewards provide more exploration.
2. **Start with outcome rewards** - Begin with the simplest viable reward (binary success/failure). Criteria: model can learn at all, even if slowly.
3. **Add process rewards if convergence is too slow** - For multi-turn tasks with delayed rewards, add per-turn feedback. Criteria: convergence speed improves without degrading final performance.
4. **Consider RLVP for constraints** - When outcome-neutral constraints exist (don't run destructive commands), add verifiable path signals. Formula: R = O + beta * Phi where Phi penalizes violations and rewards compliance.
5. **Monitor reward hacking** - Process rewards can be gamed by optimizing for intermediate metrics. Criteria: final outcome quality must not degrade when process rewards improve.

## B - Boundary
- Process rewards introduce human design bias - they encode assumptions about what the "correct" intermediate steps are
- Outcome rewards face the "sparse reward" problem - in long tasks, the Agent may never stumble into success
- Combining both requires careful normalization to prevent one signal from overwhelming the other
- Credit assignment (which step contributed to the outcome) remains hard even with process rewards in long-horizon tasks

## Related Skills
- depends-on: sft-memorizes-rl-generalizes
- complements: verification-generation-asymmetry
