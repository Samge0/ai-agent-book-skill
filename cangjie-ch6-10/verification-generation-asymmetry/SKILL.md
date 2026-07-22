---
name: verification-generation-asymmetry
description: "Trigger when deciding whether to use RL (needs only a verifier) vs. SFT (needs a generator/demonstrator) for a task. Exploits the asymmetry that verifying correctness is easier than generating correct answers. Do NOT trigger for tasks where verification is as hard as generation. Key signal: \"We c..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 7
tags: [post-training, rl, verification, reward-design]
related_skills: [sft-memorizes-rl-generalizes, data-environment-over-algorithms]
---

# Verification-Generation Asymmetry

## R - Reading
> "Verifying is easier than generating - this is the fundamental reason RL can climb higher. SFT requires someone to write out the correct answer first as a demonstration; RL only needs a way to judge whether an answer is good (assign a reward). In many tasks, judging right from wrong is far easier than producing the right answer from scratch. This is why RL can discover strategies that no demonstrator ever showed it."
> - Li Bojie, Chapter 7

## I - Interpretation
This asymmetry is why RL has a higher ceiling than SFT. For math problems, code, and logical reasoning, checking whether an answer is correct (run the test, verify the proof) is dramatically easier than producing the correct answer. This means you can train on tasks where you could never collect demonstrations - no human expert needs to solve the problem first. The RLVR (Reinforcement Learning with Verifiable Rewards) paradigm is built on this insight. But the asymmetry cuts both ways: for open-ended tasks (creative writing, customer service quality), verification is as hard as generation, and you need reward models instead of rule-based verifiers.

## A1 - Past Application
### Example 1: ReTool Code-Integrated Reasoning
- **Problem**: Training a model to solve competition math (AIME) - generating correct solutions is hard, but checking if the final answer is correct is easy
- **Method application**: RL with code execution feedback - model generates code + text reasoning, sandbox executes code, binary reward (correct/incorrect answer)
- **Conclusion**: Accuracy improved from ~25% to 67% on AIME 2024. The model learned to self-correct code errors and shift tool use from late verification to early exploration.
- **Result**: No human demonstrations needed - only a verifier (answer matching)

### Example 2: RLVP Path Penalty
- **Problem**: Outcome rewards cannot enforce outcome-neutral constraints (don't edit test files, don't run destructive commands) - violations often *raise* apparent success rate
- **Method application**: "Reward the outcome, penalize the path." Detecting bad actions (destructive command, modifying test file) is cheap and deterministic. Detecting meaningful progress is hard and error-prone.
- **Conclusion**: The dense signal environments can reliably provide is "penalties on the path," not "rewards for progress." This asymmetry determines the shape of the method.
- **Result**: Within-group variance breaks the zero-gradient deadlock in all-fail and all-pass groups

## A2 - Future Trigger
### Scenarios
1. You have a task where answers can be verified automatically (unit tests, math checking, SQL validation) but good demonstrations are expensive
2. You're deciding whether RLVR or RLHF is appropriate for your task
3. You want to train an Agent for code generation but can't afford expert-labeled solutions
### Language signals
- "We can check if the answer is right but can't write good examples"
- "Testing is easier than solving"
- "Can we use automatic verification instead of human annotation?"

## E - Execution Steps
1. **Assess verification difficulty** - Can correctness be checked by rules/code? (math, code, SQL -> easy. Creative writing, conversation quality -> hard). Criteria: if verification is automatable, RLVR is viable.
2. **Compare with generation difficulty** - Is producing correct answers significantly harder than checking them? Criteria: large asymmetry favors RL; small asymmetry favors SFT or RLHF.
3. **Design the reward function** - For rule-verifiable tasks: binary reward (pass/fail). For multi-dimensional tasks: vector reward. For open-ended: generative reward model. Criteria: reward must be computable from the output alone.
4. **Start RL training** - With verification in place, RL can explore and discover strategies without demonstrations. Criteria: model discovers solutions not present in any training data.

## B - Boundary
- For tasks where verification is as hard as generation (open-ended creative tasks), this asymmetry doesn't hold - need reward models instead
- Even with easy verification, RL still needs the "form first" SFT stage to produce parseable output
- Binary rewards are sparse - without process rewards or RLVP-style path signals, sample efficiency may be very low
- Verification-based rewards can be gamed (reward hacking) - the model finds shortcuts to pass verification without genuinely solving the task

## Related Skills
- depends-on: sft-memorizes-rl-generalizes
- complements: data-environment-over-algorithms
