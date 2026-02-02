# Contributing to write-prd-skill

Thank you for your interest in contributing! This document provides guidelines for contributing to the write-prd skill.

## How to Contribute

### Reporting Issues

If you find a bug or have a suggestion:

1. Check existing issues to avoid duplicates
2. Create a new issue with:
   - Clear description of the problem or suggestion
   - Steps to reproduce (for bugs)
   - Expected vs actual behavior
   - Claude Code version

### Submitting Changes

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Make your changes
4. Test by copying to `~/.claude/skills/write-prd/` and running `/write-prd`
5. Commit with clear messages
6. Push to your fork
7. Open a Pull Request

## Areas for Contribution

### Interview Questions

The interview questions in `SKILL.md` can be improved:

- **Add industry-specific options** - Different industries have different PRD needs
- **Refine answer choices** - Make options clearer or more comprehensive
- **Add conditional questions** - Follow-up questions based on previous answers

### PRD Template

The PRD template structure can be enhanced:

- **Add new sections** - Company-specific needs (compliance, legal, etc.)
- **Improve formatting** - Better markdown structure
- **Add examples** - Inline examples for each section

### Documentation

- **More examples** - PRDs from different domains
- **Video tutorials** - Walkthrough of using the skill
- **Translations** - Non-English documentation

### New Features

Ideas we'd love to see:

- Integration with Jira/Linear/Notion for requirement import
- PRD comparison/diff tool
- Template variations (lean PRD, technical spec, etc.)
- Automated priority suggestions based on dependencies

## Code Style

### SKILL.md Format

- Use clear section headers with `##` and `###`
- Keep YAML blocks properly indented
- Include comments for complex logic
- Test all question flows

### Markdown

- Use consistent heading levels
- Include code blocks with language hints
- Keep lines under 100 characters where possible

## Testing

Before submitting:

1. Copy your modified `SKILL.md` to `~/.claude/skills/write-prd/`
2. Start a new Claude Code session
3. Run `/write-prd` and complete the full flow
4. Verify the generated PRD is well-formatted
5. Test edge cases (minimal answers, custom answers, etc.)

## Questions?

Open an issue with the "question" label and we'll help out.
