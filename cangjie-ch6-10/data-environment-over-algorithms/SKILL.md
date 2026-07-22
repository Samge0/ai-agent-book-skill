---
name: data-environment-over-algorithms
description: "Trigger when a team is starting RL post-training and is fixated on algorithm selection (PPO vs GRPO vs DPO). Redirects effort to simulation fidelity and data quality. Do NOT trigger for pure algorithm research or when data/env are already excellent. Key signal: \"Which RL algorithm should I use?\""
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 7
tags: [post-training, rl, data-quality, simulation, priorities]
related_skills: [sft-memorizes-rl-generalizes, process-vs-outcome-reward]
---

# Data and Environment Matter More Than Algorithms

## R - Reading
> "Algorithms matter far less than three more basic elements - the fidelity of the simulation environment, the quality of the training data, and the capability of the base model. Knowing how to use existing algorithms is enough; what separates teams is how well they do the environment and the data... The real order is not algorithm > environment > prior, but prior > environment > algorithm."
> - Li Bojie, Chapter 7

## I - Interpretation
This is the chapter's most counterintuitive and most valuable lesson. Teams routinely obsess over algorithm choice (PPO vs GRPO vs DPO) when the real differentiators are mundane: is the simulation realistic? Is the data clean, diverse, and well-covered? Anthropic trained excellent coding models before 2025 using mostly SFT + RLAIF with little RLVR - not because of algorithmic superiority but because of data quality. The priority order is: choose a strong base model, then polish environment and data, and only then squeeze marginal gains from algorithms. Chasing algorithms before environment and data are ready is "the classic cart before the horse."

## A1 - Past Application
### Example 1: Anthropic's Pre-2025 Recipe
- **Problem**: How did Anthropic build excellent coding models with minimal RLVR?
- **Method application**: Heavy SFT on massive high-quality data + RLAIF (Constitutional AI), leaning little on RLVR
- **Conclusion**: When SFT data quality is high enough, an unfancy recipe can train a top-tier model. Elaborate verifiable-reward RL is not a requirement.
- **Result**: Data decides how far you can go; RL decides how much higher.

### Example 2: AWorld MCP Sandbox
- **Problem**: Real APIs have rate limits, ban accounts, and carry side effects - cannot train against them directly
- **Method application**: Built a controllable MCP server sandbox with 26 servers and 126 tool functions, all replayable and auditable
- **Conclusion**: Building a high-fidelity environment was harder and more expensive than the training itself - but without it, training is impossible
- **Result**: Environment engineering IS the bottleneck, not algorithm tuning

## A2 - Future Trigger
### Scenarios
1. A team debates whether to use PPO or GRPO before having any training data or environment
2. RL training results are poor and the team blames the algorithm
3. A startup wants to do Agent RL but hasn't invested in simulation environment
### Language signals
- "Which RL algorithm should we use?"
- "Let's try a different algorithm - PPO isn't working"
- "We need to tune the hyperparameters"

## E - Execution Steps
1. **Assess the base model** - Is it strong enough to produce parseable output? Criteria: model can follow instructions and produce structured output.
2. **Evaluate simulation fidelity** - Does the training environment match the deployment scenario? Criteria: error messages match production, state transitions follow business logic, environment can be reliably reset.
3. **Audit data quality** - Check coverage (does it cover deployment scenarios including edge cases?), diversity (styles, speakers, solutions), annotation accuracy. Criteria: all three dimensions meet threshold.
4. **Only then select algorithm** - With env and data in place: reliable reward + compute -> GRPO (simple) or PPO (flexible). Preference data -> DPO/KTO. Early exploration -> Best-of-N. Criteria: algorithm is the LAST decision, not the first.

## B - Boundary
- Algorithm choice does matter when everything else is excellent - marginal gains at the frontier
- This principle applies to RL; for SFT, the algorithm is standard (next-token prediction with loss masking)
- Building high-fidelity simulation may cost more than the training itself - budget accordingly
- "In many scenarios, if the SFT data quality is there, you may not need RL at all"

## Related Skills
- depends-on: sft-memorizes-rl-generalizes
- complements: process-vs-outcome-reward
