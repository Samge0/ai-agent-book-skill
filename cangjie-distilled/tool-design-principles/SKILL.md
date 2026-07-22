---
name: tool-design-principles
description: "Trigger when designing Agent tools, when the Agent picks the wrong tool or passes wrong parameters, or when tool descriptions need improvement. Key signals: \"tool design\", \"Agent picks wrong tool\", \"tool description\", \"ACI\", \"tool granularity\", \"parameter passing\". Do NOT trigger for model select..."
source_book: "Deep Understanding of AI Agents — Li Bojie"
source_chapter: Chapter 4
tags: [tool-design, aci, usability, interface]
related_skills: []
---

# Tool Design Principles (Agent-Computer Interface)

## R -- Reading (Original Quote)
> A practical debugging principle: when an Agent keeps picking the wrong tool, check the tool descriptions first rather than doubting the model. Most tool selection errors trace back to inaccurate descriptions -- unclear boundaries, missing negative examples, ambiguous parameter meanings.
> -- Li Bojie, Chapter 4

## I -- Interpretation (Methodology Skeleton)
Tool design quality directly determines what an Agent can reliably accomplish. The core framework is ACI (Agent-Computer Interface) -- designing tools from the Agent perspective, not the programmer perspective. Four key principles: (1) Granularity: integrate functionally similar tools (one read_document with file_type parameter beats extract_pdf_text + extract_docx_content + extract_pptx_content), but keep separate when parameter sets differ greatly. (2) Generality: prefer general-purpose tools (code_interpreter) over specialized ones (calculator) unless security/permission/performance demands otherwise. (3) Description art: describe when to use (not just what it does), explicitly list boundaries (what it cannot do), use concrete parameter examples, and include 1-5 real invocation examples. (4) Fidelity: no silent parameter transformation -- the world the model perceives must match the world the tool operates on.

## A1 -- Past Application (Book Examples)
### Example 1: Document Processing Tool Integration
- **Problem**: Three separate tools for extracting text from PDF, DOCX, and PPTX files created cognitive load for the model.
- **Method application**: Integrated into a single read_document tool with a file_type parameter. The model only needs to understand one rule: use read_document to read documents.
- **Conclusion**: Integration reduces cognitive load, makes descriptions clearer, and facilitates extensibility.
- **Result**: Tool selection accuracy improved significantly.

### Example 2: Silent Quote Conversion Bug (Cursor)
- **Problem**: An edit tool silently converted Chinese curly quotes to English straight quotes, causing the model to fail repeatedly with "no match found" despite seeing curly quotes in file reads.
- **Method application**: The tool parameter passing layer was modifying inputs without the model knowledge, creating a systematic discrepancy between what the model perceived and what the tool operated on.
- **Conclusion**: Tool parameter passing must remain transparent. If normalization is necessary, it must be documented and communicated to the model.
- **Result**: This anti-pattern (silent input transformation) was identified as a fundamental tool design violation.

## A2 -- Future Trigger (When to Activate)
### Scenarios where users need this skill
1. Agent repeatedly picks the wrong tool or passes wrong parameters
2. Designing the tool set for a new Agent system
3. Tool descriptions are vague or missing boundary conditions
4. Agent fails on tasks that should be easy given the available tools

### Language signals (user says things like)
- "My Agent keeps picking the wrong tool"
- "The Agent passes wrong parameters to my tools"
- "How should I describe my tools to the Agent?"
- "Should I combine these tools or keep them separate?"

## E -- Execution Steps
1. **Audit existing tools for ACI compliance** -- For each tool: Is the name intuitive? Are boundaries documented? Are there negative examples? Completion criteria: Audit report listing gaps for each tool.
2. **Optimize granularity** -- Identify functionally similar tools that can be integrated with a type/format parameter. Keep separate when parameter sets differ significantly. Completion criteria: No unnecessary proliferation; no overly coarse mega-tools.
3. **Rewrite descriptions with when-to-use and boundaries** -- Each description must say when to use, what it cannot do, and include concrete parameter examples. Completion criteria: Every description has positive use cases, negative boundary examples, and real invocation examples.
4. **Verify parameter fidelity** -- Ensure no silent transformations between model perception and tool execution. Completion criteria: No discrepancy between what the model sees and what the tool does.
5. **Test with wrong-tool scenarios** -- Present tasks that could be confused between tools. Completion criteria: Agent selects the correct tool in 90%+ of test cases.

## B -- Boundary (When NOT to use)
- **Tools that already work well**: If the Agent uses tools correctly and reliably, optimization is not needed.
- **Security-critical tools**: Some tools need rigid, specialized interfaces for permission control and audit trails. Generality is not always better.
- **Author blindspot**: The book focuses on text-based tool descriptions. As Freeform Tool Calling (raw text parameters) becomes more common, some description principles may need adaptation.

## Related Skills
- depends-on: harness-engineering
- composes-with: proactive-tool-discovery
