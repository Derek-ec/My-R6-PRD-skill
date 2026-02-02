# Write PRD - Interview-Driven PRD Generator for Claude Code

A Claude Code skill that generates comprehensive Product Requirements Documents through a structured interview process.

## What It Does

Instead of staring at a blank page, this skill guides you through targeted questions to gather requirements, then generates a well-structured PRD in markdown format.

**The process:**
1. **Context Gathering** - Reads any existing documents you provide
2. **Structured Interview** - 4-5 rounds of multiple-choice questions covering:
   - Core problem & primary user persona
   - AI/automation scope & UI direction
   - Success metrics & out-of-scope items
   - Deployment model & security constraints
   - Key UI/UX flows & detail level
3. **PRD Generation** - Creates a 10-section markdown document
4. **Refinement** - Iterates based on your feedback

## PRD Structure

The generated PRD includes:

| Section | Purpose |
|---------|---------|
| 1. Problem Statement | What's broken today, pain points, impact |
| 2. Solution Overview | What we're building, principles, deployment |
| 3. Target Users | Primary & secondary personas with success metrics |
| 4. Success Metrics | Measurable targets and how to track them |
| 5. Jobs to Be Done | Requirements in JTBD format with acceptance criteria |
| 6. Core UI/UX Flows | High-level user workflows |
| 7. Platform Considerations | Platform-specific notes |
| 8. Out of Scope | What's explicitly excluded from V1 |
| 9. Dependencies & Assumptions | Blockers and assumptions |
| 10. Open Questions | Unresolved items with owners |

Plus appendices for priority summary, glossary, and revision history.

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
- Feature spec documents
- Interview transcripts
- Existing requirements
- Design briefs
```

## Example Session

```
User: /write-prd

Claude: I'll help you create a PRD. Do you have any existing documents
        I should review first?

User: Yes, read the files in ~/Documents/feature-planning/

Claude: [Reads files, summarizes context]

        Now I'll ask some questions to understand your requirements.

        [Round 1: Core Problem]
        What is the primary pain point this product/feature solves?

        1. No tool exists
        2. Current tools inadequate
        3. Manual process
        4. Let me explain

User: 2

Claude: [Continues through 4-5 rounds of questions]
        ...

        [Generates PRD]

        I've created the PRD. Would you like me to refine any section?

        1. Problem statement needs more detail
        2. Add more UI/UX flow detail
        3. Adjust requirements priorities
        4. Looks good as-is

User: 4

Claude: PRD saved to [location]. Ready for team review!
```

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
- Modify the markdown format
- Add company-specific fields

### Adding Reference Materials

Create a `references/` subdirectory for:
- PRD examples
- Company templates
- Style guides

```
write-prd/
├── SKILL.md
├── references/
│   ├── example-prd.md
│   └── company-template.md
```

## Requirements

- [Claude Code](https://claude.ai/code) CLI
- Claude Code skills support (included in recent versions)

## How It Works

Claude Code skills are markdown files that provide Claude with specialized knowledge and workflows. When you invoke `/write-prd` or mention PRD-related keywords, Claude:

1. Loads the skill instructions from `SKILL.md`
2. Follows the defined workflow (gather context → interview → generate → refine)
3. Uses the `AskUserQuestion` tool for structured interviews
4. Generates output based on the PRD template

## Contributing

Contributions welcome! Ideas for improvement:

- [ ] Industry-specific question sets (SaaS, hardware, gaming, etc.)
- [ ] Integration with project management tools
- [ ] PRD diff/versioning support
- [ ] Multi-language support

## License

MIT License - See [LICENSE](LICENSE) file.

## Acknowledgments

This skill was developed through an actual PRD-writing session, capturing the interview-driven approach that produced effective results. The workflow prioritizes understanding the problem space before generating documentation.
