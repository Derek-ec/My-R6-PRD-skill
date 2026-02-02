---
name: write-prd
description: Use this skill when the user asks to "write a PRD", "create a PRD", "help me with a PRD", "product requirements document", or wants to document product requirements for a feature or product. This skill uses an interview-driven approach to gather requirements and generate a structured PRD.
version: 3.0.0
allowed-tools: [Read, Write, Edit, Glob, Grep, AskUserQuestion]
---

# Write PRD - Interview-Driven PRD Generator

This skill helps create comprehensive Product Requirements Documents through a structured interview process based on best practices from top product managers (Shreyas Doshi, Marty Cagan, Teresa Torres, Yuhki Yamashita, Amazon's Working Backwards, and the Jobs-to-be-Done framework).

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

> **Problem First, Solution Second** - The best PRDs obsess over the problem before prescribing solutions. Engineers should have room to solve problems creatively. Define the outcome you want, not just the features.

> **Interview First, Write Second** - Ask targeted questions to understand the problem, users, and constraints before generating any document. A 15-minute interview produces a better PRD than guessing.

> **Every Word Earns Its Place** - A strong PRD captures only what is necessary to align teams and guide development. Cut ruthlessly. If a sentence doesn't help someone build or decide, delete it.

> **Quantify Everything** - Vague problems get vague solutions. Replace "users struggle" with "40% of users abandon at checkout." Numbers create urgency and clarity.

---

## 7 Universal Principles for Powerful PRDs

Based on research from Lenny Rachitsky, Figma, Stripe, and Amazon:

1. **Make it scannable** - Add TL;DRs, use formatting, cut what you can. Busy engineers skim first.
2. **Tailor the format** - Not every project needs a 10-section PRD. Scale to the problem.
3. **Problem over solution** - The problem statement is the only thing that shouldn't change. Solutions evolve.
4. **Write it with your team** - Include eng and design early. A PRD written in isolation fails.
5. **Use it to achieve alignment** - Stress test it in reviews. Disagreements surface requirements.
6. **Revisit and update** - Keep the spec fresh. A stale PRD is worse than no PRD.
7. **Don't skip the postmortem** - After launch, document what you learned for the next PRD.

## Workflow Overview

1. **Gather Context** - Read any existing documents the user provides
2. **Run Discovery Interview** - 5 rounds of targeted questions using AskUserQuestion
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
  4. Skip questions already answered by the context

If no files provided:
  1. Ask if there are any existing documents to review
  2. Proceed to interview
```

---

## Step 2: Discovery Interview

Use AskUserQuestion tool for structured discovery. Run 5 rounds of questions.

**Important:**
- Adapt questions based on context - skip what's already known
- Listen for custom answers - they often provide richer context
- Ask follow-up clarifying questions when answers are vague
- The goal is to understand the problem deeply, not just fill out a template

### Round 1: Problem Validation

These questions determine if we're solving a real problem worth solving.

```yaml
questions:
  - question: "How do users solve this problem today?"
    header: "Current State"
    multiSelect: false
    options:
      - label: "No solution exists"
        description: "Users simply can't do this - they give up or go without"
      - label: "Manual workarounds"
        description: "Users cobble together spreadsheets, emails, or manual processes"
      - label: "Competitor products"
        description: "Users rely on other tools that don't fully meet their needs"
      - label: "Internal tools"
        description: "We have something, but it's inadequate or hard to use"
      - label: "Let me explain"
        description: "I'll describe the current situation"

  - question: "How do we know this is a real problem (not an assumed one)?"
    header: "Problem Evidence"
    multiSelect: true
    options:
      - label: "User interviews/research"
        description: "We've talked to users who describe this pain"
      - label: "Support tickets/complaints"
        description: "Users are actively asking for this"
      - label: "Usage data"
        description: "Analytics show users struggling or dropping off"
      - label: "Lost deals/churn"
        description: "We're losing customers over this"
      - label: "Assumption - needs validation"
        description: "We believe this is a problem but haven't validated yet"
```

### Round 2: User & Context

These questions ensure we deeply understand who we're building for and their context.

```yaml
questions:
  - question: "Who feels this pain most acutely? (Primary user for V1)"
    header: "Target User"
    multiSelect: false
    options:
      - label: "End user/consumer"
        description: "Individual people using the product directly"
      - label: "Business user"
        description: "Employees using it for their job (not technical)"
      - label: "Technical user"
        description: "Developers, engineers, or IT staff"
      - label: "Buyer (not user)"
        description: "Person who purchases but doesn't use directly"
      - label: "Multiple personas"
        description: "Two or more user types with equal priority"
      - label: "Let me describe"
        description: "I'll explain the specific persona"

  - question: "What triggers the user to need this? (The 'job to be done')"
    header: "Trigger/Context"
    multiSelect: false
    options:
      - label: "Time-based"
        description: "Daily, weekly, monthly - part of a regular workflow"
      - label: "Event-based"
        description: "Triggered by something happening (new customer, error, etc.)"
      - label: "Goal-based"
        description: "User has a specific outcome they're trying to achieve"
      - label: "Pain-based"
        description: "User is frustrated and looking for relief"
      - label: "Let me explain"
        description: "I'll describe the specific trigger and context"
```

### Round 3: Strategic Fit & Timing

These questions ensure this is the right thing to build right now.

```yaml
questions:
  - question: "Why build this now? What's the urgency?"
    header: "Timing"
    multiSelect: false
    options:
      - label: "Customer commitment"
        description: "Promised to a customer or required for a deal"
      - label: "Competitive pressure"
        description: "Competitors have this or market is moving"
      - label: "Strategic initiative"
        description: "Supports a company-wide goal or bet"
      - label: "Technical opportunity"
        description: "New capability makes this possible/easier now"
      - label: "Accumulated demand"
        description: "Requests have piled up - can't ignore anymore"
      - label: "No specific urgency"
        description: "Important but not time-sensitive"

  - question: "Is this a 'painkiller' or a 'vitamin'?"
    header: "Problem Severity"
    multiSelect: false
    options:
      - label: "Painkiller (must-have)"
        description: "Solves acute pain - users desperately need this"
      - label: "Vitamin (nice-to-have)"
        description: "Makes things better but users can live without it"
      - label: "Strategic bet"
        description: "Unlocks future opportunity even if not urgent now"
      - label: "Not sure yet"
        description: "Need to validate severity with users"
```

### Round 4: Success & Trade-offs

These questions define what winning looks like and what we're NOT doing.

```yaml
questions:
  - question: "What does success look like? (Pick the primary metric)"
    header: "Success Metric"
    multiSelect: false
    options:
      - label: "Adoption/usage"
        description: "X users actively using this feature"
      - label: "Task completion"
        description: "Users can complete [workflow] successfully"
      - label: "Time savings"
        description: "Reduces time to do X from Y to Z"
      - label: "Error reduction"
        description: "Fewer mistakes, support tickets, or failures"
      - label: "Revenue impact"
        description: "Increases conversion, retention, or deal size"
      - label: "Let me specify"
        description: "I have a specific metric in mind"

  - question: "What are we explicitly NOT doing in V1? (Non-goals)"
    header: "Out of Scope"
    multiSelect: true
    options:
      - label: "Power-user features"
        description: "Advanced capabilities that can wait"
      - label: "Integrations"
        description: "Connections to other tools/systems"
      - label: "Additional platforms"
        description: "Mobile, desktop, API - focus on one first"
      - label: "Edge cases"
        description: "Handle the 80% case, not every scenario"
      - label: "Polish/delight"
        description: "Functional first, delightful later"
      - label: "Let me specify"
        description: "I'll list specific non-goals"
```

### Round 5: Risks & Requirements

These questions surface unknowns and define what we need to document.

```yaml
questions:
  - question: "What's the hardest or riskiest part of this?"
    header: "Key Risk"
    multiSelect: true
    options:
      - label: "Technical feasibility"
        description: "Not sure if/how we can build this"
      - label: "User adoption"
        description: "Will users actually use it?"
      - label: "Scope creep"
        description: "Could easily grow beyond what's manageable"
      - label: "Dependencies"
        description: "Relies on other teams, systems, or decisions"
      - label: "Timeline"
        description: "Tight deadline with uncertainty"
      - label: "No major risks"
        description: "Straightforward problem and solution"

  - question: "What level of detail does this PRD need?"
    header: "Detail Level"
    multiSelect: false
    options:
      - label: "Problem-focused (Recommended)"
        description: "Define the problem and success criteria - let eng/design solve it"
      - label: "Solution-outlined"
        description: "Include high-level solution approach and key flows"
      - label: "Fully specified"
        description: "Detailed requirements, edge cases, and acceptance criteria"
```

---

## Step 3: Generate PRD

After completing the interview, generate a PRD with this structure.

**Key principles for generation:**
- Focus on the problem more than the solution
- Include what we're NOT doing (non-goals)
- Make success metrics specific and measurable
- Document assumptions and risks explicitly
- Leave room for engineering creativity

---

## Section-by-Section Writing Guide

Before using the template, understand how to write each section powerfully.

### Writing Problem Statements

**The Formula:** `[User segment] + [problem] + [impact] + [evidence]`

**Rules:**
- 3-4 sentences maximum for the core problem
- Quantify the pain with specific numbers
- Describe the problem, never the solution
- Use the 5 Whys to find root causes

**❌ Weak:** "Users don't like our checkout process."

**✅ Strong:** "40% of users abandon their cart at checkout, costing $2M in lost monthly revenue. Exit surveys cite 'too many steps' as the primary reason."

**❌ Weak:** "Our app crashes sometimes."

**✅ Strong:** "15% of active users experience crashes weekly, leading to a 10% drop in 7-day retention and 23 support tickets per day."

---

### Writing Success Metrics

**Rules:**
- Every metric needs: baseline → target → timeframe
- Include both leading indicators (early signals) and lagging indicators (outcomes)
- Make metrics testable - if you can't measure it, rewrite it
- Define kill criteria - what failure looks like

**❌ Weak:** "Users should have a good experience."

**✅ Strong:** "Page load time < 2 seconds for 95th percentile users."

**❌ Weak:** "Improve customer support."

**✅ Strong:** "Reduce average time-to-resolution from 48 hours to 24 hours within 30 days of launch."

---

### Writing Target User Sections

**Rules:**
- Never use generic "user" - use specific roles (e.g., "field technician," "marketing manager")
- Base personas on research, not assumptions
- Keep to 1-2 paragraphs per persona
- Include a representative quote that captures their frustration

**❌ Weak:** "Users who need to manage data."

**✅ Strong:** "Operations managers at manufacturing plants (50-200 employees) who currently track production metrics across 3+ disconnected spreadsheets. They check these daily but trust the data only weekly due to manual entry errors."

**Include:**
- Who they are (role, context, technical proficiency)
- What triggers their need (the job to be done)
- How they solve it today (current workflow)
- A quote: *"I spend 2 hours every Monday reconciling numbers that should match automatically."*

---

### Writing Requirements & Acceptance Criteria

**User Story Format:** "As a [specific role], I want to [action] so that [measurable benefit]."

**INVEST Criteria** - Every requirement should be:
- **I**ndependent - Can be built without other stories
- **N**egotiable - Details can be discussed with eng
- **V**aluable - Delivers clear user value
- **E**stimable - Team can estimate effort
- **S**mall - Completable in one sprint
- **T**estable - Has clear pass/fail criteria

**Two Formats for Acceptance Criteria:**

**Format 1: Rule-Based (Checklist)**
```
- Username must be 3-20 characters
- Password requires 1 uppercase, 1 number, 8+ characters
- Error message appears within 200ms of invalid input
- Session times out after 15 minutes of inactivity
```

**Format 2: Scenario-Based (Given/When/Then)**
```
GIVEN I am a logged-in user with items in my cart
WHEN I click "Checkout"
THEN I see my saved payment method pre-selected
AND I see my default shipping address pre-filled
AND I can complete purchase in ≤3 clicks
```

**❌ Weak acceptance criteria:**
- "The system should perform well"
- "User is notified"
- "Data is validated"

**✅ Strong acceptance criteria:**
- "System processes 1,000 transactions/second at p99 < 200ms"
- "User receives email confirmation within 5 minutes"
- "Invalid entries show inline error before form submission"

---

### Writing Non-Goals

**Rules:**
- Include things stakeholders might reasonably expect (not obvious exclusions)
- Be specific enough to prevent scope creep arguments
- Explain *why* it's deferred, not just *that* it's deferred
- Non-goals often become V2 goals - this is your future backlog

**❌ Weak non-goals:**
- "We won't build a spaceship" (obviously out of scope)
- "Advanced features" (too vague to enforce)

**✅ Strong non-goals:**
- "Mobile app - V1 is desktop-only. Mobile usage is 12% of our traffic; we'll revisit after validating desktop adoption."
- "Multi-language support - English only for launch. Localization adds 3 weeks; we'll prioritize based on international signup rates."
- "Real-time sync - Updates refresh on page load. Real-time adds infrastructure complexity we'll evaluate post-launch."

---

### Writing Assumptions & Risks

**Assumptions Structure:**
| What we believe | Impact if wrong | How to validate |
|-----------------|-----------------|-----------------|

**Risk Categories to Consider:**
- **Technical:** Can we build it? Will it scale?
- **Adoption:** Will users actually use it?
- **Dependencies:** What could block us?
- **Timeline:** What makes the deadline slip?

**❌ Weak risk:** "Things might go wrong."

**✅ Strong risk:** "Third-party payment API has 99.5% uptime SLA. During outages, users cannot complete purchases. Mitigation: Queue failed transactions for retry; show 'payment processing' status."

---

### Writing User Flows

**Rules:**
- PRD flows are high-level (5-10 steps max)
- Detailed interactions belong in design specs
- Focus on the "what," let design define the "how"
- Include decision points and error states

**Symbols:**
- Rectangle = Screen/page
- Diamond = Decision point
- Arrow = Flow direction

**Format:**
```
Flow: First-Time User Completes Setup
1. User clicks "Get Started" → Onboarding screen
2. User enters company name → [Validation: 2-50 chars]
3. User selects industry → Dropdown, 12 options
4. System creates workspace → Loading state, <3 sec
5. User sees dashboard → Success state with guided tour prompt
   └─ Error: Creation fails → Retry button + support link
```

---

### PRD Template Structure

```markdown
# [Product Name] - Product Requirements Document

**Version:** 1.0
**Date:** [Today's date]
**Author:** [Author name]
**Status:** Draft

---

## TL;DR

[3-5 bullet points. Busy stakeholders read this first. Include: the problem, the solution approach, primary success metric, and target launch date.]

---

## 1. Problem Statement

### The Problem
[2-3 sentences. Quantify the pain. No solutions here.]

### How Users Solve This Today
[Current workarounds, competitor products, or why they go without]

### Why This Matters Now
[Strategic urgency. What's the cost of waiting?]

### Evidence
[User research, support data, analytics. How do we KNOW this is real?]

---

## 2. Goals & Success Metrics

### Primary Goal
[One sentence. What does winning look like?]

### Success Metrics
| Metric | Current | Target | Timeframe |
|--------|---------|--------|-----------|
| [Primary - the number that matters most] | [Baseline] | [Target] | [When] |
| [Leading indicator - early signal] | [Baseline] | [Target] | [When] |

### Kill Criteria
[What signals would tell us to stop? Be specific.]

---

## 3. Target User

### Primary Persona: [Name]
**Who:** [Specific role and context - not "users"]
**Trigger:** "When [situation], I need to [action], so I can [outcome]."
**Today:** [How they solve this now - their current workflow]
**Quote:** *"[A real or representative quote capturing their frustration]"*

### Secondary Personas
| Persona | V1 Consideration |
|---------|------------------|
| [Name] | [How V1 addresses or defers their needs] |

---

## 4. Solution Overview

### Approach
[High-level direction. What are we building? Not a feature list - the concept.]

### Design Principles
[2-4 principles that guide decisions. Help eng/design make tradeoffs.]

| Principle | Implication |
|-----------|-------------|
| [e.g., "Speed over completeness"] | [e.g., "Ship MVP, iterate based on usage data"] |

---

## 5. Requirements

### Priority Definitions
| Priority | Meaning |
|----------|---------|
| **P0** | Must have - blocks launch |
| **P1** | Should have - launch is weaker without it |
| **P2** | Nice to have - adds value, not critical |

### [Category Name]

**[Requirement Title]** (P0/P1/P2)

*Story:* As a [specific role], I want to [action] so that [benefit].

*Acceptance Criteria:*
- [ ] [Specific, testable, measurable]
- [ ] [Specific, testable, measurable]

*Notes:* [Design considerations, technical context, or open questions]

---

## 6. User Flows

> High-level flows only. Design owns detailed interactions.

### Flow: [Name]
```
1. User [trigger] → [Screen]
2. User [action] → [System response]
3. [Decision point] →
   ├─ Yes: [Next step]
   └─ No: [Alternative]
4. [End state]
```

---

## 7. Non-Goals

**Explicitly out of scope for V1:**

| Item | Why Deferred |
|------|--------------|
| [Feature] | [Specific rationale - not "later"] |

---

## 8. Assumptions & Risks

### Assumptions
| Assumption | If Wrong | Validation |
|------------|----------|------------|
| [What we believe] | [Impact] | [How to test] |

### Risks
| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| [What could go wrong] | H/M/L | H/M/L | [Specific plan] |

---

## 9. Dependencies

| Dependency | Owner | Status | If Delayed |
|------------|-------|--------|------------|
| [What we need] | [Who] | [Status] | [Impact] |

---

## 10. Open Questions

| Question | Owner | Decide By |
|----------|-------|-----------|
| [Unresolved item] | [Who] | [Date] |

---

## Appendix

### A. Glossary
| Term | Definition |
|------|------------|
| [Domain term] | [Plain language definition] |

### B. References
- [User research link]
- [Competitive analysis link]
- [Design explorations link]

### C. Revision History
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
  - question: "What would you like me to refine?"
    header: "Feedback"
    multiSelect: true
    options:
      - label: "Sharpen problem statement"
        description: "Make the problem more specific or compelling"
      - label: "Clarify success metrics"
        description: "Make metrics more specific or add targets"
      - label: "Add requirements"
        description: "Document additional features or acceptance criteria"
      - label: "Adjust priorities"
        description: "Change P0/P1/P2 assignments"
      - label: "Expand risks/assumptions"
        description: "Document more unknowns"
      - label: "Looks good"
        description: "Ready for stakeholder review"
```

3. Apply any requested changes
4. Repeat until user is satisfied

---

## Output Location

Save the PRD to the user's specified location, or suggest:
- `[Product Name] - PRD V1.md` in the current working directory
- Or in a `docs/` folder if one exists

---

## Quality Checklist

Before finalizing any PRD, verify both content and writing quality.

### Content Completeness
- [ ] Problem is quantified with specific numbers
- [ ] Evidence proves this is real (not assumed)
- [ ] Success metrics have baseline → target → timeframe
- [ ] Kill criteria are defined
- [ ] Non-goals include things stakeholders might expect
- [ ] Risks have mitigation plans
- [ ] Open questions have owners and deadlines

### Writing Quality
- [ ] TL;DR is ≤5 bullets and captures the essence
- [ ] Problem statement is ≤4 sentences
- [ ] No solutions appear in the problem section
- [ ] User personas use specific roles (not "users")
- [ ] Acceptance criteria are testable (pass/fail)
- [ ] Every metric is measurable
- [ ] Non-goals explain *why*, not just *what*
- [ ] No filler words ("very," "really," "quite," "basically")

### Alignment Test
Read the PRD and ask:
- [ ] Could an engineer start building from this tomorrow?
- [ ] Could a designer start wireframing from this tomorrow?
- [ ] Would two engineers reading this build the same thing?
- [ ] If requirements conflict, does the PRD help prioritize?

### The "So What?" Test
For every section, ask "So what?" If you can't answer with impact, cut or rewrite it.

---

## Framework References

This skill incorporates best practices from:

- **Marty Cagan (SVPG):** Focus on problems, not solutions. Let engineering solve creatively.
- **Teresa Torres:** Continuous discovery, opportunity solution trees, weekly user contact.
- **Shreyas Doshi:** High-agency thinking, challenge assumptions, real vs. assumed problems.
- **Yuhki Yamashita (Figma):** Three questions framework - What's the problem? Why now? What's our unique solution?
- **Amazon Working Backwards:** Start with the press release. Write the customer benefit before the feature.
- **Stripe:** Engineers as owners. Specs include alternatives considered and rollout strategy.
- **Lenny Rachitsky:** 7 principles for great PRDs. Problem over solution. Write with your team.
- **Jobs to be Done:** Frame requirements around the job users hire the product to do.
- **Aha.io 10-Step Process:** Systematic PRD creation with context, assumptions, and metrics.

### Key Quotes from Product Leaders

> "A good PRD should evoke emotion. When your reader can feel the pain of the problem and the joy of the proposed solution, they'll be more likely to join you in building it." — Sarah Tavel, Benchmark

> "The purpose of a spec is to articulate the problem and bring everyone together on the solution, not to dictate." — Ravi Mehta, CPO at Tinder

> "A key tenet of being a PM is building alignment. A key tool for building alignment is a PM spec." — Paige Costello, Asana

---

## Customization

This skill can be adapted for specific contexts:

**For B2B/Enterprise:**
- Add questions about buyer vs. user
- Include procurement/security requirements
- Consider pilot/rollout strategy

**For Regulated Industries:**
- Add compliance/regulatory requirements section
- Include audit trail needs
- Document approval workflows

**For Platform/API Products:**
- Add developer experience questions
- Include API design principles
- Document integration patterns
