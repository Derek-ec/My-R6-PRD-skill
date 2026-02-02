# Write PRD - Interview-Driven PRD Generator for Claude Code

A Claude Code skill that generates comprehensive Product Requirements Documents through a structured interview process, based on best practices from top product managers.

## Philosophy

> **Problem First, Solution Second** - The best PRDs obsess over the problem before prescribing solutions. Engineers should have room to solve problems creatively.

> **Every Word Earns Its Place** - A strong PRD captures only what is necessary. Cut ruthlessly. If a sentence doesn't help someone build or decide, delete it.

> **Quantify Everything** - Vague problems get vague solutions. Replace "users struggle" with "40% of users abandon at checkout."

Instead of staring at a blank page, this skill guides you through targeted questions that the best PMs ask, then generates a well-structured PRD with powerful, concise writing.

## What It Does

1. **Gathers Context** - Reads any existing documents you provide
2. **Runs 5 Rounds of Discovery Questions:**
   - **Problem Validation** - Is this a real problem? How do users solve it today?
   - **User & Context** - Who feels this pain? What triggers their need?
   - **Strategic Fit** - Why now? Is this a painkiller or vitamin?
   - **Success & Trade-offs** - What does winning look like? What are we NOT doing?
   - **Risks & Requirements** - What's the hardest part? What could go wrong?
3. **Generates a Structured PRD** with 10 sections + section-by-section writing guidance
4. **Refines Based on Feedback**

## 7 Principles for Powerful PRDs

Based on research from Lenny Rachitsky, Figma, Stripe, and Amazon:

1. **Make it scannable** - Add TL;DRs, use formatting, cut what you can
2. **Tailor the format** - Not every project needs a 10-section PRD
3. **Problem over solution** - The problem statement shouldn't change; solutions evolve
4. **Write it with your team** - Include eng and design early
5. **Use it for alignment** - Stress test it in reviews
6. **Revisit and update** - Keep the spec fresh
7. **Don't skip the postmortem** - Learn from every launch

## Writing Guidance Highlights

The skill includes detailed guidance for writing each section powerfully:

| Section | Key Rule | Example |
|---------|----------|---------|
| Problem Statement | Quantify with numbers | "40% abandon at checkout" not "users struggle" |
| Success Metrics | Baseline → Target → Timeframe | "Reduce load time from 4s to 2s in 30 days" |
| Target User | Specific roles, not "users" | "Field technician" not "user" |
| Acceptance Criteria | Testable pass/fail | "< 200ms response" not "fast" |
| Non-Goals | Explain WHY deferred | "Mobile: 12% of traffic, revisit post-launch" |

## PRD Structure

The generated PRD includes:

| Section | Purpose |
|---------|---------|
| 1. Problem Statement | The pain, current solutions, why now, evidence |
| 2. Goals & Success Metrics | Primary goal, metrics with targets, kill criteria |
| 3. Target User | Persona, context, job to be done, current workflow |
| 4. Solution Overview | High-level approach and principles |
| 5. Requirements | Prioritized requirements with acceptance criteria |
| 6. User Flows | High-level workflows |
| 7. Non-Goals | What we're explicitly NOT doing |
| 8. Assumptions & Risks | What we believe, what could go wrong |
| 9. Dependencies | What we need from others |
| 10. Open Questions | Unresolved items |

## Key Questions This Skill Asks

Based on research into how top PMs approach PRDs:

### Problem Validation
- How do users solve this problem today?
- How do we know this is a real problem (not an assumed one)?

### User Understanding
- Who feels this pain most acutely?
- What triggers the user to need this? (The "job to be done")

### Strategic Fit
- Why build this now? What's the urgency?
- Is this a "painkiller" (must-have) or "vitamin" (nice-to-have)?

### Success Definition
- What does success look like? (Primary metric)
- What would make us kill this project?

### Scope & Trade-offs
- What are we explicitly NOT doing in V1?
- What's the smallest version that delivers value?

### Risk Assessment
- What's the hardest or riskiest part?
- What assumptions are we making?

## Installation

### Option 1: Clone to Claude Skills Directory

```bash
git clone https://github.com/YOUR_USERNAME/write-prd-skill.git ~/.claude/skills/write-prd
```

### Option 2: Manual Installation

1. Create the skills directory if it doesn't exist:
   ```bash
   mkdir -p ~/.claude/skills/write-prd
   ```

2. Copy `SKILL.md` to that directory:
   ```bash
   cp SKILL.md ~/.claude/skills/write-prd/
   ```

3. Restart Claude Code (or start a new session)

## Usage

### Invoke Directly

```
/write-prd
```

### Natural Language

The skill activates automatically when you say things like:
- "Help me write a PRD"
- "Create a PRD for [feature]"
- "I need a product requirements document"

### With Context Files

For best results, provide existing documents:

```
/write-prd

[Then provide paths to existing docs like:]
- User research findings
- Interview transcripts
- Competitive analysis
- Design briefs
```

## Example Session

```
User: /write-prd

Claude: I'll help you create a PRD. Do you have any existing documents
        I should review first?

User: Yes, read the files in ~/Documents/feature-planning/

Claude: [Reads files, summarizes context]

        Now I'll ask questions to understand the problem deeply.

        [Round 1: Problem Validation]
        How do users solve this problem today?

        1. No solution exists
        2. Manual workarounds
        3. Competitor products
        4. Internal tools
        5. Let me explain

User: 2 - They use spreadsheets and email

Claude: How do we know this is a real problem?

        1. User interviews/research
        2. Support tickets/complaints
        3. Usage data
        4. Lost deals/churn
        5. Assumption - needs validation

User: 1, 2

Claude: [Continues through 5 rounds...]

        [Generates PRD]

        What would you like me to refine?

        1. Sharpen problem statement
        2. Clarify success metrics
        3. Add requirements
        4. Adjust priorities
        5. Expand risks/assumptions
        6. Looks good

User: 6

Claude: PRD saved to [location]. Ready for stakeholder review!
```

## Framework References

This skill incorporates best practices from:

- **[Marty Cagan (SVPG)](https://www.svpg.com/)** - Focus on problems, not solutions. Let engineering solve creatively.
- **[Teresa Torres](https://www.producttalk.org/)** - Continuous discovery, opportunity solution trees.
- **[Shreyas Doshi](https://twitter.com/shreyas)** - High-agency thinking, real vs. assumed problems.
- **[Yuhki Yamashita (Figma)](https://www.figma.com/)** - Three questions: What's the problem? Why now? What's our unique solution?
- **[Amazon Working Backwards](https://www.aboutamazon.com/)** - Start with the press release. Customer benefit before features.
- **[Lenny Rachitsky](https://www.lennysnewsletter.com/)** - 7 principles for great PRDs. Write with your team.
- **[Stripe](https://stripe.com/)** - Engineers as owners. Include alternatives considered.
- **[Jobs to be Done](https://jtbd.info/)** - Frame requirements around the job users hire the product to do.
- **[Aha.io PRD Guide](https://www.aha.io/roadmapping/guide/requirements-management/what-is-a-good-product-requirements-document-template)** - Systematic PRD creation with context and assumptions.

### Quotes from Product Leaders

> "A good PRD should evoke emotion. When your reader can feel the pain of the problem and the joy of the proposed solution, they'll be more likely to join you in building it." — **Sarah Tavel**, Benchmark

> "The purpose of a spec is to articulate the problem and bring everyone together on the solution, not to dictate." — **Ravi Mehta**, CPO at Tinder

## Customization

### Modifying Interview Questions

Edit the `SKILL.md` file to customize:
- Question wording
- Answer options
- Number of rounds
- Which questions to ask

### Modifying PRD Template

The PRD template structure is in Step 3 of `SKILL.md`. You can:
- Add/remove sections
- Change section order
- Add company-specific fields

### Domain-Specific Adaptations

**For B2B/Enterprise:**
- Add questions about buyer vs. user
- Include procurement/security requirements

**For Regulated Industries:**
- Add compliance/regulatory sections
- Include audit trail needs

**For Platform/API Products:**
- Add developer experience questions
- Include API design principles

## Requirements

- [Claude Code](https://claude.ai/code) CLI
- Claude Code skills support (included in recent versions)

## Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Ideas for improvement:
- [ ] Industry-specific question sets
- [ ] Integration with project management tools
- [ ] PRD templates for different product types
- [ ] Assumption testing frameworks

## License

MIT License - See [LICENSE](LICENSE) file.

## Acknowledgments

This skill was developed through actual PRD-writing sessions and refined based on research into how the best product managers approach requirements documentation.
