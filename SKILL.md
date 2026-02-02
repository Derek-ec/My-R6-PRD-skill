---
name: write-prd
description: Use this skill when the user asks to "write a PRD", "create a PRD", "help me with a PRD", "product requirements document", or wants to document product requirements for a feature or product. This skill uses an interview-driven approach to gather requirements and generate a structured PRD for ATARS and Red 6 products.
version: 4.1.0
allowed-tools: [Read, Write, Edit, Glob, Grep, AskUserQuestion]
---

# Write PRD - ATARS Product Requirements Generator

This skill helps create comprehensive Product Requirements Documents for ATARS products through a structured interview process. Optimized for Red 6's military aviation training context.

## When to Use This Skill

Activate when user:
- Asks to write, create, or help with a PRD
- Wants to document product requirements for ATARS features
- Says "PRD", "product requirements document"
- Needs requirements for ATARS 2.0, ATARS Planning, ATARS Debrief, ATARS 1.2, or related products

Do NOT activate for:
- Technical documentation or API docs
- Project status updates
- Bug reports or tickets

---

## ATARS Domain Context

### About Red 6

Red 6 builds augmented reality training and operational systems for military aviation. ATARS enables pilots to train against synthetic threats rendered in AR during actual flight.

### Value Proposition: The Three Pillars

All ATARS products support these core outcomes:

| Pillar | Description |
|--------|-------------|
| **Pilot Production** | Increase throughput of trained pilots through the pipeline |
| **Absorption** | Help squadrons absorb more training sorties with existing resources |
| **Retention** | Improve pilot retention through better, more engaging training |

### Key Products

| Product | Description |
|---------|-------------|
| **ATARS 2.0** | Next-gen certified PFR headset for training + operations |
| **ATARS Planning** | Ground-based scenario creation tool for IPs and mission planners |
| **ATARS Debrief** | Post-flight analysis and training review tool |
| **ATARS 1.2** | Daytime training and daytime flight reference capability |

### Target User Types

| User Type | Description | Context |
|-----------|-------------|---------|
| **IP (Instructor Pilot)** | Sets up and runs training scenarios for students | Time-constrained, runs multiple sorties/day |
| **Mission Planner (MP)** | Builds complex multi-phase scenarios for units | More planning time, higher scenario complexity |
| **Student Pilot** | Undergraduate pilot training (UPT/U-Jets) | Consumes pre-built scenarios, follows syllabus |
| **FTU Pilot** | Already trained pilot at Formal Training Unit | Learning new airframe, higher baseline skill |
| **Operational Pilot** | Career pilot doing mission readiness or remedial training | Maintains proficiency, addresses gaps |
| **EXCON (Exercise Controller)** | Manages large force exercises | Multi-aircraft coordination, real-time adjustments |

### Platforms (2026 Focus)

| Platform | Command | Notes |
|----------|---------|-------|
| **T-38** | AETC | Primary training aircraft; BFM, tactical intercept, formation |
| **T-45** | Navy | U-Jets training; syllabus-aligned, pre-scripted scenarios |
| **F-16** | ACC | Legacy fighter; ATARS 2.0 certification pathway |
| **MC-130J** | AFSOC | Special operations; penetration, threat avoidance |
| **T-7** | Future | Next-gen trainer |

*Note: Platform focus expands annually. PRDs should specify which platforms are in scope for the capability.*

### ATARS Glossary

| Term | Definition |
|------|------------|
| **PFR** | Primary Flight Reference - certified flight display |
| **Constructive Entity** | Synthetic/simulated aircraft or threat |
| **"Behind the Glass"** | Integration where ATARS data appears on cockpit displays |
| **BFM** | Basic Fighter Maneuvers |
| **ACM** | Air Combat Maneuvering |
| **SEAD/DEAD** | Suppression/Destruction of Enemy Air Defenses |
| **SAM** | Surface-to-Air Missile |
| **MEZ/WEZ** | Missile/Weapon Engagement Zone |
| **Bullseye** | Common reference point for tactical communication |
| **LFE** | Large Force Engagement |
| **Sortie** | Single flight mission |
| **CARBON** | State synchronization framework for networked LVC |

---

## Core Philosophy

> **"Thin to Win"** - Build the minimum viable feature set, expand based on real feedback. Avoid over-engineering for hypothetical use cases.

> **Problem First, Solution Second** - The best PRDs obsess over the problem before prescribing solutions. IPs and MPs should have room to solve problems creatively.

> **Interview First, Write Second** - Ask targeted questions to understand the operational context, user workflows, and constraints before generating any document.

> **Every Word Earns Its Place** - A strong PRD captures only what is necessary to align teams and guide development. If a sentence doesn't help someone build or decide, delete it.

---

## Workflow Overview

1. **Gather Context** - Read any existing documents (ConOps, discovery notes, prior PRDs)
2. **Run Discovery Interview** - 5 rounds of targeted questions using AskUserQuestion
3. **Generate PRD** - Create structured markdown with JTBD-format requirements
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
  1. Ask if there are any existing documents to review (ConOps, discovery notes, prior PRDs)
  2. Proceed to interview
```

---

## Step 2: Discovery Interview

Use AskUserQuestion tool for structured discovery. Run 5 rounds of questions.

**Important:**
- Adapt questions based on context - skip what's already known
- Listen for custom answers - they often provide richer operational context
- Ask follow-up clarifying questions when answers are vague
- Think in terms of the ATARS value pillars (Production, Absorption, Retention)

### Round 1: Problem Validation

These questions determine if we're solving a real operational problem.

```yaml
questions:
  - question: "How do IPs/MPs solve this problem today?"
    header: "Current State"
    multiSelect: false
    options:
      - label: "No solution exists"
        description: "They can't do it - workaround or go without"
      - label: "Manual workarounds"
        description: "Paper, whiteboards, spreadsheets, verbal coordination"
      - label: "TACTS/ACMI/Legacy tools"
        description: "Existing training systems that don't fully meet needs"
      - label: "Contractor support"
        description: "Rely on Red 6 or other contractors to set up scenarios"
      - label: "Let me explain"
        description: "I'll describe the current operational workflow"

  - question: "How do we know this is a real problem (not an assumed one)?"
    header: "Problem Evidence"
    multiSelect: true
    options:
      - label: "Pilot/IP interviews"
        description: "We've talked to operators who describe this pain"
      - label: "Customer request"
        description: "AETC/ACC/AFSOC has explicitly asked for this"
      - label: "Observation during flights"
        description: "Seen the problem during ATARS demonstrations"
      - label: "Support/field issues"
        description: "Recurring problems reported from deployed units"
      - label: "Assumption - needs validation"
        description: "We believe this is a problem but haven't validated"
```

### Round 2: User & Context

These questions ensure we deeply understand who we're building for.

```yaml
questions:
  - question: "Who is the primary user for V1?"
    header: "Target User"
    multiSelect: false
    options:
      - label: "Instructor Pilot (IP)"
        description: "Runs daily training sorties, time-constrained"
      - label: "Mission Planner (MP)"
        description: "Builds complex scenarios, more planning time"
      - label: "EXCON"
        description: "Manages LFE/multi-aircraft exercises"
      - label: "Red 6 Technician"
        description: "Internal staff supporting customer operations"
      - label: "Multiple user types"
        description: "Two or more with equal priority"
      - label: "Let me describe"
        description: "I'll explain the specific user and their context"

  - question: "What triggers the need for this capability?"
    header: "Trigger/Context"
    multiSelect: false
    options:
      - label: "Pre-flight planning"
        description: "Setting up scenarios before the sortie"
      - label: "In-flight adjustment"
        description: "Real-time changes during training"
      - label: "Post-flight debrief"
        description: "Reviewing and analyzing training outcomes"
      - label: "Syllabus milestone"
        description: "Student reaches specific training stage"
      - label: "Let me explain"
        description: "I'll describe the specific operational trigger"
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
        description: "Promised to AETC/ACC or required for contract"
      - label: "Project Juice / POR"
        description: "Required for Program of Record pathway"
      - label: "Competitive pressure"
        description: "Other solutions emerging in the market"
      - label: "Demo/validation event"
        description: "Needed for upcoming demonstration"
      - label: "Accumulated demand"
        description: "Repeated requests from field units"
      - label: "No specific urgency"
        description: "Important but not time-sensitive"

  - question: "Which ATARS value pillar does this primarily support?"
    header: "Value Pillar"
    multiSelect: false
    options:
      - label: "Pilot Production"
        description: "Increases throughput of trained pilots"
      - label: "Absorption"
        description: "Helps squadrons handle more sorties with existing resources"
      - label: "Retention"
        description: "Improves training quality/engagement to retain pilots"
      - label: "Platform enablement"
        description: "Required for new aircraft integration"
      - label: "Operational capability"
        description: "Extends beyond training to operational missions"
```

### Round 4: Success & Trade-offs

These questions define what winning looks like and what we're NOT doing.

```yaml
questions:
  - question: "What does success look like? (Pick the primary metric)"
    header: "Success Metric"
    multiSelect: false
    options:
      - label: "Self-service rate"
        description: "IPs can create scenarios without contractor help"
      - label: "Time to scenario"
        description: "Reduces setup time from X to Y minutes"
      - label: "Scenario complexity"
        description: "Enables scenarios that weren't possible before"
      - label: "Adoption rate"
        description: "X% of sorties use ATARS Planning scenarios"
      - label: "Error reduction"
        description: "Fewer setup mistakes, less wasted flight time"
      - label: "Let me specify"
        description: "I have a specific metric in mind"

  - question: "What are we explicitly NOT doing in V1? (Non-goals)"
    header: "Out of Scope"
    multiSelect: true
    options:
      - label: "Classified environments"
        description: "V1 is unclassified only"
      - label: "Advanced entity behaviors"
        description: "Complex AI/reactive behaviors wait for V2"
      - label: "Multi-ship coordination"
        description: "Focus on single-aircraft scenarios first"
      - label: "Real-time in-flight editing"
        description: "Ground-based planning only for V1"
      - label: "Legacy system integration"
        description: "No TACTS/ACMI data import"
      - label: "Let me specify"
        description: "I'll list specific non-goals"
```

### Round 5: Risks & Requirements

These questions surface unknowns and define documentation depth.

```yaml
questions:
  - question: "What's the hardest or riskiest part of this?"
    header: "Key Risk"
    multiSelect: true
    options:
      - label: "Technical feasibility"
        description: "Not sure if/how ATARS can support this"
      - label: "User adoption"
        description: "Will IPs actually use it vs. existing workflows?"
      - label: "Platform variation"
        description: "Different aircraft have different requirements"
      - label: "Certification impact"
        description: "Could affect PFR or airworthiness certification"
      - label: "Timeline"
        description: "Tight deadline with uncertainty"
      - label: "No major risks"
        description: "Straightforward problem and solution"

  - question: "What level of detail does this PRD need?"
    header: "Detail Level"
    multiSelect: false
    options:
      - label: "Problem-focused (Recommended)"
        description: "Define problem and success - let eng/design solve it"
      - label: "Solution-outlined"
        description: "Include high-level solution approach and key flows"
      - label: "Fully specified"
        description: "Detailed requirements, edge cases, acceptance criteria"
```

---

## Step 3: Generate PRD

After completing the interview, generate a PRD with JTBD-format requirements.

**Key principles for generation:**
- Focus on the problem more than the solution
- Write requirements in JTBD format: "When... I want to... So that..."
- Include platform-specific considerations where relevant
- Make success metrics specific to ATARS operational context
- Apply "Thin to Win" - don't over-specify for V1

---

## Section-by-Section Writing Guide

### Writing Problem Statements (ATARS Context)

**The Formula:** `[User type] + [operational problem] + [impact on training/ops] + [evidence]`

**Rules:**
- 3-4 sentences maximum for the core problem
- Quantify the pain in operational terms (flight hours, setup time, sorties)
- Describe the problem, never the solution
- Connect to ATARS value pillars where possible

**❌ Weak:** "IPs have trouble setting up scenarios."

**✅ Strong:** "IPs at Sheppard AFB spend 45+ minutes per sortie configuring threat presentations manually, limiting them to 2 training sorties per day instead of the potential 4. This represents 50% of lost training capacity."

**❌ Weak:** "Mission planners need better tools."

**✅ Strong:** "Mission planners building LFE scenarios require contractor support for 80% of complex setups, creating a $50K/month dependency and 48-hour lead time for scenario changes."

---

### Writing Success Metrics (ATARS Context)

**Rules:**
- Every metric needs: baseline → target → timeframe
- Use operational metrics (sorties, flight hours, setup time)
- Consider self-service rate as a key indicator
- Define what failure looks like

**❌ Weak:** "IPs should find it easy to use."

**✅ Strong:** "Self-service rate: >90% of scenarios created without contractor support within 60 days of deployment."

**❌ Weak:** "Faster scenario creation."

**✅ Strong:** "Time to scenario: <15 minutes from launch to export-ready (baseline: 45 minutes with current workflow)."

**Example Metrics by Product:**

Choose metrics appropriate to the product. These are examples, not requirements.

| Product | Example Metrics |
|---------|-----------------|
| **ATARS Planning** | Self-service rate, time to scenario, template usage rate, scenario complexity |
| **ATARS Debrief** | Debrief completion rate, time to first insight, reconstruction accuracy, student comprehension |
| **ATARS 2.0** | System availability, tracking accuracy, pilot trust rating, time to operational |
| **ATARS 1.2** | Daytime usability score, glare/visibility issues, certification milestones |

**General Metric Categories:**
| Category | What to Measure |
|----------|-----------------|
| **Adoption** | % of eligible users/sorties using the capability |
| **Efficiency** | Time savings, steps reduced, throughput increase |
| **Quality** | Accuracy, error rate, reconstruction fidelity |
| **Self-Service** | % of tasks completed without contractor/support help |
| **Satisfaction** | User ratings, NPS, qualitative feedback |

---

### Writing Target User Sections (ATARS Context)

**Rules:**
- Use specific ATARS user types (IP, MP, Student, FTU Pilot, Operational Pilot, EXCON)
- Include platform and command context (T-38/AETC, F-16/ACC)
- Distinguish training phase (UPT, FTU, mission readiness, remedial)
- Base personas on discovery interviews, not assumptions
- Include a representative quote from actual user research

**❌ Weak:** "Users who plan training missions."

**✅ Strong:** "T-38 Instructor Pilots at Sheppard AFB (AETC) running 3-4 BFM sorties daily. They receive their flight schedule the afternoon before and have 30 minutes between sorties. Currently rely on whiteboards and verbal briefings for threat setup."

**✅ Strong (FTU context):** "F-16 pilots at Luke AFB transitioning from T-38. Already proficient in BFM fundamentals but learning platform-specific systems and tactics. Need realistic threat presentations that challenge beyond basic intercepts."

**Include:**
- User type (IP, MP, Student, FTU Pilot, Operational Pilot, EXCON)
- Platform and command context (T-38/AETC, F-16/ACC, MC-130J/AFSOC)
- Training phase (UPT, FTU, mission readiness, remedial)
- Time constraints and workflow triggers
- A quote: *"I'd want it all pre-scripted and aligned to the syllabus... there's not a whole lot of autonomy in U-Jets. It's a script."* — Navy IP

---

### Writing Requirements in JTBD Format

All requirements should use the Jobs to Be Done format:

```
**When** [situation/trigger]
**I want to** [action/capability]
**So that** [measurable outcome]
```

**Organize requirements by functional category:**

| Category | Examples |
|----------|----------|
| **Entity Placement** | Place threat aircraft, SAMs, ground targets |
| **Entity Behaviors** | Define flight paths, engagement logic, reactions |
| **Templates & Scenarios** | Save, load, share scenario configurations |
| **Geo-Reference** | Working areas, bullseyes, IPs, boundaries |
| **Export & Integration** | Data formats, aircraft upload, CARBON sync |

**Example JTBD Requirements:**

#### Job: Load Syllabus-Aligned Scenario Template (P0)
**When** I'm assigned a training sortie from the syllabus (e.g., "BFM Sortie 3.2")
**I want to** select a pre-built scenario that matches my sortie number
**So that** the scenario spawns immediately without manual configuration

**Evidence:**
> "Sortie 1.1 boom and next thing you know Mig-29 spawns next to me a mile abeam and I can get off and start fighting." — Tim Fitzpatrick, Navy IP

**Acceptance Criteria:**
- [ ] Template library organized by syllabus/training stage
- [ ] One-click scenario loading
- [ ] Scenario ready for export in <30 seconds of selection

---

#### Job: Place SAMs with Threat Rings (P0)
**When** I'm building a SEAD/DEAD or penetration scenario
**I want to** place SAM systems with automatic WEZ/MEZ ring visualization
**So that** pilots can see threat envelopes and plan ingress routes

**Evidence:**
> "We're plotting out your SAM sites... we put rings around them to know the threat area radius and potentially 3 dimensions." — Tim Fitzpatrick

**Acceptance Criteria:**
- [ ] SAM library includes SA-2, SA-6, SA-10, SA-20 (minimum)
- [ ] WEZ rings auto-calculate based on system type
- [ ] Rings display in 2D map view and export to aircraft

---

### Writing Acceptance Criteria (For Engineers & Designers)

Acceptance criteria are the contract between PM, engineering, and design. Great criteria answer: "How do we know when this is done?" Write them so an engineer can implement and a designer can validate without asking clarifying questions.

**What Engineers Need:**
- **Specific boundaries** - Numbers, not adjectives ("< 3 seconds" not "fast")
- **Edge cases called out** - What happens when X fails? When list is empty?
- **Data requirements** - What inputs? What formats? What's required vs. optional?
- **System interactions** - What triggers this? What does it affect downstream?
- **Testable conditions** - Can write an automated test against this criterion

**What Designers Need:**
- **User context** - What state is the user in when they see this?
- **Visual requirements** - What must be visible? What's the hierarchy?
- **Interaction model** - Click, drag, hover? Single or multi-select?
- **Feedback requirements** - Loading states, success, error, empty states
- **Accessibility** - Screen reader support, keyboard navigation, color contrast

**Two Formats:**

**Format 1: Rule-Based (Best for discrete behaviors)**
```
- [ ] SAM placement requires: type (dropdown), position (lat/long or map click)
- [ ] WEZ ring radius auto-populates from threat library (editable)
- [ ] Ring opacity: 30% fill, 100% border
- [ ] Rings update in <200ms when SAM position changes
- [ ] Maximum 20 SAMs per scenario (show warning at 15)
```

**Format 2: Scenario-Based (Best for workflows)**
```
GIVEN I have an unsaved scenario with 5 entities
WHEN I click "Save Template"
THEN I see a modal with: name field (required), description (optional), visibility toggle
AND name field is auto-focused
AND I cannot save until name is 3+ characters
AND saving shows spinner, then success toast, then returns to editor
```

**❌ Weak acceptance criteria (Engineers will ask questions):**
- "System should be fast" → How fast? Under what conditions?
- "Show appropriate error" → What error? Where? What can user do?
- "User can edit" → Edit what? How? What's the save model?

**✅ Strong acceptance criteria (Engineers can build):**
- "Map pan/zoom responds in <100ms at 60fps on iPad Pro"
- "Invalid coordinates show inline error: 'Latitude must be between -90 and 90'"
- "Edits auto-save after 2 seconds of inactivity; show 'Saving...' then 'Saved' indicator"

**✅ Strong acceptance criteria (Designers can validate):**
- "Threat rings use platform-specific color palette: red (hostile), yellow (unknown), green (friendly)"
- "Selected entity shows: highlight border (2px), resize handles (corners), delete button (top-right)"
- "Empty template library shows illustration + 'No templates yet' + 'Create Template' CTA"

**Platform-Specific Criteria:**
When a requirement behaves differently across platforms, call it out:

```
- [ ] T-38: Export includes bullseye reference point
- [ ] F-16: Export includes Link-16 network ID
- [ ] MC-130J: Export includes terrain masking data
- [ ] All platforms: Export completes in <5 seconds
```

---

### Writing Non-Goals (ATARS Context)

**Rules:**
- Include things stakeholders might reasonably expect
- Be specific enough to prevent scope creep
- Explain *why* it's deferred, connecting to "Thin to Win"
- Non-goals often become V2 goals

**❌ Weak non-goals:**
- "We won't build a flight simulator" (obviously out of scope)
- "Advanced features" (too vague)

**✅ Strong non-goals:**
- "Classified environments - V1 is unclassified only. CUI/classified requires additional security infrastructure; we'll evaluate based on customer demand post-launch."
- "Real-time in-flight editing - V1 is ground-based planning only. In-flight adjustments require CARBON integration and add certification complexity."
- "Multi-ship sync - V1 focuses on single-aircraft scenarios. Multi-ship coordination via CARBON is roadmapped for V2 pending Project Juice milestones."

---

### Writing Assumptions & Risks (ATARS Context)

**Risk Categories for ATARS:**
- **Technical:** Can ATARS support this? Will it affect certification?
- **Adoption:** Will IPs use it vs. current workflows?
- **Platform:** Different aircraft have different requirements
- **Certification:** Could this affect PFR or airworthiness?
- **Timeline:** What makes the milestone slip?

**❌ Weak risk:** "Things might not work."

**✅ Strong risk:** "Platform variation - T-38 and F-16 have different data export formats. Mitigation: Build abstraction layer; validate export format with each aircraft integration team before development."

---

## PRD Template Structure (ATARS)

```markdown
# [Feature Name] - Product Requirements Document

**Product:** [ATARS Planning / Big Red / ATARS 2.0]
**Version:** 1.0
**Date:** [Today's date]
**Author:** [Author name]
**Status:** Draft

---

## TL;DR

[3-5 bullet points. Include: the problem, the solution approach, primary success metric, target milestone, and which value pillar it supports.]

---

## 1. Problem Statement

### The Problem
[2-3 sentences. Quantify the pain in operational terms.]

### How It's Solved Today
[Current workarounds - manual processes, contractor dependency, etc.]

### Why This Matters Now
[Connection to Project Juice, customer commitment, or accumulated demand]

### Evidence
[User interviews, field observations, customer requests. Include quotes.]

---

## 2. Goals & Success Metrics

### Primary Goal
[One sentence. What does winning look like for IPs/MPs?]

### Success Metrics
| Metric | Current | Target | Timeframe |
|--------|---------|--------|-----------|
| Self-service rate | [Baseline] | >90% | 60 days post-launch |
| Time to scenario | [Baseline] | <15 min | At launch |
| [Leading indicator] | [Baseline] | [Target] | [When] |

### Kill Criteria
[What signals would tell us to stop? E.g., "<30% adoption after 90 days"]

---

## 3. Target User

### Primary Persona: [IP / MP / EXCON]
**Platform:** [T-38 / F-16 / MC-130J / etc.]
**Command:** [AETC / ACC / AFSOC]
**Context:** [Daily workflow, time constraints, current tools]

**JTBD:** "When [situation], I want to [action], so that [outcome]."

**Quote:** *"[Real quote from user research]"*

### Secondary Personas
| Persona | V1 Consideration |
|---------|------------------|
| [User type] | [How V1 addresses or defers their needs] |

---

## 4. Solution Overview

### Approach
[High-level direction. What are we building? Not a feature list - the concept.]

### Design Principles
| Principle | Implication |
|-----------|-------------|
| "Thin to Win" | Ship MVP, iterate based on field feedback |
| "Spawn and Go" | Minimize clicks from launch to scenario-ready |
| [Principle] | [Implication] |

---

## 5. Requirements

### Priority Definitions
| Priority | Meaning |
|----------|---------|
| **P0** | Must have - blocks launch |
| **P1** | Should have - launch is weaker without it |
| **P2** | Nice to have - adds value, not critical |

### [Category: Entity Placement]

#### Job: [Requirement Title] (P0/P1/P2)
**When** [situation/trigger]
**I want to** [action/capability]
**So that** [measurable outcome]

**Evidence:**
> "[Quote from user research]" — [Source]

**Acceptance Criteria:**
- [ ] [Specific, testable, measurable]
- [ ] [Specific, testable, measurable]

**Platform Notes:** [Any platform-specific considerations]

---

## 6. User Flows

> High-level flows only. Design owns detailed interactions.

### Flow: [Name]
```
1. IP [trigger] → [Screen]
2. IP [action] → [System response]
3. [Decision point] →
   ├─ Yes: [Next step]
   └─ No: [Alternative]
4. [End state - scenario exported/ready]
```

---

## 7. Non-Goals

**Explicitly out of scope for V1 ("Thin to Win"):**

| Item | Why Deferred |
|------|--------------|
| Classified environments | Requires additional security infrastructure |
| [Feature] | [Specific rationale] |

---

## 8. Assumptions & Risks

### Assumptions
| Assumption | If Wrong | Validation |
|------------|----------|------------|
| IPs prefer templates over daily customization | Build flexible editor instead | Validate with 3+ IP interviews |

### Risks
| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Platform variation in data formats | Medium | High | Build abstraction layer |

---

## 9. Dependencies

| Dependency | Owner | Status | If Delayed |
|------------|-------|--------|------------|
| CARBON integration | Engineering | In progress | Multi-ship deferred to V2 |

---

## 10. Open Questions

| Question | Owner | Decide By |
|----------|-------|-----------|
| [Unresolved item] | [Who] | [Date] |

---

## Appendix

### A. Platform-Specific Notes
| Aircraft | Consideration |
|----------|---------------|
| T-38 | [Notes] |
| F-16 | [Notes] |

### B. ATARS Glossary
[Include relevant terms from the domain glossary]

### C. References
- [User research / Interview Snapshots]
- [ConOps documents]
- [Prior PRDs]

### D. Revision History
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
        description: "Make the operational problem more specific"
      - label: "Clarify success metrics"
        description: "Make metrics more specific or add targets"
      - label: "Add requirements"
        description: "Document additional JTBD or acceptance criteria"
      - label: "Adjust priorities"
        description: "Change P0/P1/P2 assignments"
      - label: "Add platform considerations"
        description: "More detail on T-38/F-16/MC-130J differences"
      - label: "Looks good"
        description: "Ready for stakeholder review"
```

3. Apply any requested changes
4. Repeat until user is satisfied

---

## Output Location

Save the PRD to the user's specified location, or suggest:
- `[Feature Name] - PRD V1.md` in the current working directory
- Or in the relevant product folder (e.g., `/ATARS Planning/`)

---

## Quality Checklist

Before finalizing any PRD, verify both content and writing quality.

### ATARS-Specific Checks
- [ ] Problem is quantified in operational terms (sorties, flight hours, setup time)
- [ ] Requirements use JTBD format (When/I want to/So that)
- [ ] Evidence includes quotes from user research
- [ ] Platform considerations documented where relevant (2026 focus: T-38, T-45, F-16, MC-130J)
- [ ] User types specified (IP, MP, Student, FTU Pilot, Operational Pilot, EXCON)
- [ ] "Thin to Win" applied - V1 scope is minimal viable
- [ ] Success metrics appropriate to the product (not just Planning metrics)
- [ ] Non-goals explain why deferred (not just listed)
- [ ] Value pillar connection is clear (Production/Absorption/Retention)

### Content Completeness
- [ ] Problem is quantified with specific numbers
- [ ] Evidence proves this is real (not assumed)
- [ ] Success metrics have baseline → target → timeframe
- [ ] Kill criteria are defined
- [ ] Non-goals include things stakeholders might expect
- [ ] Risks have mitigation plans
- [ ] Open questions have owners and deadlines

### Acceptance Criteria Quality (For Engineers & Designers)
- [ ] Every criterion has specific boundaries (numbers, not adjectives)
- [ ] Edge cases called out (empty states, errors, max limits)
- [ ] Data requirements specified (inputs, formats, required vs. optional)
- [ ] Visual requirements clear (what's visible, hierarchy, feedback states)
- [ ] Interaction model defined (click, drag, hover, keyboard)
- [ ] Platform-specific behaviors noted where different
- [ ] An engineer could write a test against each criterion
- [ ] A designer could create mockups without asking questions

### Writing Quality
- [ ] TL;DR is ≤5 bullets and captures the essence
- [ ] Problem statement is ≤4 sentences
- [ ] No solutions appear in the problem section
- [ ] User personas use specific roles (IP, MP, FTU Pilot, etc.)
- [ ] Every metric is measurable
- [ ] No filler words ("very," "really," "quite," "basically")
- [ ] No vague criteria ("fast," "appropriate," "good")

### Alignment Test
Read the PRD and ask:
- [ ] Could an engineer start building from this tomorrow?
- [ ] Could a designer start wireframing from this tomorrow?
- [ ] Would two engineers reading this build the same thing?
- [ ] If requirements conflict, does the PRD help prioritize?
- [ ] Would a new team member understand the context?

---

## Framework References

This skill incorporates best practices from:

- **Marty Cagan (SVPG):** Focus on problems, not solutions. Let engineering solve creatively.
- **Teresa Torres:** Continuous discovery, opportunity solution trees, user evidence.
- **Jobs to be Done:** Frame requirements around the job users hire the product to do.
- **Amazon Working Backwards:** Start with the customer benefit before the feature.
- **Red 6 "Thin to Win":** Build minimum viable, expand based on real feedback.

### Key Quotes

> "For U-Jets, I'd want it all pre-scripted and aligned to the syllabus and then defaults would be by exception. There's not a whole lot of autonomy or thought in U-Jets. It's a script." — Tim Fitzpatrick, Navy IP

> "You trust the system because the tolerance allows you to fly that mission without question." — Tim Fitzpatrick

> "The first thing you think of is where in the world are you actually operating... how does ATARS relate to the geo-reference of the world." — Tim Fitzpatrick

---

## Customization

This skill is optimized for ATARS products. For specific products:

**For ATARS Planning:**
- Focus on ground-based scenario creation workflow
- Emphasize template-first design for training commands
- Consider syllabus alignment requirements
- Key metrics: self-service rate, time to scenario, template usage

**For ATARS Debrief:**
- Focus on post-flight analysis workflow
- Emphasize reconstruction accuracy and insight speed
- Consider IP vs. student views
- Key metrics: debrief completion rate, time to first insight

**For ATARS 1.2 (Daytime):**
- Focus on daytime flight reference requirements
- Emphasize glare handling, visibility, outdoor usability
- Consider certification pathway implications
- Key metrics: daytime usability score, pilot trust rating, certification milestones

**For ATARS 2.0:**
- Add certification/airworthiness considerations (DO-178C, DO-254)
- Include symbology standards (MIL-STD-1787D)
- Document PFR integration requirements
- Consider both training and operational use cases
- Key metrics: system availability, tracking accuracy, time to operational
