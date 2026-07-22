---
name: model-swap-vs-ablation
description: "Trigger when diagnosing Agent performance issues - specifically to determine whether the bottleneck is the model or the Harness. Use model swap (fix Harness, change model) vs. ablation (disable Harness components). Do NOT trigger for general ML model comparison or hyperparameter tuning. Key signa..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 6
tags: [evaluation, diagnosis, ablation, model-selection, harness]
related_skills: [three-level-evaluation-system, sft-memorizes-rl-generalizes]
---

# Model Swap vs. Ablation: Diagnosing the Bottleneck

## R - Reading
> "A common way to tell them apart is the model swap experiment: fix the Harness, swap in a stronger or weaker model, and watch how much the score moves. If a stronger model doesn't raise the score, the bottleneck is the Harness... Ablation disables a Harness component to see how overall performance changes; model swapping fixes the Harness and changes only the model. The former locates which part inside the Harness matters; the latter tells you whether the bottleneck is the model or the Harness."
> - Li Bojie, Chapter 6

## I - Interpretation
These two experiments answer fundamentally different questions. The model swap answers "WHERE is the bottleneck?" - model or Harness. Ablation answers "WHICH component matters?" - inside the Harness. They are complementary diagnostic tools. The key insight: when a stronger model doesn't improve performance, the problem isn't the model's capability but the Harness design (prompts, tool design, feedback loops). This prevents wasted effort on model upgrades when the real fix is Harness engineering.

## A1 - Past Application
### Example 1: AndroidWorld Complex UI Failure
- **Problem**: Agent failed on complex_ui_understanding tasks at 17% success rate
- **Method application**: Model swap test (Claude to GPT-5) improved complex_ui to only 35% despite 15s/step latency. This told the team the bottleneck was NOT model capability but information richness.
- **Conclusion**: Adding element tree info (H6) improved to 52% at 30% token cost, far better than model swap alone. The model swap experiment correctly redirected effort from model upgrade to information enrichment.
- **Result**: 35-percentage-point improvement for 30% more tokens vs. model swap's 18-point improvement at 4x latency

### Example 2: OpenClaw Feature Contribution Analysis
- **Problem**: Which features (thinking mode, context compression, memory, background tasks) actually contribute to user experience?
- **Method application**: Built-in ablation master switch disables features one at a time, creating "bare model" baseline
- **Conclusion**: Discovered "feature debt" - features once effective but no longer necessary as models evolved
- **Result**: Regular ablation before each major release ensures resources aren't wasted on unnecessary features

## A2 - Future Trigger
### Scenarios
1. Your Agent performs poorly and you need to decide: invest in a better model or improve the Harness?
2. You added a new Harness component (retrieval, memory, planning) and want to verify it actually helps
3. A new model launches with better benchmarks but you suspect your Harness is the real bottleneck
### Language signals
- "Would a bigger model fix this?"
- "Is the problem in our prompts/tools or the model itself?"
- "We added RAG but performance didn't improve - why?"

## E - Execution Steps
1. **Run model swap experiment** - Fix all Harness components, swap to a model 2+ capability tiers apart. Criteria: if score doesn't move, bottleneck is Harness.
2. **Run ablation experiments** - Disable one Harness component at a time, measure performance delta. Criteria: identify which components contribute most.
3. **Cross-analyze results** - If model swap shows large delta: model is bottleneck. If ablation shows large delta: specific Harness component matters. Criteria: improvement resources should target the identified bottleneck.
4. **Validate with paired analysis** - Run 3-5 times with different seeds, use paired comparison. Criteria: observed difference exceeds noise bandwidth.

## B - Boundary
- Model swap requires models from different capability tiers - swapping between similar models may not reveal the bottleneck
- Ablation only works if features are independently disableable - must be designed into the architecture from the start
- Both experiments assume the evaluation system itself is reliable - "when Agent performance drops, check the evaluation system first, then the Agent"
- Interactions between components may mean ablation underestimates the value of a component that enables others

## Related Skills
- depends-on: three-level-evaluation-system
- complements: mechanism-vs-target-metrics
- contrasts-with: sft-memorizes-rl-generalizes
