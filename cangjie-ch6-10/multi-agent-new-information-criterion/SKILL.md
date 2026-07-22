---
name: multi-agent-new-information-criterion
description: "Trigger when deciding whether to use multi-agent collaboration vs. a single Agent. The core criterion: does the collaboration introduce NEW information (execution results, rendered screenshots, tool verification) that a single Agent could not obtain? If no (debate, self-review), single Agent suff..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 10
tags: [multi-agent, collaboration, verification, architecture]
related_skills: [externalized-learning-three-products, proposer-reviewer-new-information]
---

# Multi-Agent New Information Criterion

## R - Reading
> "Does the collaboration introduce new information that a single Agent could not obtain during generation? Self-review by the same model (re-reading its own output): No -> usually ineffective. Different Agents debating the same text: No -> comparable to a single Agent with equal compute. Reviewer uses test execution results: Yes -> significant improvement. Reviewer uses rendered screenshots: Yes -> significant improvement. The value of review lies not in 'asking the model to think again,' but in introducing new information that was not available during the model's generation."
> - Li Bojie, Chapter 10

## I - Interpretation
This criterion resolves the apparent contradiction between academic research ("single Agent is sufficient") and engineering practice ("multi-agent systems work better"). The resolution: academic studies compare modes where "multiple Agents look at the same text" (no new information = no benefit), while effective multi-agent systems include external feedback loops (execution results, visual rendering, tool calls = new information = significant benefit). The data processing inequality in information theory explains why: multiple Agents processing the same textual information can only lose information through serial transmission, not create it. The practical implication: don't build multi-agent debate systems; build multi-agent systems where the Reviewer can access information the Proposer couldn't.

## A1 - Past Application
### Example 1: RLEF Code Execution Feedback
- **Problem**: Does multi-agent code review beat single Agent sampling?
- **Method application**: Trained a model via RL to use code execution feedback for iterative improvement vs. independent sampling multiple times
- **Conclusion**: Execution feedback (compilation errors, test failures, runtime exceptions) introduces NEW information that didn't exist when the code was written. Multi-agent with execution feedback significantly outperformed multi-sampling.
- **Result": Training with execution feedback is the key to iterative code improvement

### Example 2: WebGen-Agent Visual Feedback
- **Problem**: Can multi-level feedback improve webpage generation?
- **Method application**: Multi-level visual feedback scaffolding (screenshots + visual language model descriptions) used by a Reviewer Agent
- **Conclusion**: Screenshots contain visual information the code-generating Agent could not obtain when writing code. This new information improved Claude 3.5 Sonnet from 26.4% to 51.9% - nearly doubling performance.
- **Result": Visual feedback is new information that justifies multi-agent architecture

## A2 - Future Trigger
### Scenarios
1. Your team wants to build a "debate" system where multiple Agents discuss the same problem
2. You're deciding between a single Agent with more compute vs. a multi-agent system
3. Your multi-agent system isn't performing better than a single Agent
### Language signals
- "Do we really need multiple Agents?"
- "Would multiple Agents debating give us better answers?"
- "Our multi-agent system isn't outperforming a single Agent"

## E - Execution Steps
1. **Identify what information each Agent can access** - Does the Reviewer have access to execution results, rendered output, or external tool verification that the Proposer didn't? Criteria: if YES, multi-agent is justified; if NO, use single Agent with more compute.
2. **Evaluate against single-Agent baseline** - Compare multi-agent system against a single Agent given the same total compute budget. Criteria: multi-agent must significantly exceed the single-Agent baseline to justify the complexity and cost (often 5-15x tokens).
3. **Design for new information injection** - If proceeding with multi-agent: ensure the Reviewer accesses external feedback (tests, screenshots, tool outputs). Criteria: every multi-agent link should introduce information not available to the upstream Agent.
4. **Avoid pure debate/self-review** - Multiple Agents examining the same text provide no new information. Criteria: if the only difference between Agents is their prompt/persona, the benefit is marginal.

## B - Boundary
- Multi-agent systems cost 5-15x more tokens than single-Agent - the gains must justify the overhead
- A weak planner/Manager is the most critical bottleneck of the entire system (Plan-and-Act paper: 54% success driven by planner quality)
- Multi-agent systems introduce new failure modes: concurrency conflicts, cascading error amplification, premature termination
- For tasks where verification IS as hard as generation (creative tasks), the criterion doesn't clearly favor multi-agent
- Data processing inequality: serial transmission of intermediate conclusions between Agents can only lose information

## Related Skills
- depends-on: externalized-learning-three-products
- complements: proposer-reviewer-new-information
