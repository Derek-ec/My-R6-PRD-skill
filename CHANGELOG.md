# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [4.1.0] - 2026-02-02

### Refined - Products, Users, Metrics, and Acceptance Criteria

**Corrected Key Products:**
- ATARS 2.0 (certified PFR headset)
- ATARS Planning (scenario creation)
- ATARS Debrief (post-flight analysis)
- ATARS 1.2 (daytime training and flight reference)

**Expanded Target User Types:**
- Added FTU Pilot (already trained pilot at Formal Training Unit)
- Added Operational Pilot (career pilot doing mission readiness or remedial training)
- Clarified Student Pilot context (UPT/U-Jets, follows syllabus)

**Platforms Clarified:**
- Noted that listed platforms (T-38, T-45, F-16, MC-130J, T-7) are 2026 focus
- PRDs should specify which platforms are in scope

**Product-Specific Metrics:**
- ATARS Planning: self-service rate, time to scenario, template usage
- ATARS Debrief: debrief completion rate, time to first insight, reconstruction accuracy
- ATARS 2.0: system availability, tracking accuracy, pilot trust rating
- ATARS 1.2: daytime usability score, visibility issues, certification milestones

**Enhanced Acceptance Criteria Guidance:**
Added comprehensive section on writing acceptance criteria that engineers and designers want to read:
- What engineers need: specific boundaries, edge cases, data requirements, testable conditions
- What designers need: user context, visual requirements, interaction model, feedback states
- Two formats: rule-based and scenario-based (Given/When/Then)
- Good vs. bad examples
- Platform-specific criteria guidance

**Updated Quality Checklist:**
- Added "Acceptance Criteria Quality" section
- Checks for specific boundaries, edge cases, testability
- Verifies designers can create mockups without questions

---

## [4.0.0] - 2026-02-02

### Added - ATARS-Specific Domain Context

Transformed the skill from a generic PRD generator to an ATARS-specific tool optimized for Red 6 military aviation training products.

**New Domain Context Section:**
- About Red 6 and ATARS products (ATARS 2.0, ATARS Planning, Big Red, RedTrace)
- Value Proposition: The Three Pillars (Pilot Production, Absorption, Retention)
- Target User Types (IP, Mission Planner, EXCON, Student Pilot)
- Platform coverage (T-38, T-45, F-16, MC-130J, T-7)
- ATARS Glossary (PFR, BFM, ACM, SEAD/DEAD, SAM, MEZ/WEZ, etc.)

**New Philosophy:**
- "Thin to Win" - Build minimum viable, expand based on real feedback

**ATARS-Specific Interview Questions:**
- Round 1: How do IPs/MPs solve this today? (Manual workarounds, TACTS/ACMI, Contractor support)
- Round 2: Primary user (IP, MP, EXCON)? What platform?
- Round 3: Why now? (Project Juice, customer commitment) Which value pillar?
- Round 4: Success metrics (self-service rate, time to scenario)
- Round 5: Risks (platform variation, certification impact)

**Requirements in JTBD Format:**
All requirements now use Jobs to Be Done format:
```
**When** [situation/trigger]
**I want to** [action/capability]
**So that** [measurable outcome]
```

**JTBD Categories for ATARS:**
- Entity Placement (threat aircraft, SAMs, ground targets)
- Entity Behaviors (flight paths, engagement logic)
- Templates & Scenarios (save, load, share)
- Geo-Reference (working areas, bullseyes, IPs)
- Export & Integration (data formats, CARBON sync)

**Example JTBD Requirements:**
- "Load Syllabus-Aligned Scenario Template" with Tim Fitzpatrick quotes
- "Place SAMs with Threat Rings" with evidence from user research

**Updated PRD Template:**
- Product field includes ATARS products
- Target User section includes Platform and Command
- Requirements organized by JTBD categories
- Design Principles include "Thin to Win" and "Spawn and Go"
- Non-Goals framed with "Thin to Win" rationale
- Platform-Specific Notes in Appendix

**ATARS-Specific Success Metrics:**
- Self-service rate (% without contractor support)
- Time to scenario (launch to export-ready)
- Template usage rate
- Scenario complexity
- Sortie throughput

**Updated Quality Checklist:**
- Problem quantified in operational terms (sorties, flight hours, setup time)
- Requirements use JTBD format
- Evidence includes quotes from user research
- Platform considerations documented
- "Thin to Win" applied
- Value pillar connection clear

**Key Quotes from User Research:**
- Tim Fitzpatrick (Navy IP) on templates, spawning, geo-reference, system trust

**Product-Specific Customization Guidance:**
- ATARS Planning (ground-based, template-first, syllabus alignment)
- Big Red (natural language, voice interface)
- ATARS 2.0 (certification, symbology standards)

---

## [3.0.0] - 2026-02-01

### Added - Section-by-Section Writing Guide

Deep research into how top PMs write each PRD section, with specific guidance for powerful, concise writing.

**New Philosophy:**
- "Every Word Earns Its Place" - Cut ruthlessly. If it doesn't help build or decide, delete it.
- "Quantify Everything" - Replace vague problems with specific numbers.

**7 Universal Principles** (from Lenny Rachitsky, Figma, Stripe, Amazon):
1. Make it scannable - Add TL;DRs, use formatting
2. Tailor the format - Not every project needs 10 sections
3. Problem over solution - The problem shouldn't change
4. Write it with your team - Include eng/design early
5. Use it for alignment - Stress test in reviews
6. Revisit and update - Keep the spec fresh
7. Don't skip the postmortem - Learn for next time

**Section Writing Guides with Good/Bad Examples:**

1. **Problem Statements**
   - Formula: User segment + problem + impact + evidence
   - Rule: 3-4 sentences max, quantify the pain
   - Bad: "Users don't like checkout" → Good: "40% abandon at checkout, costing $2M/month"

2. **Success Metrics**
   - Rule: Every metric needs baseline → target → timeframe
   - Include leading indicators (early signals) and lagging indicators (outcomes)
   - Bad: "Good experience" → Good: "Page load < 2 seconds for 95th percentile"

3. **Target Users**
   - Rule: Never use "users" - use specific roles
   - Include representative quotes capturing frustration
   - Keep to 1-2 paragraphs per persona

4. **Requirements & Acceptance Criteria**
   - INVEST criteria: Independent, Negotiable, Valuable, Estimable, Small, Testable
   - Two formats: Rule-based (checklist) or Scenario-based (Given/When/Then)
   - Bad: "System performs well" → Good: "1,000 txn/sec at p99 < 200ms"

5. **Non-Goals**
   - Include things stakeholders might reasonably expect
   - Explain WHY deferred, not just WHAT
   - Bad: "Advanced features" → Good: "Mobile app - desktop-only for V1. Mobile is 12% of traffic."

6. **Assumptions & Risks**
   - Categories: Technical, Adoption, Dependencies, Timeline
   - Every risk needs a mitigation plan

7. **User Flows**
   - High-level only (5-10 steps max)
   - Include decision points and error states
   - Design owns detailed interactions

**New Template Features:**
- TL;DR section at top (3-5 bullets for busy stakeholders)
- Kill Criteria in Success Metrics
- Design Principles section with implications
- Improved formatting for scannability

**Enhanced Quality Checklist:**
- Content completeness checks
- Writing quality checks (no filler words, testable criteria)
- Alignment test (could eng/design start tomorrow?)
- "So What?" test for every section

**Additional Framework References:**
- Yuhki Yamashita (Figma) - Three questions framework
- Amazon Working Backwards - Press release first
- Stripe - Engineers as owners, alternatives considered
- Lenny Rachitsky - 7 principles
- Quotes from Sarah Tavel, Ravi Mehta, Paige Costello

---

## [2.0.0] - 2026-02-01

### Changed - Major Rewrite Based on PM Best Practices Research

Completely rewrote interview questions based on research into how top product managers (Marty Cagan, Shreyas Doshi, Teresa Torres) approach PRDs.

**New Philosophy:**
- "Problem First, Solution Second" - obsess over the problem before prescribing solutions
- Focus on validating that problems are real, not assumed
- Leave room for engineering creativity

**New Interview Structure (5 Rounds):**

1. **Problem Validation** - Is this a real problem worth solving?
   - How do users solve this today?
   - How do we know this is a real problem (not assumed)?

2. **User & Context** - Deep understanding of who we're building for
   - Who feels this pain most acutely?
   - What triggers the user to need this? (Jobs to be Done framing)

3. **Strategic Fit & Timing** - Is this the right thing to build now?
   - Why build this now? What's the urgency?
   - Is this a "painkiller" or "vitamin"?

4. **Success & Trade-offs** - What does winning look like?
   - What does success look like? (Primary metric)
   - What are we explicitly NOT doing? (Non-goals)

5. **Risks & Requirements** - What could go wrong?
   - What's the hardest or riskiest part?
   - What level of detail does this PRD need?

**New PRD Template Sections:**
- Added "How Users Solve This Today" to Problem Statement
- Added "Evidence" section (how do we know this is real?)
- Added "What Would Make Us Kill This Project?" (failure criteria)
- Added "Job to be Done" format for user context
- Added "Assumptions & Risks" with validation plans
- Added "Key Questions Checklist" for PRD review

**Framework References Added:**
- Marty Cagan (SVPG)
- Teresa Torres (Continuous Discovery)
- Shreyas Doshi (high-agency thinking)
- Jobs to be Done (Christensen, Ulwick, Moesta)
- Aha.io 10-Step Process

---

## [1.1.0] - 2026-02-01

### Changed
- Made interview questions more generic and universally applicable
- Removed domain-specific questions (AI scope, security classification, deployment model)

---

## [1.0.0] - 2026-01-30

### Added
- Initial release of write-prd skill
- 5-round structured interview process
- 10-section PRD template
- Context file reading support
- Iterative refinement workflow
- Example PRD output

### Interview Rounds
1. Core problem & primary user persona
2. AI/automation scope & UI direction
3. Success metrics & out-of-scope items
4. Deployment model & security constraints
5. Key UI/UX flows & detail level

### PRD Sections
1. Problem Statement
2. Solution Overview
3. Target Users
4. Success Metrics
5. Jobs to Be Done (Requirements)
6. Core UI/UX Flows
7. Platform Considerations
8. Out of Scope for V1
9. Dependencies & Assumptions
10. Open Questions
