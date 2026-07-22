# Chapter 6: Evaluating Agents

## Core Idea
Evaluation is not an afterthought — it is the compass that guides every improvement. Build the evaluation environment, datasets, and metrics system BEFORE building the Agent. The core framework is three-level: Environment, Methods, and Decisions.

## Frameworks Introduced

- **Three-Level Evaluation System**:
  1. **Environment**: Automated evaluation environment `tool-calling, human-computer interaction`
  2. **Methods**: Evaluation task datasets + metrics + automated evaluation methods
  3. **Decisions**: From benchmark reports to system improvements
  - When to use: Designing any evaluation program
  - How: Environment first, then datasets, then metrics, then methods, then decisions

- **LLM-as-Judge**: Use a language model to evaluate output quality based on expert-defined Rubric
  - When to use: Open-ended tasks without standard answers
  - How: Rubric defines dimensions, weights, scoring levels, and veto items

- **Four Rubric Principles** `Scale AI`:
  1. Based on Expert Guidance — reflect domain knowledge
  2. Comprehensive Coverage — factual, logical, completeness, safety + explicit Pitfalls
  3. Standard Importance Weights — Essential/Important/Optional/Pitfall + Veto mechanism
  4. Self-Contained Evaluation — independently actionable, no reliance on evaluator domain knowledge
  - When to use: Designing any Rubric
  - How: Define objectively verifiable scoring levels; actively guard against Reward Hacking

- **Model Swap Experiment**: Swap models to isolate model vs system contributions
  - When to use: Diagnosing whether a problem is model or Harness
  - How: Keep Harness constant, swap model; keep model constant, swap Harness component

- **Ablation Infrastructure**: Systematically remove/restore features to measure true contribution
  - When to use: Understanding what actually drives performance
  - How: Feature on/off matrix; compare with/without each component

- **Pass@k vs Pass^k**:
  - **Pass@k**: Probability that AT LEAST ONE of k attempts succeeds — "Can the Agent do it?"
  - **Pass^k**: Probability that ALL k attempts succeed — "Is the Agent reliable?"
  - When to use: Choosing evaluation metrics
  - How: Pass@k for capability ceiling; Pass^k for stability/regression testing

## Key Concepts
- **Process Metrics**: Action legality rate, tool call correctness, path efficiency, retrieval coverage, cost/latency
- **Outcome Metrics**: Task success rate, hierarchical standards `core goals + secondary quality`
- **Goodhart Law**: When a metric becomes an optimization target, it ceases to be a good metric
- **Position Bias**: Judging model systematically favors the candidate in a certain position
- **Multi-Source Heterogeneous Judging**: Independent judges from different model families
- **Elo Rating**: Quantify relative ability through pairwise matchups; Bradley-Terry model
- **Agent Observability**: Logging, tracing, metrics for production-grade monitoring
- **A/B Testing**: Distinguish mechanism `what changed` from goal `what improved`
- **Dual Coverage**: Evaluate both trajectory `what Agent said/did` AND outcome `system state`

## Mental Models
- **Think of evaluation as the steering wheel**: Without it, you are driving blind — improvements become guesses
- **Think of Rubric as a casebook**: It starts as abstract principles and evolves through trial use into detailed cases
- **Think of ablation as a controlled experiment**: Remove one variable at a time to measure its true contribution
- **Use Pass@k to measure ceiling, Pass^k to measure floor**: Confuse them and you misread your Agent

## Anti-patterns
- **Same-family judging**: Agent exploits judge model preferences and blind spots — use multi-source heterogeneous judges
- **Length bias in judging**: Judge scores longer responses higher even when no more correct — penalize verbosity in Rubric
- **Evaluating only outcome or only trajectory**: "Said it but did not do it" vs "achieved it through a wrong path"
- **No judge calibration**: LLM judge scores are just another model opinion without human-annotated gold standard `kappa > 0.7`

## Code Examples
```yaml
# Rubric example: User Memory Agent
# Test: "Who is my daughter pediatrician?"
# Requires linking: daughter=Lily + Lily doctor=Dr. Chen

rubric:
  dimensions:
    - name: Factual Correctness
      weight: essential
      scoring:
        4_Excellent: "Correctly answers Dr. Chen, links to daughter Lily"
        1_Fail: "Incorrect doctor name or I do not know"
    - name: Hallucination Detection
      weight: veto  # Once triggered, total score is zero
      scoring:
        pass: "All info traceable to conversation records"
        fail: "Fabricated info not in conversation"
```
- **What it demonstrates**: Each scoring level specifies verifiable, concrete behavior. The veto item sets the bottom line: even if every other dimension scores full marks, one hallucination = automatic zero.

## Reference Tables

### Pass@k vs Pass^k `at 60% single-attempt success`
| Metric | k=1 | k=5 | Measures |
|--------|-----|-----|----------|
| **Pass@k** | 60% | 99% | Can the Agent do it? `ceiling` |
| **Pass^k** | 60% | 7.8% | Is the Agent reliable? `stability` |

### Evaluation Metrics Categories
| Category | Metric | What It Measures |
|----------|--------|-----------------|
| **Process** | Action legality rate | Valid/legal operations proportion |
| **Process** | Tool call correctness | Parameters semantically reasonable |
| **Process** | Path efficiency | Steps, redundant actions, backtracking |
| **Outcome** | Task success rate | Core goals achieved |
| **Safety** | Zero-tolerance violations | Data leakage, prohibited content |

## Worked Example
**Reading a benchmark report**: A team sees their Agent at 72% on a coding benchmark. They check: Which model was used? What was the step budget? Which tasks failed and are there patterns? What is the cost per task? They form hypotheses: "Model X fails on multi-file refactoring" and build an improvement roadmap targeting that specific weakness. **Lesson**: Data without hypotheses is noise; hypotheses without data are guesses.

## Key Takeaways
1. **Build evaluation infrastructure before building the Agent**: Environment, datasets, metrics — in that order.
2. **LLM-as-Judge needs calibrated Rubrics**: Four principles; veto items for hallucination; guard against reward hacking.
3. **Use multi-source heterogeneous judging**: Different model families have orthogonal biases.
4. **Pass@k measures ceiling, Pass^k measures stability**: Choose based on your question.
5. **Ablation reveals true feature contribution**: What you think helps may hurt; what you think is irrelevant may be critical.
6. **Cover both trajectory and outcome**: "Said it but did not do it" is a trajectory-outcome gap.
7. **Cost is a first-class metric**: Anthropic multi-agent research system uses 15x tokens of normal conversation.

## Connects To
- **Ch 1**: Harness Engineering — evaluation is the Verify function at system level
- **Ch 3**: Three-level memory evaluation framework
- **Ch 7**: Evaluation drives post-training; reward signals from evaluation environments
- **Ch 9**: Simulation environments for robotics `Sim2Real`
- **Ch 10**: A/B testing for multi-agent systems; model swap experiments
