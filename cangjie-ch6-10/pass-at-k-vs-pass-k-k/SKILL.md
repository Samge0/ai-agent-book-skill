---
name: pass-at-k-vs-pass-k-k
description: "Trigger when selecting evaluation metrics for Agent reliability vs. capability assessment. Pass@k measures capability ceiling (\"can it do it?\"), Pass^k measures stability (\"is it reliable?\"). Do NOT trigger for general accuracy metrics or classification tasks. Key signal: \"Is our Agent reliable e..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 6
tags: [evaluation, metrics, reliability, statistical-methods]
related_skills: [three-level-evaluation-system, model-swap-vs-ablation]
---

# Pass@k vs Pass^k: Capability Ceiling vs. Reliability

## R - Reading
> "Pass@k: The probability that at least one of k attempts succeeds, answering 'Can the Agent do it?' Pass^k: The probability that all k attempts succeed, answering 'Is the Agent stable and reliable?' Suppose the Agent's single-attempt success rate is 60%. Over 5 attempts: Pass@5 = 99% (almost certain to succeed at least once), while Pass^5 = 7.8% (all five succeeding is unlikely). The former measures the capability ceiling, the latter stability; confuse them and you will misread your Agent."
> - Li Bojie, Chapter 6

## I - Interpretation
These two metrics answer diametrically different questions about the same Agent. Pass@k is optimistic - it asks "can it ever succeed?" Pass^k is pessimistic - it asks "will it always succeed?" The practical implication is profound: an Agent with 60% single-attempt success rate looks great on Pass@5 (99%) but terrible on Pass^5 (7.8%). For production deployment where reliability matters (banking, healthcare), you must use Pass^k. For exploratory tasks where you can retry, Pass@k or Best@k is appropriate. Confusing them leads to deploying unreliable Agents or rejecting capable ones.

## A1 - Past Application
### Example 1: tau-bench Binary Reward Design
- **Problem**: tau-bench collapses all component-level checks into a binary reward (all must pass for 1, any failure = 0)
- **Method application**: This binary reward makes Pass^k easy to compute - each attempt either fully succeeds or fails
- **Conclusion**: Binary rewards at task level naturally support reliability measurement via Pass^k, at the cost of granularity
- **Result**: tau-bench can distinguish "operationally accurate but missing one field" from "complete failure" only through detailed sub-checks, while the task-level metric enforces strict reliability

## A2 - Future Trigger
### Scenarios
1. You're deploying an Agent for critical operations (financial transactions, medical appointments) and need to verify reliability
2. Your Agent succeeds sometimes but fails other times on the same task - how do you quantify this?
3. You need to choose between two Agents: one with 80% single-shot success, another with 60% but more consistent behavior
### Language signals
- "Is our Agent reliable enough for production?"
- "Sometimes it works, sometimes it doesn't - how do we measure this?"
- "We need the Agent to never make a mistake"

## E - Execution Steps
1. **Determine the evaluation purpose** - Regression testing/production deployment: use Pass^k. Exploratory/capability assessment: use Pass@k or Best@k. Criteria: purpose determines metric.
2. **Calculate baseline metrics** - Run k attempts (k >= 3) on the same task set. Compute Pass@k = 1-(1-p)^k and Pass^k = p^k where p is single-attempt rate. Criteria: both values reported.
3. **Interpret the gap** - Large gap between Pass@k and Pass^k indicates instability. Criteria: if Pass^k < 50% while Pass@k > 90%, the Agent is capable but unreliable.
4. **Choose deployment strategy** - For high Pass@k / low Pass^k: deploy with retry mechanisms or human-in-the-loop. For high Pass^k: deploy autonomously. Criteria: deployment strategy matches measured reliability.

## B - Boundary
- Pass^k becomes impractical for large k with moderate success rates (60%^5 = 7.8%)
- Binary task definitions (pass/fail) are required - continuous scoring needs Best@k instead
- Running k attempts is k times more expensive - balance statistical power with cost
- These metrics assume task difficulty is uniform - heterogeneous task sets need per-category analysis

## Related Skills
- depends-on: three-level-evaluation-system
- complements: statistical-significance-evaluation
