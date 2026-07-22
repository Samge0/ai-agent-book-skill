---
name: statistical-significance-eval
description: "Trigger when comparing two Agent configurations (models, prompts, tools) by success rate and deciding whether to switch. Prevents decisions based on sampling noise. Key signal: \"the new model scored 3% higher -- should I switch?\" Do NOT trigger for single-model benchmarking or non-comparative eva..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 6
tags: [evaluation, statistics, decision-making, noise]
related_skills: [pass-at-k-vs-pass-k-k, model-swap-vs-ablation]
---

# Statistical Significance: Don't Switch on Noise

## R -- Reading (Original Quote)
> When the score difference is smaller than the noise bandwidth, do not make a switching decision... A 3-point difference like "new model 73% vs. old model 70%" sits entirely inside the noise band -- treating the two success rates as independent, the standard error of their difference is about 6.5%. Switching models on such evidence is little better than a coin flip.
> -- Li Bojie, Chapter 6

## I -- Interpretation (Methodology Skeleton)
Agent evaluation is stochastic: same model, same dataset, different runs produce different results due to temperature sampling, fluctuating tool returns, and environmental timing. Before making a switching decision, you must estimate the noise bandwidth. The standard error of a binomial success rate p measured on n cases is approximately sqrt(p*(1-p)/n). For 100 cases at 70% success, the standard error is ~4.6%, and the 95% confidence interval is +/- 9 percentage points. When the difference between two configurations is smaller than the noise bandwidth, the observed difference may be pure sampling noise. The correct approach: run multiple times (3-5 runs per config), use paired analysis (compare win/loss on the SAME tasks), and only switch when the difference exceeds the noise threshold.

## A1 -- Past Application (Book Examples)
### Example 1: The 3% Trap
- **Problem**: New model scores 73% vs old model 70% on 100 test cases. Should we switch?
- **Method application**: Standard error = sqrt(0.7*0.3/100) = 4.6%. The 95% CI is 70% +/- 9%. The 3% gap falls entirely within noise.
- **Conclusion**: The difference is not statistically significant. Switching is a coin flip.
- **Result**: Avoided a costly migration based on noise.

### Example 2: Paired Analysis
- **Problem**: Two models show similar average scores, but are they really equivalent?
- **Method application**: Instead of comparing average success rates, compare per-task win/loss. Model A wins 35 tasks, Model B wins 25, 40 ties.
- **Conclusion**: Paired analysis reveals A is consistently better on specific task types, even though averages are close.
- **Result**: Migrate only the task types where A wins, keep B for the rest -- a differentiated strategy.

### Example 3: Multi-Run Averaging
- **Problem**: A single evaluation run gives model X 82%, but is that reliable?
- **Method application**: Run 5 times with different random seeds: scores are 82%, 74%, 88%, 79%, 85%. Mean = 81.6%, spread = 14 points.
- **Conclusion**: The 14-point spread reveals high variance. A single run's 82% could have been 74% or 88%.
- **Result**: Always report mean AND spread, never a single run.

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. Comparing two models/prompts/tools and deciding whether to switch based on success rates
2. Interpreting benchmark results where scores are close (within 5%)
3. Designing an evaluation methodology with proper sample sizes
4. A stakeholder claims "model X is better" based on a single run

### Language signals (user says things like)
- "The new model scored X% higher -- should I switch?"
- "Is a 3% improvement significant?"
- "How many test cases do I need for a reliable benchmark?"
- "Our A/B test showed model A is slightly better"

## E -- Execution Steps
1. **Calculate noise bandwidth** -- Completion criteria: standard error = sqrt(p*(1-p)/n), report 95% CI = p +/- 2*SE
2. **Compare gap to noise** -- Completion criteria: if gap < 2*SE, the difference is NOT significant; do not switch
3. **Run multiple times** -- Completion criteria: 3-5 runs per configuration, report mean and spread
4. **Apply paired analysis** -- Completion criteria: compare per-task win/loss on the SAME task set, not independent averages
5. **Decide** -- Completion criteria: switch only if the difference exceeds noise AND is consistent across runs

## B -- Boundary (When NOT to use)
- Evaluating a single model in isolation (no comparison)
- Tasks with deterministic outputs (no stochasticity, so single runs suffice)
- When sample size is very large (>10000) and differences are clearly significant
- Over-analysis: if the difference is 30%, you don't need statistics to know it's real

## Related Skills
- depends-on: model-swap-vs-ablation (significance testing tells you IF a difference is real; ablation tells you WHY)
- complements: pass-at-k-vs-pass-k-k (Pass^k measures reliability, significance testing measures confidence)
