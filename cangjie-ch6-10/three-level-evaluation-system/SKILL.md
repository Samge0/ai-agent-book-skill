---
name: three-level-evaluation-system
description: "Trigger when building or improving an Agent evaluation pipeline. Covers the Environment (where to test), Methods (how to judge), and Decisions (what to do after testing) levels. Do NOT trigger for single-model benchmarking or non-Agent evaluation. Key signal: \"how do I know my Agent got better?\""
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 6
tags: [evaluation, benchmarking, metrics, model-selection]
related_skills: [model-swap-vs-ablation, pass-at-k-vs-pass-k-k, llm-as-judge-rubric]
---

# Three-Level Evaluation System

## R - Reading
> "This chapter builds a complete evaluation system on three levels. The first level is the Evaluation Environment ('where to test'). The second level is Evaluation Methods ('how to judge'). The third level is Evaluation-Driven Decision Making ('what to do after testing'). An evaluation system's primary value is not scoring the current system, but letting you keep up with model evolution quickly and reliably."
> - Li Bojie, Chapter 6

## I - Interpretation
Evaluation is not a one-time exam but a continuous infrastructure. The three levels form a closed loop: the environment provides automated testing, methods define scoring criteria, and decisions turn results into actionable improvements. The deepest insight is that evaluation's primary value is speed - a team with a robust evaluation system can reach a model-switching decision within hours, while a team without one can only trust intuition. Evaluation is the "verification" function within the Harness engineering framework.

## A1 - Past Application
### Example 1: AndroidWorld Benchmark-to-Improvement Loop
- **Problem**: Agent scored 88% on AndroidWorld but failures clustered in transcription, settings, and complex UI tasks
- **Method application**: Used three-level system - evaluation environment (AndroidWorld automated testing), methods (per-task table + capability tag matrix), decisions (hypothesized H1-H6 improvements, tested with cost-benefit analysis)
- **Conclusion**: Deployed H1 (settings hints) + H3 (fix multimodal pipeline) + H6 (element tree), rising from 88% to 94%. Rejected H4 (global thinking) as too costly for 8% of tasks.
- **Result**: Data-driven improvement prioritization instead of blind adoption

## A2 - Future Trigger
### Scenarios
1. Your team needs to decide whether to switch from Claude to a new Gemini model on your specific tasks
2. You observe performance regression after a prompt change and need to diagnose the root cause
3. A new model ships with better benchmark scores and you need to determine if it helps your production Agent
### Language signals
- "How do we know this change actually helped?"
- "What's our evaluation strategy for model upgrades?"
- "We need a repeatable way to measure Agent performance"

## E - Execution Steps
1. **Build the evaluation environment** - Set up automated, reproducible testing (tool-calling or HCI env). Criteria: can reset to clean initial state, runs without manual intervention.
2. **Design the evaluation dataset** - Balance clarity vs. openness, authenticity vs. controllability. Criteria: covers typical + edge cases, prevents data contamination.
3. **Choose metrics and scoring methods** - Select Pass@k vs Pass^k based on purpose. Apply LLM-as-a-Judge with structured Rubric. Criteria: metrics distinguish capability from stability.
4. **Establish statistical significance** - Run multiple times, use paired analysis. Criteria: observed differences exceed noise bandwidth.
5. **Close the improvement loop** - Read benchmark reports, hypothesize improvements, test, deploy based on cost-benefit. Criteria: every change runs regression tests on the evaluation set.

## B - Boundary
- Evaluation cannot fully replace human judgment for subjective quality dimensions
- Static evaluation sets degrade over time as models evolve - requires continuous refresh from production traces
- Building a high-fidelity evaluation environment may cost more than the Agent itself
- Evaluation data must be strictly isolated from training data

## Related Skills
- depends-on: model-swap-vs-ablation
- complements: llm-as-judge-rubric
- contrasts-with: pass-at-k-vs-pass-k-k
