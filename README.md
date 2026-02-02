# Write PRD - ATARS Product Requirements Generator for Claude Code

A Claude Code skill that generates comprehensive Product Requirements Documents for ATARS and Red 6 products through a structured interview process. Optimized for military aviation training context with JTBD-format requirements.

**Domain:** ATARS, military aviation training, Red 6 products
**Output:** PRDs with Jobs to Be Done format requirements

## Philosophy

> **"Thin to Win"** - Build the minimum viable feature set, expand based on real feedback. Avoid over-engineering for hypothetical use cases.

> **Problem First, Solution Second** - The best PRDs obsess over the problem before prescribing solutions. IPs and MPs should have room to solve problems creatively.

> **Every Word Earns Its Place** - A strong PRD captures only what is necessary. Cut ruthlessly. If a sentence doesn't help someone build or decide, delete it.

Instead of staring at a blank page, this skill guides you through targeted questions about operational context, then generates a well-structured PRD with JTBD-format requirements.

## ATARS Value Pillars

All PRDs should connect to one or more of these core outcomes:

| Pillar | Description |
|--------|-------------|
| **Pilot Production** | Increase throughput of trained pilots through the pipeline |
| **Absorption** | Help squadrons absorb more training sorties with existing resources |
| **Retention** | Improve pilot retention through better, more engaging training |

## What It Does

1. **Gathers Context** - Reads ConOps, discovery notes, prior PRDs
2. **Runs 5 Rounds of Discovery Questions:**
   - **Problem Validation** - Is this a real operational problem? How do IPs/MPs solve it today?
   - **User & Context** - IP, MP, or EXCON? What platform? What triggers the need?
   - **Strategic Fit** - Why now? Project Juice? Which value pillar?
   - **Success & Trade-offs** - Self-service rate? Time to scenario? What are we NOT doing?
   - **Risks & Requirements** - Platform variation? Certification impact? What could block us?
3. **Generates a Structured PRD** with JTBD-format requirements and platform considerations
4. **Refines Based on Feedback**

## Key Products

| Product | Description |
|---------|-------------|
| **ATARS 2.0** | Next-gen certified PFR headset for training + operations |
| **ATARS Planning** | Ground-based scenario creation tool |
| **ATARS Debrief** | Post-flight analysis and training review |
| **ATARS 1.2** | Daytime training and daytime flight reference |

## Target User Types

| User Type | Description |
|-----------|-------------|
| **IP (Instructor Pilot)** | Sets up and runs training scenarios; time-constrained |
| **Mission Planner (MP)** | Builds complex multi-phase scenarios; more planning time |
| **Student Pilot** | Undergraduate pilot training (UPT/U-Jets); follows syllabus |
| **FTU Pilot** | Already trained pilot at Formal Training Unit; learning new airframe |
| **Operational Pilot** | Career pilot doing mission readiness or remedial training |
| **EXCON (Exercise Controller)** | Manages large force exercises |

## Platforms (2026 Focus)

| Platform | Command | Notes |
|----------|---------|-------|
| **T-38** | AETC | Primary training; BFM, tactical intercept |
| **T-45** | Navy | U-Jets; syllabus-aligned scenarios |
| **F-16** | ACC | Legacy fighter; ATARS 2.0 certification |
| **MC-130J** | AFSOC | Special operations; penetration |
| **T-7** | Future | Next-gen trainer |

*Platform focus expands annually. PRDs should specify which platforms are in scope.*

## Requirements Format (JTBD)

All requirements use the Jobs to Be Done format:

```markdown
#### Job: [Requirement Title] (P0/P1/P2)
**When** [situation/trigger]
**I want to** [action/capability]
**So that** [measurable outcome]

**Evidence:**
> "[Quote from user research]" — [Source]

**Acceptance Criteria:**
- [ ] [Specific, testable, measurable]
```

### Example JTBD Requirement

```markdown
#### Job: Load Syllabus-Aligned Scenario Template (P0)
**When** I'm assigned a training sortie from the syllabus (e.g., "BFM Sortie 3.2")
**I want to** select a pre-built scenario that matches my sortie number
**So that** the scenario spawns immediately without manual configuration

**Evidence:**
> "Sortie 1.1 boom and next thing you know Mig-29 spawns next to me a mile abeam
> and I can get off and start fighting." — Tim Fitzpatrick, Navy IP

**Acceptance Criteria:**
- [ ] Template library organized by syllabus/training stage
- [ ] One-click scenario loading
- [ ] Scenario ready for export in <30 seconds of selection
```

## PRD Structure

| Section | Purpose |
|---------|---------|
| 1. Problem Statement | Operational pain, current solutions, why now, evidence with quotes |
| 2. Goals & Success Metrics | Self-service rate, time to scenario, kill criteria |
| 3. Target User | IP/MP/EXCON, platform, command, JTBD, quote |
| 4. Solution Overview | High-level approach, design principles ("Thin to Win") |
| 5. Requirements | JTBD-format requirements organized by category |
| 6. User Flows | High-level operational workflows |
| 7. Non-Goals | What we're NOT doing for V1 (with rationale) |
| 8. Assumptions & Risks | Platform variation, certification, adoption risks |
| 9. Dependencies | CARBON, platform teams, etc. |
| 10. Open Questions | Unresolved items with owners |

## Key Questions This Skill Asks

### Problem Validation
- How do IPs/MPs solve this problem today?
- How do we know this is a real problem (not an assumed one)?

### User Understanding
- Who is the primary user for V1? (IP, MP, EXCON)
- What triggers the need for this capability?

### Strategic Fit
- Why build this now? (Project Juice, customer commitment, etc.)
- Which ATARS value pillar does this support?

### Success Definition
- What does success look like? (Self-service rate, time to scenario)
- What would make us kill this project?

### Scope & Trade-offs
- What are we explicitly NOT doing in V1?
- What's the smallest version that delivers value?

### Risk Assessment
- What's the hardest part? (Platform variation, certification, adoption)
- What assumptions are we making?

## Success Metrics by Product

Choose metrics appropriate to the product:

| Product | Example Metrics |
|---------|-----------------|
| **ATARS Planning** | Self-service rate, time to scenario, template usage |
| **ATARS Debrief** | Debrief completion rate, time to first insight, reconstruction accuracy |
| **ATARS 2.0** | System availability, tracking accuracy, pilot trust rating |
| **ATARS 1.2** | Daytime usability score, visibility issues, certification milestones |

## Writing Acceptance Criteria (For Engineers & Designers)

Acceptance criteria are the contract between PM, engineering, and design. Write them so an engineer can implement and a designer can validate without asking questions.

**What Engineers Need:**
- Specific boundaries (numbers, not adjectives)
- Edge cases called out
- Data requirements (inputs, formats)
- Testable conditions

**What Designers Need:**
- User context and states
- Visual requirements and hierarchy
- Interaction model (click, drag, hover)
- Feedback states (loading, success, error, empty)

**Example:**
```
- [ ] SAM placement requires: type (dropdown), position (lat/long or map click)
- [ ] WEZ ring radius auto-populates from threat library (editable)
- [ ] Rings update in <200ms when position changes
- [ ] Maximum 20 SAMs per scenario (show warning at 15)
```

## Installation

### Option 1: Clone to Claude Skills Directory

```bash
git clone https://github.com/Derek-ec/My-R6-PRD-skill.git ~/.claude/skills/write-prd
```

### Option 2: Manual Installation

1. Create the skills directory:
   ```bash
   mkdir -p ~/.claude/skills/write-prd
   ```

2. Copy `SKILL.md` to that directory:
   ```bash
   cp SKILL.md ~/.claude/skills/write-prd/
   ```

3. Restart Claude Code

## Usage

### Invoke Directly

```
/write-prd
```

### Natural Language

The skill activates automatically when you say:
- "Help me write a PRD"
- "Create a PRD for [ATARS feature]"
- "I need product requirements for ATARS Planning"

### With Context Files

For best results, provide existing documents:

```
/write-prd

[Then provide paths to:]
- ConOps documents
- Discovery interview notes
- User research synthesis
- Prior PRDs
```

## Example Session

```
User: /write-prd

Claude: I'll help you create a PRD for an ATARS feature.
        Do you have any existing documents I should review first?

User: Yes, read the files in ~/ATARS Planning/Context/

Claude: [Reads files, summarizes context]

        Now I'll ask questions to understand the operational problem.

        [Round 1: Problem Validation]
        How do IPs/MPs solve this problem today?

        1. No solution exists
        2. Manual workarounds (paper, whiteboards)
        3. TACTS/ACMI/Legacy tools
        4. Contractor support
        5. Let me explain

User: 4 - They rely on Red 6 contractors

Claude: How do we know this is a real problem?

        1. Pilot/IP interviews
        2. Customer request (AETC/ACC)
        3. Observation during flights
        4. Support/field issues
        5. Assumption - needs validation

User: 1, 2

Claude: [Continues through 5 rounds...]

        [Generates PRD with JTBD requirements]

        What would you like me to refine?

        1. Sharpen problem statement
        2. Clarify success metrics
        3. Add requirements
        4. Adjust priorities
        5. Add platform considerations
        6. Looks good

User: 6

Claude: PRD saved to ATARS Planning - Feature X PRD V1.md
```

## Quality Checklist

The skill verifies PRDs meet these standards:

### ATARS-Specific Checks
- [ ] Problem quantified in operational terms (sorties, setup time)
- [ ] Requirements use JTBD format (When/I want to/So that)
- [ ] Evidence includes quotes from user research
- [ ] User types specified (IP, MP, Student, FTU Pilot, Operational Pilot, EXCON)
- [ ] Platform considerations documented (2026: T-38, T-45, F-16, MC-130J)
- [ ] "Thin to Win" applied - V1 scope is minimal viable
- [ ] Success metrics appropriate to the product
- [ ] Value pillar connection is clear

### Acceptance Criteria Quality
- [ ] Every criterion has specific boundaries (numbers, not adjectives)
- [ ] Edge cases called out (empty states, errors, max limits)
- [ ] An engineer could write a test against each criterion
- [ ] A designer could create mockups without asking questions

### Writing Quality
- [ ] TL;DR captures the essence in 5 bullets
- [ ] Problem statement is 4 sentences max
- [ ] User personas use specific roles
- [ ] No vague criteria ("fast," "appropriate," "good")

## Framework References

This skill incorporates best practices from:

- **Marty Cagan (SVPG)** - Focus on problems, not solutions
- **Teresa Torres** - Continuous discovery, user evidence
- **Jobs to be Done** - Frame requirements around user jobs
- **Amazon Working Backwards** - Customer benefit before features
- **Red 6 "Thin to Win"** - Build minimum viable, expand based on feedback

### Key Quotes

> "For U-Jets, I'd want it all pre-scripted and aligned to the syllabus and then defaults would be by exception. There's not a whole lot of autonomy or thought in U-Jets. It's a script." — Tim Fitzpatrick, Navy IP

> "You trust the system because the tolerance allows you to fly that mission without question." — Tim Fitzpatrick

## Customization

This skill is optimized for ATARS. For specific products:

**For ATARS Planning:**
- Focus on ground-based scenario creation
- Emphasize template-first design
- Key metrics: self-service rate, time to scenario

**For ATARS Debrief:**
- Focus on post-flight analysis workflow
- Key metrics: debrief completion rate, time to first insight

**For ATARS 1.2 (Daytime):**
- Focus on daytime flight reference
- Key metrics: usability score, certification milestones

**For ATARS 2.0:**
- Add certification considerations (DO-178C, DO-254)
- Key metrics: system availability, tracking accuracy

## Requirements

- [Claude Code](https://claude.ai/code) CLI
- Claude Code skills support

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

MIT License - See [LICENSE](LICENSE) file.

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 4.1.0 | 2026-02-02 | Refined products (Planning, Debrief, 1.2, 2.0), expanded user types (FTU, Operational Pilot), product-specific metrics, enhanced acceptance criteria guidance |
| 4.0.0 | 2026-02-02 | ATARS-specific with JTBD format, value pillars, platform context |
| 3.0.0 | 2026-01-XX | Added section-by-section writing guide, 7 principles |
| 2.0.0 | 2026-01-XX | Interview-driven approach, 5-round discovery |
| 1.0.0 | 2026-01-XX | Initial release |
