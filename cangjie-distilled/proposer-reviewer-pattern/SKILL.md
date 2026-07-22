---
name: proposer-reviewer-pattern
description: "Trigger when Agent prematurely declares tasks complete, when output quality needs verification before delivery, or when multi-step Agent tasks require self-checking. Key signals: \"Agent says done but is not done\", \"premature completion\", \"self-review\", \"verification loop\", \"proposer-reviewer\". Do..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 1 (introduced), Chapter 4 (tool design), Chapter 5 (reinforced)
tags: [verification, quality, reliability, multi-step]
related_skills: []
---

# Proposer-Reviewer Pattern

## R -- Reading (Original Quote)
> The core of the method is to let the Agent review its own output artifacts and iterate, so that verification -- not the model own feeling -- decides when a task ends.
> -- Li Bojie, Introduction (referring to loop engineering / proposer-reviewer method)

## I -- Interpretation (Methodology Skeleton)
The Proposer-Reviewer Pattern prevents the two most common failure modes of long Agent tasks: declaring completion prematurely, and giving up after one blocked path. The mechanism: one Agent (or one role) proposes a solution or artifact; another Agent (or the same Agent in a reviewer role) evaluates it against objective criteria before the action proceeds. The reviewer checks: Does the output meet the requirements? Are there errors? Is the task actually complete? Only when verification passes does the task end. This can be implemented as: (a) same-model self-review (Agent reviews its own output), (b) separate reviewer Agent with different context, (c) deterministic checks (linters, tests, validators). The key principle: the model own feeling of completion is not sufficient -- external verification must confirm it.

## A1 -- Past Application (Book Examples)
### Example 1: Pine AI Negotiation Agent
- **Problem**: Agent negotiating bill reductions over the phone might declare "done" before the negotiation is actually closed, or give up after one failed attempt.
- **Method application**: The proposer-reviewer method has the Agent review its own output artifacts -- did the refund actually post? Was the negotiation truly concluded? The Agent iterates until verification confirms success.
- **Conclusion**: Verification -- not the model own feeling -- decides when a task ends.
- **Result**: Reliable task completion for sensitive financial negotiations.

### Example 2: PPT/Video Generation (Chapter 5)
- **Problem**: Code-generated multimedia content (PPTs, videos) may have quality issues that the generation Agent cannot detect in a single pass.
- **Method application**: A Proposer Agent generates the content; a Reviewer Agent evaluates it (information density, visual quality, correctness). If the Reviewer finds issues, the Proposer iterates. This creates a feedback loop that converges on acceptable quality.
- **Conclusion**: The proposer-reviewer mechanism is repeated across PPT generation, video editing, and log visualization throughout the book.
- **Result**: Quality convergence through iterative review.

### Example 3: Execution Tool Verification (Chapter 4)
- **Problem**: Execution tools (write_file, send_email) need verification before actions are committed.
- **Method application**: Pre-approval (constrain before execution) and post-validation (verify after execution). The Sidecar mechanism uses an independent reviewer outside the main Agent context to approve high-risk operations.
- **Conclusion**: Critical operations must be reviewed by mechanisms outside the Agent own context -- an Agent within the same context cannot reliably determine if it has been compromised.
- **Result**: Layered defense for execution tools.

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. Agent declares tasks "done" but outputs are incomplete or incorrect
2. Building a long-running Agent where task completion must be verified automatically
3. Multi-step Agent tasks where errors compound across steps
4. Designing quality control for Agent-generated content

### Language signals (user says things like)
- "My Agent says it is done but the work is not actually complete"
- "How do I verify Agent outputs automatically?"
- "The Agent gives up too easily when it hits an obstacle"
- "I need a self-checking mechanism for my Agent"

## E -- Execution Steps
1. **Define verification criteria** -- What does "done" mean for this task? Specify objective, checkable criteria. Completion criteria: Verification rules documented and testable.
2. **Implement the reviewer role** -- Create a separate Agent call or deterministic check that evaluates proposed output against criteria. Completion criteria: Reviewer can approve or reject with specific feedback.
3. **Design the iteration loop** -- When reviewer rejects, feed back specific issues to the proposer for correction. Set a maximum iteration count. Completion criteria: Loop terminates on success or max iterations.
4. **For high-risk operations, use external review** -- The Sidecar mechanism: an independent reviewer outside the main Agent context approves critical actions. Completion criteria: Critical actions require independent authorization.
5. **Test with premature-completion scenarios** -- Give the Agent tasks where the obvious first attempt is wrong. Completion criteria: Reviewer catches errors and triggers iteration in 80%+ of cases.

## B -- Boundary (When NOT to use)
- **Simple deterministic tasks**: If the output can be verified by a simple rule (e.g., JSON validity check), a full proposer-reviewer loop is overkill.
- **When external verification exists**: If tests, CI/CD, or human review already covers verification, adding Agent self-review adds latency without benefit.
- **Author blindspot**: The Reviewer aesthetic preferences may differ from the user. The book acknowledges this for content generation (PPT, video) -- the Reviewer might consider information density reasonable while the user finds it too crowded.

## Related Skills
- depends-on: harness-engineering
- composes-with: coding-agent-meta-capability
