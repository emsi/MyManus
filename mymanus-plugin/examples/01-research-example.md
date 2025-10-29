# Example 1: Research Task

## Task Description

**User Request:**
```
Research the current state of quantum computing in 2024-2025.
Focus on: major breakthroughs, leading companies, practical applications,
and remaining challenges. Create a comprehensive report with citations.
```

## Expected Behavior with MyManus Plugin

### Phase 1: Planning (TodoWrite)

Claude will automatically create a structured task list:

```
✓ Creating research todo list
  - Search for recent quantum computing breakthroughs (2024-2025)
  - Search for leading quantum computing companies
  - Research practical applications currently in use
  - Research remaining technical challenges
  - Compile findings into structured outline
  - Write comprehensive report with citations
  - Create reference list with URLs
```

### Phase 2: Execution

#### Step 1: Web Search

Claude uses WebSearch tool to find current information:

```
Searching: "quantum computing breakthroughs 2024 2025"
Searching: "leading quantum computing companies 2024"
Searching: "quantum computing practical applications"
Searching: "quantum computing challenges limitations"
```

**Autonomous behavior:** Claude performs multiple searches systematically without asking for permission each time.

#### Step 2: Source Verification

Claude uses WebFetch or Playwright to access original sources:

```
Accessing: https://example.com/quantum-breakthrough-article
Accessing: https://quantumcompany.com/latest-research
Accessing: https://arxiv.org/quantum-paper-2024
```

**Autonomous behavior:** Claude visits multiple URLs from search results to verify information and get comprehensive data.

#### Step 3: Information Organization

Claude creates draft files for each section:

```
Creating: quantum_breakthroughs_draft.md
Creating: quantum_companies_draft.md
Creating: quantum_applications_draft.md
Creating: quantum_challenges_draft.md
```

**Autonomous behavior:** Intermediate results are saved automatically to prevent loss and enable incremental progress.

#### Step 4: Compilation

Claude merges all drafts into the final report:

```
Creating final report: quantum_computing_report_2024.md
Adding breakthroughs section...
Adding companies section...
Adding applications section...
Adding challenges section...
Compiling references...
```

**Autonomous behavior:** Final document is longer and more detailed than individual drafts combined, not summarized.

### Phase 3: Delivery

#### Final Report Structure

```markdown
# The State of Quantum Computing: 2024-2025

## Executive Summary
[Comprehensive overview in prose format...]

## Recent Breakthroughs
[Detailed analysis with specific examples and dates...]

In 2024, researchers at University X achieved a significant milestone... [1]
Building on this work, Company Y demonstrated... [2]

## Leading Companies and Organizations
[Analysis of major players, their approaches, and achievements...]

IBM's quantum roadmap has focused on... [3]
Google Quantum AI has recently announced... [4]

## Practical Applications
[Current real-world uses and near-term possibilities...]

Financial institutions have begun experimenting with... [5]
Drug discovery applications have shown promise in... [6]

## Remaining Challenges
[Technical and practical barriers to widespread adoption...]

Error correction remains a fundamental challenge... [7]
Scaling quantum systems faces obstacles including... [8]

## Conclusion
[Synthesis and future outlook...]

## References

[1] University X Research Team (2024). "Breakthrough in Quantum Error Correction."
    https://example.com/paper1

[2] Company Y Press Release (2024). "Quantum Advantage Demonstrated."
    https://company-y.com/press-release

[3] IBM Quantum Blog (2024). "Roadmap Update."
    https://ibm.com/quantum/blog/roadmap

[... more references ...]
```

**Length:** 3000+ words with proper citations and references.

### Phase 4: Progress Updates Throughout

Claude keeps you informed via TodoWrite updates:

```
[In Progress] Searching for recent quantum computing breakthroughs
[Completed] Searching for recent quantum computing breakthroughs
[In Progress] Searching for leading quantum computing companies
[Completed] Searching for leading quantum computing companies
...
```

## Key Plugin Features Demonstrated

1. **Autonomous Planning**: Automatic task breakdown without user guidance
2. **Systematic Execution**: Step-by-step information gathering
3. **Source Verification**: Accessing original sources, not just snippets
4. **Progress Transparency**: Real-time TodoWrite updates
5. **Quality Output**: Comprehensive, well-cited research report
6. **Error Handling**: If a source is unavailable, Claude finds alternatives
7. **Persistence**: Intermediate results saved to prevent data loss

## User Experience

### What You See

```
User: Research the current state of quantum computing...

Claude: I'll help you research the current state of quantum computing
in 2024-2025. Let me create a comprehensive research plan and begin
gathering information.

[TodoWrite creates task list]

[Multiple web searches occur]

[Claude accesses and analyzes sources]

[Progress updates appear]

Claude: I've completed the research and compiled a comprehensive report.
The report covers recent breakthroughs, leading companies, practical
applications, and remaining challenges, with 47 citations from
authoritative sources.

The final report is available in quantum_computing_report_2024.md
(~3,200 words).
```

### What You Don't Need To Do

- ❌ Tell Claude to create a plan
- ❌ Ask Claude to search for each topic
- ❌ Tell Claude to verify sources
- ❌ Remind Claude to cite references
- ❌ Request progress updates
- ❌ Ask Claude to organize findings

**The plugin makes Claude autonomous.** You provide the goal, Claude handles the execution.

## Customization Options

### Adjust Report Length

```
Research quantum computing (as above), but keep it to 1500 words.
```

Claude will adapt the writing_rules accordingly.

### Focus on Specific Aspects

```
Research quantum computing, focusing exclusively on practical
applications in drug discovery.
```

Claude will adjust the research plan to be more targeted.

### Change Output Format

```
Research quantum computing and create a slide deck outline
instead of a report.
```

Claude will adapt the output format while maintaining research thoroughness.

## Comparison: With vs Without Plugin

### Without MyManus Plugin

**User:** Research quantum computing...

**Claude:** I can help with that. What specific aspects would you like me to focus on?

**User:** Breakthroughs, companies, applications, challenges.

**Claude:** Here's a summary based on my knowledge:
- [Provides general information from training data]
- [May be outdated]
- [Limited to ~500 words]
- [No citations]

### With MyManus Plugin

**User:** Research quantum computing...

**Claude:** [Immediately creates plan and begins autonomous research]
- Searches current sources (2024-2025)
- Verifies information from multiple sources
- Creates comprehensive 3000+ word report
- Includes 40+ citations with URLs
- Organizes information professionally
- Tracks progress transparently

**This is the power of autonomous agent behavior.**
