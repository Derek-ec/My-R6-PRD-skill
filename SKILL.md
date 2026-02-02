---
name: write-prd
description: Use this skill when the user asks to "write a PRD", "create a PRD", "help me with a PRD", "product requirements document", or wants to document product requirements for a feature or product. This skill uses an interview-driven approach to gather requirements and generate a structured PRD.
version: 1.0.0
allowed-tools: [Read, Write, Edit, Glob, Grep, AskUserQuestion]
---

# Write PRD - Interview-Driven PRD Generator

This skill helps create comprehensive Product Requirements Documents through a structured interview process, then generates a well-organized PRD in markdown format.

## When to Use This Skill

Activate when user:
- Asks to write, create, or help with a PRD
- Wants to document product requirements
- Says "PRD", "product requirements document"
- Needs to create requirements for a new feature or product

Do NOT activate for:
- Technical documentation or API docs
- Project status updates
- Bug reports or tickets

## Core Philosophy

> **Interview First, Write Second** - Ask targeted questions to understand the problem, users, and constraints before generating any document. A 10-minute interview produces a better PRD than guessing.

## Workflow Overview

1. **Gather Context** - Read any existing documents the user provides
2. **Run Discovery Interview** - 4-5 rounds of targeted questions using AskUserQuestion
3. **Generate PRD** - Create structured markdown document
4. **Review & Refine** - Iterate based on user feedback

---

## Step 1: Gather Context

Before asking questions, check if user provided context files:

```
If user provides files/folders:
  1. Read all provided files using Read tool
  2. Summarize key information found
  3. Use this context to inform interview questions

If no files provided:
  1. Ask if there are any existing documents to review
  2. Proceed to interview
```

---

## Step 2: Discovery Interview

Use AskUserQuestion tool for structured discovery. Run 4-5 rounds of questions.

### Round 1: Core Problem & Solution

```yaml
questions:
  - question: "What is the primary pain point this product/feature solves? What's broken today?"
    header: "Core Problem"
    multiSelect: false
    options:
      - label: "No tool exists"
        description: "Users have no way to accomplish this task today"
      - label: "Current tools inadequate"
        description: "Existing tools are too slow, complex, or limited"
      - label: "Manual process"
        description: "Users do this manually and it's time-consuming"
      - label: "Let me explain"
        description: "I'll provide a custom description"

  - question: "Who is the primary user persona for V1?"
    header: "Primary User"
    multiSelect: false
    options:
      - label: "Internal team"
        description: "Employees, operators, admins within the organization"
      - label: "End customer"
        description: "External paying customers or users"
      - label: "Multiple personas"
        description: "Two or more user types are equally important"
      - label: "Let me specify"
        description: "I'll describe the specific persona"
```

### Round 2: Scope & AI

```yaml
questions:
  - question: "What level of AI/automation should be included in V1?"
    header: "AI Scope"
    multiSelect: false
    options:
      - label: "AI co-pilot (Recommended)"
        description: "AI assists but human validates - drafts, suggestions, automation with review"
      - label: "No AI for V1"
        description: "Manual/template-based - AI features come later"
      - label: "Full AI autonomy"
        description: "AI handles tasks end-to-end without human review"
      - label: "TBD"
        description: "Haven't decided yet"

  - question: "What interaction model should the UI emphasize?"
    header: "UI Direction"
    multiSelect: false
    options:
      - label: "Visual canvas"
        description: "Map, diagram, or visual workspace as primary interface"
      - label: "Form-based wizard"
        description: "Step-by-step guided flow with forms"
      - label: "Template gallery"
        description: "Start from templates, customize in place"
      - label: "Hybrid approach"
        description: "Combine multiple interaction patterns"
```

### Round 3: Success & Constraints

```yaml
questions:
  - question: "What are the key success metrics for this product?"
    header: "Success Metrics"
    multiSelect: true
    options:
      - label: "Time savings"
        description: "Users complete tasks faster (e.g., <15 minutes)"
      - label: "Self-service rate"
        description: "% of tasks completed without support/engineering help"
      - label: "Adoption"
        description: "Number of users/teams actively using the product"
      - label: "Quality improvement"
        description: "Error reduction, consistency, accuracy improvements"

  - question: "What should be explicitly OUT OF SCOPE for V1?"
    header: "Out of Scope"
    multiSelect: true
    options:
      - label: "Advanced AI features"
        description: "Complex AI/ML capabilities - keep V1 simple"
      - label: "External integrations"
        description: "Third-party system integrations"
      - label: "Mobile/offline support"
        description: "Mobile apps or offline functionality"
      - label: "Let me specify"
        description: "I'll list specific exclusions"
```

### Round 4: Platform & Security

```yaml
questions:
  - question: "What deployment model should the PRD specify?"
    header: "Deployment"
    multiSelect: false
    options:
      - label: "Web application"
        description: "Browser-based, requires network"
      - label: "Desktop application"
        description: "Installed app, works offline"
      - label: "Desktop with sync"
        description: "Desktop app with optional cloud sync"
      - label: "TBD"
        description: "Deployment model not yet decided"

  - question: "Are there security/classification constraints?"
    header: "Security"
    multiSelect: false
    options:
      - label: "Unclassified only"
        description: "V1 targets unclassified environments"
      - label: "Must support classified"
        description: "Needs to work on classified networks"
      - label: "No constraints"
        description: "Standard security practices sufficient"
      - label: "TBD"
        description: "Security requirements not yet defined"
```

### Round 5: UI/UX Flows

```yaml
questions:
  - question: "What are the core user workflows to document?"
    header: "Key Flows"
    multiSelect: true
    options:
      - label: "Create from scratch"
        description: "User creates new item/content from empty state"
      - label: "Use template"
        description: "User starts from template and customizes"
      - label: "Edit existing"
        description: "User modifies existing item/content"
      - label: "Review/approve"
        description: "User reviews and approves work"

  - question: "How detailed should UI/UX flow descriptions be?"
    header: "Detail Level"
    multiSelect: false
    options:
      - label: "High-level steps only (Recommended)"
        description: "e.g., 'User places item, configures settings, saves' - design fills details"
      - label: "Detailed with wireframe guidance"
        description: "Specific screens, controls, interactions for each step"
      - label: "User journey with decision points"
        description: "Document decision trees and branching paths"
```

---

## Step 3: Generate PRD

After completing the interview, generate a PRD with this structure:

### PRD Template Structure

```markdown
# [Product Name] - Product Requirements Document

**Version:** 1.0
**Date:** [Today's date]
**Author:** [Product Team]
**Status:** Draft
**Target Release:** [Date if known]

---

## 1. Problem Statement

### The Challenge
[Describe what's broken today - 2-3 paragraphs based on interview]

### Today's Pain Points
1. **[Pain point 1]** - [Description]
2. **[Pain point 2]** - [Description]
3. **[Pain point 3]** - [Description]

### The Impact
- [Impact 1]
- [Impact 2]
- [Impact 3]

---

## 2. Solution Overview

### What We're Building
[1-2 sentence description of the solution]

### Core Philosophy
> [Guiding principle or philosophy quote if applicable]

### Solution Principles
| Principle | Description |
|-----------|-------------|
| [Principle 1] | [Description] |
| [Principle 2] | [Description] |

### Deployment Model
[Desktop/Web/TBD - from interview]

### Security Classification
[Security constraints from interview]

---

## 3. Target Users

### Primary Personas
[For each persona identified in interview:]

#### [Persona Name]
- **Role:** [Description]
- **Context:** [When/how they use the product]
- **Key need:** [Primary need]
- **Success metric:** [How we measure their success]

### Secondary Personas (V1 Awareness)
| Persona | V1 Consideration |
|---------|------------------|
| [Name] | [How V1 addresses or defers their needs] |

---

## 4. Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| [Metric from interview] | [Target] | [How measured] |

---

## 5. Jobs to Be Done (Requirements)

### Priority Definitions
| Priority | Definition |
|----------|------------|
| **P0** | Must have for MVP |
| **P1** | Should have for MVP; critical for adoption |
| **P2** | Nice to have; can ship without |
| **Stretch** | Valuable but not committed |

[For each requirement category, use this format:]

### Category N: [Category Name]

#### Job N.N: [Job Title] (P0/P1/P2)
**When** [context/situation]
**I want to** [action/capability]
**So that** [benefit/outcome]

**Acceptance Criteria:**
- [Criterion 1]
- [Criterion 2]
- [Criterion 3]

---

## 6. Core UI/UX Flows

> **Note:** These are high-level flows. Design team will define detailed interactions.

### Flow 1: [Flow Name]
```
1. [Step 1]
2. [Step 2]
3. [Step 3]
...
```

[Repeat for each flow identified in interview]

---

## 7. Platform Considerations

[If multiple platforms, document each:]

| Platform | Considerations |
|----------|----------------|
| [Platform 1] | [Notes] |

**Common Requirements:**
- [Requirement 1]
- [Requirement 2]

---

## 8. Out of Scope for V1

| Item | Rationale |
|------|-----------|
| [Item from interview] | [Why excluded] |

---

## 9. Dependencies & Assumptions

### Dependencies
| Dependency | Owner | Impact if Delayed |
|------------|-------|-------------------|
| [Dependency] | [Team] | [Impact] |

### Assumptions
1. [Assumption 1]
2. [Assumption 2]

---

## 10. Open Questions

| Question | Owner | Due Date |
|----------|-------|----------|
| [Question] | [Owner] | TBD |

---

## Appendix A: Priority Summary

| Priority | Jobs | Count |
|----------|------|-------|
| **P0** | [List] | [N] |
| **P1** | [List] | [N] |
| **P2** | [List] | [N] |
| **Stretch** | [List] | [N] |

---

## Appendix B: Glossary

| Term | Definition |
|------|------------|
| [Term] | [Definition] |

---

## Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | [Date] | [Author] | Initial draft |
```

---

## Step 4: Review & Refine

After generating the PRD:

1. Present the PRD to the user
2. Ask for feedback using AskUserQuestion:

```yaml
questions:
  - question: "Would you like me to refine any section of the PRD?"
    header: "PRD Feedback"
    multiSelect: true
    options:
      - label: "Problem statement needs more detail"
        description: "Expand pain points or add examples"
      - label: "Add more UI/UX flow detail"
        description: "Break down flows into more steps"
      - label: "Adjust requirements priorities"
        description: "Change P0/P1/P2 assignments"
      - label: "Looks good as-is"
        description: "PRD is ready for team review"
```

3. Apply any requested changes
4. Repeat until user is satisfied

---

## Output Location

Save the PRD to the user's specified location, or suggest:
- `[Project Name] - PRD V1.md` in the current working directory
- Or in a `docs/` folder if one exists

---

## Tips for Best Results

**During Interview:**
- Listen for custom answers - users often select "Let me explain" and provide richer context
- If answers are vague, ask follow-up clarifying questions
- Note any existing documents or context files mentioned

**During Generation:**
- Use specific language from the interview in the PRD
- Include concrete examples where possible
- Keep acceptance criteria testable and specific
- Prioritize ruthlessly - most things should be P1 or P2, not P0

**During Refinement:**
- Make surgical edits rather than regenerating entire sections
- Track what changed in the revision history
- Confirm changes with user before moving on

---

## Example Invocation

User: "Help me write a PRD for our new scenario planning tool"

Claude:
1. Asks if there are existing documents to review
2. Reads any provided files
3. Runs 4-5 rounds of interview questions
4. Generates structured PRD
5. Asks for feedback and refines
6. Saves final PRD to specified location
