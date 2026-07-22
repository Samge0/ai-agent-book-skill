---
name: fast-slow-thinking-separation
description: "Trigger when designing real-time Agent systems that need both instant response and deep reasoning. Separates a fast model (real-time interaction) from a slow model (background reasoning) communicating through a text or latent bridge. Do NOT trigger for non-real-time tasks or single-model architec..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 9
tags: [real-time, voice, architecture, latency, fast-slow]
related_skills: [three-voice-paradigms, multi-agent-new-information-criterion]
---

# Fast-Slow Thinking Separation for Real-Time Agents

## R - Reading
> "Solution 2 allows slow thinking to see the output of fast thinking. It provides suggestions to fast thinking via the Agent Status Bar, rather than speaking directly to the user. Slow thinking runs asynchronously in the background, using the gaps in speech to keep thinking; because it can see fast thinking's output, it no longer clashes with it head-on, retreating instead to the role of behind-the-scenes 'strategist.'... The Latent Bridge: project the slow model's hidden states into 'latent tokens' spliced into the fast model's input, bypassing the round trip of 'idea -> text -> re-understanding.' On Atari games, +26% to +82%, at ~5ms/step."
> - Li Bojie, Chapter 9

## I - Interpretation
The fundamental contradiction in real-time Agents is that users expect millisecond responses while complex problems require seconds of thinking. The solution is architectural separation: a fast model handles real-time interaction (filler words, acknowledgments, simple answers) while a slow model reasons in the background and feeds conclusions to the fast model. The communication channel between them matters enormously: text is simple but lossy; latent space bridges preserve rich intermediate state but require training. The judgment of when to use this separation: "whether the task's bottleneck is 'can't think of it' or 'can't react in time.'" The bridge pays off only where the slow thinker is genuinely better than the fast reactor.

## A1 - Past Application
### Example 1: GPT-Live Delegation Architecture
- **Problem**: ChatGPT Voice needs to maintain real-time conversation while accessing search tools and complex reasoning
- **Method application**: Interactive GPT-Live (fast) maintains conversation while delegating to frontier GPT-5.5 (slow) in background. Results stream back incrementally; GPT-Live weaves them in at a natural moment.
- **Conclusion**: Delivers "planning, tool, and agent capabilities of a reasoning model" with "latency of a non-thinking model." Turn-switching latency ~0.40s vs. GPT-realtime-2.0's ~1.18s.
- **Result": "Sustainable swapping to the latest frontier models" - the fast interaction model stays fixed while the slow reasoning model can be upgraded independently

### Example 2: Computer Use Voice-Operation Decoupling
- **Problem**: A Computer Use Agent takes 3-5 seconds per action, making voice conversation impossible if one model does both
- **Method application**: Split into fast voice Agent (real-time conversation) and slow Computer Use Agent (step-by-step browser operation). They communicate through a "plain text contract" - rolling status summary.
- **Conclusion**: Voice responses became ~15x faster (0.58s vs 8.64s median) with no loss in task success rate. Remove the text channel and success collapses to zero.
- **Result**: The fast Agent must never say "done" until the status summary confirms completion

## A2 - Future Trigger
### Scenarios
1. You're building a voice Agent that needs to answer simple questions instantly but research complex ones in the background
2. Your single-model Agent is too slow for real-time conversation but switching to a smaller model loses reasoning quality
3. You want to build a "talking while operating" Agent that maintains voice conversation while using a computer
### Language signals
- "We need fast response AND deep thinking"
- "The model is too slow for real-time but too dumb when fast"
- "Can the Agent talk while it works?"

## E - Execution Steps
1. **Identify the latency budget** - What response time does the user expect? (voice: <1s, chat: <3s, batch: no limit). Criteria: latency budget determines whether separation is needed.
2. **Choose the split** - Fast model: lightweight, real-time interaction. Slow model: frontier reasoning, background operation. Criteria: fast model can handle simple cases independently.
3. **Design the communication channel** - Text (simple, lossy, debuggable) or Latent Bridge (rich, requires training, ~5ms overhead). Criteria: channel bandwidth matches the information the slow model needs to convey.
4. **Implement the status contract** - Fast model reads status from slow model via Agent Status Bar. Criteria: fast model never claims "done" without slow model's confirmation.
5. **Handle edge cases** - What if slow model disagrees with fast? What if user interrupts during slow reasoning? Criteria: disagreement resolution and interruption handling are designed explicitly.

## B - Boundary
- This separation adds system complexity - for non-real-time tasks, a single model is simpler and often better
- The Latent Bridge requires training a custom bridge model - only viable for teams with ML training capability
- Fast and slow models may give inconsistent answers (Solution 1's problem) without careful coordination
- "The bridge pays off only where the slow thinker is genuinely better than the fast reactor" - for purely reactive tasks, separation adds latency without benefit

## Related Skills
- depends-on: three-voice-paradigms
- complements: multi-agent-new-information-criterion
