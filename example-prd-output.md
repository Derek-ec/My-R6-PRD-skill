# ATARS Planning V1 - Product Requirements Document

**Version:** 1.0
**Date:** 2026-01-30
**Author:** [Product Team]
**Status:** Draft
**Target Release:** May 1, 2026

---

## 1. Problem Statement

### The Challenge

ATARS is Red Six's in-flight AR training system that acts as a semi-embedded training system in military aircraft. ATARS enables pilots to train against synthetic threats, friendly aircraft, and SAM systems rendered in augmented reality during actual flight.

**However, there is currently no efficient way to create ATARS scenarios on the ground or in the air.**

Today's pain points:

1. **No dedicated planning tool exists.** The in-cockpit tablet webapp allows scenario execution and control, but pilots don't have time in the cockpit to input all parameters necessary to generate realistic simulations.

2. **JMPS is not designed for ATARS.** The Joint Mission Planning System (JMPS) that squadrons use for traditional mission planning cannot generate ATARS-specific scenarios with synthetic entities that have physics-based behaviors, tactics, and strategies, whether red air or blue air.

3. **Contractor dependency creates unacceptable delays.** Without a self-service tool, units must wait weeks or longer for contractor support to customize scenarios—time they don't have.

### The Impact

- Training scenarios will be suboptimal and inconsistent
- Valuable flight hours will be wasted on inadequate simulations
- Units will not be able to rapidly adapt training to emerging needs
- Pilots may resort to workarounds that limit ATARS capabilities

---

## 2. Solution Overview

### What We're Building

**ATARS Planning** is a ground-based application that enables Instructor Pilots and Mission Planners to create, customize, and transfer ATARS training scenarios in under 15 minutes—without contractor support.

### Core Philosophy

> **"Thin to Win"** — Start with minimal viable features, expand based on unit feedback. Users need a "leave-behind tool" where they can customize scenarios quickly, not wait for external support.

### ATARS Planning Tenets

| Tenet | Description |
|-----------|-------------|
| Self-Service | Any trained user can create scenarios without engineering support |
| Speed | Scenario creation in <15 minutes |
| Hybrid Interface | Map canvas + templates for maximum flexibility |
| Platform-Aware | Respects constraints of T-38, F-16, and MC-130J |
| Transfer-Ready | Scenarios export to aircraft systems |

### Deployment Model

Deployment model (desktop application vs. web application) is pending finalization of TO-4 and Project Juice contracts. The PRD is written to be deployment-agnostic.

### Security Classification

V1 targets **unclassified environments only**. Classified network support is a future consideration for Project Juice and beyond.

---

## 3. Target Users

### Primary Personas (Equal Priority)

#### Instructor Pilot (IP)
- **Role:** Sets up and runs training scenarios for student pilots
- **Context:** May plan scenarios hours or days before flight, or adapt quickly before a sortie
- **Key need:** Rapid scenario customization with templates
- **Success metric:** Can customize a template in <10 minutes

#### Mission Planner (MP)
- **Role:** Dedicated planner who builds scenarios for units
- **Context:** Plans multiple scenarios in advance, may support multiple flights/units
- **Key need:** Full control over entity placement, behaviors, and triggers
- **Success metric:** Can build complex multi-phase scenarios from scratch

### Secondary Personas (V1 Awareness)

| Persona | V1 Consideration |
|---------|------------------|
| Student Pilot | May use templates for self-study; primary training via IP-created scenarios |
| EXCON (Exercise Controller) | Multi-ship coordination needs addressed via networking features |

---

## 4. Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| **Self-Service Rate** | >90% | % of scenarios created without contractor/engineering support |
| **Time to Scenario** | <15 minutes | Average time from launch to export-ready scenario |
| **Template Usage** | >60% | % of scenarios starting from templates (indicates templates are useful) |

---

## 5. Jobs to Be Done (Requirements)

### Priority Definitions

| Priority | Definition |
|----------|------------|
| **P0** | Must have for MVP (May 1) |
| **P1** | Should have for MVP; critical for adoption |
| **P2** | Nice to have; can ship without |
| **Stretch** | Valuable but not committed |

---

### Category 1: Entity Placement

#### Job 1.1: Place Entities at Precise Locations (P0)
**When** I'm planning a mission rehearsal against a known target or threat location
**I want to** place entities using exact GPS coordinates (lat/long)
**So that** I can train against geographically accurate threat positions

**Acceptance Criteria:**
- Input field accepts lat/long coordinates
- Entities snap to specified location on map
- Coordinates display in standard military format

---

#### Job 1.2: Place Entities Relative to a Reference Point (P0)
**When** I don't have exact coordinates or need flexible positioning
**I want to** place entities using bearing, range, and altitude from a reference point
**So that** I can rapidly build scenarios without precise GPS data

**Acceptance Criteria:**
- Input bearing (degrees), range (NM), altitude (feet MSL/AGL)
- Reference point can be a defined location or bullseye
- Entity appears at calculated position on map

---

### Category 2: Entity Types

#### Job 2.1: Create Geometric Shapes for Training Boundaries (P0)
**When** I'm setting up training areas, drop zones, or threat envelopes
**I want to** place geometric entities (hemispheres, cubes, rectangles, cylinders)
**So that** pilots can visualize boundaries and restricted areas

**Entity Types:**
| Shape | Use Case |
|-------|----------|
| Hemisphere (Dome) | SAM threat rings based on missile performance |
| Cube | 3D block airspace |
| 2D Rectangle | Drop zones, ground target areas, airspace boundaries |
| Cylinder | Missile Engagement Zones (MEZ) |

> **Design Note:** ATARS Planning uses a top-down (god's eye) map view. Rendering true 3D shapes (hemispheres, cubes, cylinders) in this view presents design challenges. Design team should explore representation options (e.g., 2D projections with altitude annotations, contour rings, or simplified footprints). Final implementation approach TBD.

**Acceptance Criteria:**
- Each shape has configurable dimensions
- Shapes render with clear boundaries on map
- Color and transparency configurable
- Airspace boundaries support floor/ceiling altitude limits

---

#### Job 2.2: Place Constructive Threat Aircraft (P0)
**When** I'm building a Red Air scenario
**I want to** place enemy aircraft with defined type, behavior, and starting position
**So that** pilots can train against realistic adversary presentations

**Acceptance Criteria:**
- Select from threat library (MiG-29, Su-35, etc.)
- Define starting position, altitude, heading
- Assign behavior profile (see Category 3)

---

#### Job 2.3: Place Constructive Friendly Aircraft (P0)
**When** I'm building a scenario with blue air
**I want to** place friendly/allied aircraft
**So that** pilots can practice formation tactics (including single-ship formation flying, e.g., AETC) and blue-on-blue deconfliction

**Acceptance Criteria:**
- Select aircraft type from friendly library
- Define starting position, altitude, heading
- Assign behavior profile or designate as networked participant

---

#### Job 2.4: Place Surface-to-Air Threats (SAMs) (P0)
**When** I'm building a SEAD/DEAD or penetration scenario
**I want to** place SAM systems with doctrinal behavior
**So that** pilots can train against realistic integrated air defense systems

**Acceptance Criteria:**
- Select SAM type (SA-6, SA-10, etc.)
- System uses doctrinal engagement ranges by default
- Configurable: SAM type, missile types, engagement range, missile count

---

### Category 3: Entity Behaviors

#### Job 3.1: Define Pre-Planned Routes (P0)
**When** I need predictable, repeatable threat presentations
**I want to** define a flight path with waypoints and timing
**So that** I can create consistent training scenarios

**Complexity Levels:**
1. Straight line (A to B)
2. Turn sequences (waypoint-to-waypoint)
3. Timed events at specific times/locations

**Acceptance Criteria:**
- Draw route on map interface
- Set airspeed and altitude for each leg
- Preview route animation before export

---

#### Job 3.2: Assign AI Agent Behaviors (P2)
**When** I need dynamic, reactive threat presentations
**I want to** assign AI-driven behavior to entities
**So that** advanced pilots can train against unpredictable adversaries

**Behavior Types:**
- Autonomous intercept (reacts to blue position)
- Defensive reactions (responds to missile launch)
- Tactical formations (maintains element integrity)

**Acceptance Criteria:**
- Select from behavior presets
- AI operates within defined parameters/ROE

---

#### Job 3.3: Configure SAM Engagement Doctrine (P2)
**When** I'm setting up SAM threats
**I want to** use doctrinal presets that match real-world threat behavior
**So that** pilots learn correct reactions and timing

**Acceptance Criteria:**
- Presets match known threat doctrine
- Engagement range, shot doctrine, reload times configurable

---

### Category 4: Scenario Structure & Triggers

#### Job 4.1: Build Discrete Training Exercises (P0)
**When** I'm running focused skill training
**I want to** create single, contained exercises
**So that** pilots can practice specific maneuvers repeatedly

**Acceptance Criteria:**
- Create standalone exercise with defined start/end conditions
- Exercise can be saved as template
- Clear success/failure criteria
- Exercise can be reset and repeated

---

#### Job 4.2: Build Multi-Phase Missions (P0)
**When** I'm running full mission simulations
**I want to** create long-form scenarios with multiple phases
**So that** pilots experience realistic mission flow

**Phase Structure:**
1. Ingress — Navigation, threat avoidance
2. Target Area — Engagement, strike execution
3. Egress — Escape, defensive maneuvering

**Acceptance Criteria:**
- Define multiple phases within a single scenario
- Set transition conditions (time, location, or event-based)
- Each phase can have independent entity activations

---

#### Job 4.3: Configure Conditional Triggers (P0)
**When** I want scenarios to react to pilot actions
**I want to** set up conditional triggers that activate entities
**So that** training adapts dynamically

**Trigger Types:**
| Trigger | Example |
|---------|---------|
| Range-based | "When blue crosses 30nm, launch interceptors" |
| Detection-based | "When SAM radar detects blue, scramble QRF" |
| Time-based | "At T+10 minutes, activate secondary threat" |

**Acceptance Criteria:**
- Visual trigger builder (no coding)
- Triggers can chain (A triggers B triggers C)
- Clear indication when trigger conditions will be met

---

### Category 5: Mission Planning Phase

#### Job 5.1: Ground Planning Phase with Brief Generation (Stretch)
**When** I'm preparing for a training sortie
**I want to** build and brief the entire scenario before flight
**So that** all participants understand objectives and threats

**Capabilities:**
- Map-based scenario builder
- Brief generation (printable/shareable)
- Scenario validation
- Pilot call sign and aircraft configuration fields

---

### Category 6: Terrain & Environment

#### Job 6.1: Overlay Mission-Relevant Terrain (P1)
**When** I'm rehearsing a real-world mission
**I want to** display terrain data from the operational area
**So that** pilots can visualize terrain masking and ingress routes

**Acceptance Criteria:**
- Terrain data pre-loaded in application
- Terrain renders accurately on map
- Supports low-altitude route planning visualization

---

### Category 7: User Self-Service

#### Job 7.1: Customize Scenarios Without Engineering Support (P1)
**When** I have a specific training need
**I want to** modify or create scenarios myself in under 15 minutes
**So that** I'm not dependent on contractors

**Acceptance Criteria:**
- Intuitive UI (no coding required)
- Template library for common scenarios
- Save/share custom scenarios with unit

---

#### Job 7.2: Access Scenario Templates for Common Training (P1)
**When** I need to run standard syllabus training
**I want to** select from pre-built templates
**So that** I can be mission-ready quickly

**Template Categories:**
- Basic Fighter Maneuvers (BFM)
- Basic Formation
- Tactical Intercept
- Air-to-Air Refueling
- SEAD/DEAD
- Large Force Engagement (LFE)

**Acceptance Criteria:**
- Templates organized by training category
- Each template includes default entity placements and behaviors
- Templates can be customized and saved as new templates

---

### Category 8: Navigation & Reference Points

#### Job 8.1: Define Points of Interest on the Map (P0)
**When** I'm building a scenario
**I want to** place and label navigation and reference points
**So that** pilots have clear references for coordination

**Point Types:**
| Type | Description |
|------|-------------|
| Waypoints | Navigation points for route planning |
| CAP Points | Combat Air Patrol station points |
| Bullseyes | Common reference for tactical communication |
| Cultural Points | Landmarks, airfields, cities |

**Acceptance Criteria:**
- Each point type has distinct visual representation
- Points can be named/labeled
- Bullseye can be set as scenario reference point
- Points can be shown/hidden by type

---

### Category 9: Network & Multi-Ship Configuration

#### Job 9.1: Configure Networked ATARS Session (P0)
**When** I'm setting up a multi-ship training session
**I want to** configure network parameters for all participants
**So that** multiple aircraft can share the same scenario in real-time

**Acceptance Criteria:**
- Input fields for: IP address, node number, server/client designation
- Messaging and bandwidth configuration

---

### Scenario Transfer (V1 Scope)

#### Job 10.1: Transfer Scenario to Aircraft Systems
**When** I've completed scenario planning
**I want to** transfer the scenario to the ATARS-equipped aircraft
**So that** the scenario is ready for in-flight execution

**Acceptance Criteria:**
- Export scenario in ATARS-compatible format
- Transfer mechanism defined (method TBD based on platform)
- Validation confirms scenario integrity after transfer
- Clear confirmation of successful transfer

---

## 6. Core UI/UX Flows

> **Note:** These are high-level flows. Design team will define detailed interactions, screens, and controls.

### Flow 1: Create New Scenario from Scratch

```
1. Launch → Select "New Scenario"
2. Configure basics → Name, platform (T-38, F-16, MC-130J, T-7, etc.)
3. Map canvas loads → Operational area displayed
4. Place entities → Drag from entity palette onto map
   - Threat aircraft
   - SAMs
   - Geometric boundaries
   - Navigation points
5. Configure behaviors → For each entity:
   - Set route (if applicable)
   - Assign behavior preset
   - Configure triggers
6. Validate scenario → System checks for conflicts/errors
7. Preview → Animate timeline to verify triggers
8. Export → Transfer to aircraft system
```

### Flow 2: Customize Existing Template

```
1. Launch → Select "Templates"
2. Browse templates → Filter by category (BFM, Tactical Intercept, etc.)
3. Preview template → See default entity layout and behaviors
4. Select template → "Customize" button
5. Modify as needed:
   - Adjust entity positions
   - Change behavior parameters
   - Add/remove entities
6. Save options:
   - Save as new template (for reuse)
   - Export directly (one-time use)
7. Transfer to aircraft
```

### Flow 3: Configure Multi-Ship Network Session

```
1. Launch → Select "Multi-Ship Setup"
2. Define participants:
   - Number of aircraft
   - Roles (server/client)
   - Node assignments
3. Configure network:
   - IP addresses
   - Messaging parameters
   - Bandwidth settings
4. Assign scenario:
   - Select existing scenario or create new
   - Designate which entities are networked vs. constructive
5. Validate network config → System confirms connectivity requirements
6. Export configuration → Each aircraft receives appropriate settings
7. Pre-flight verification → Status indicators confirm readiness
```

---

## 7. Platform Considerations

ATARS Planning V1 targets three platforms. Platform-specific considerations are high-level; detailed constraints defined in separate technical documentation.

| Platform | Considerations |
|----------|----------------|
| **T-38** | Primary training aircraft; scenarios emphasize BFM, tactical intercept, formation |
| **F-16** | [Specific constraints TBD based on ATARS integration status and contractual requirements] |
| **MC-130J** | Different mission profile; scenarios may emphasize penetration, threat avoidance, timing |

**Common Requirements:**
- All platforms share the same ATARS Planning interface
- Scenario export format consistency across platforms is TBD (pending contractual requirements, particularly for F-16)
- Platform selection affects available templates and validation rules

---

## 8. Out of Scope for V1

The following are explicitly **not included** in ATARS Planning V1:

| Item | Rationale |
|------|-----------|
| **In-flight execution features** | Handled by existing tablet/AR system; ATARS Planning is ground-only |
| **Debrief/replay functionality** | Post-flight analysis is a separate product area |
| **AI-generated scenarios** | V1 is manual/template-based; AI scenario generation is future |
| **External system integrations** | No JMPS or other external integrations for V1 |
| **Classified network support** | V1 targets unclassified environments only |

---

## 9. Dependencies & Assumptions

### Dependencies

| Dependency | Owner | Impact if Delayed |
|------------|-------|-------------------|
| ATARS scenario file format spec | Platform teams | Cannot build export function |
| Transfer mechanism definition | Platform teams | Cannot complete transfer flow |
| Threat/entity library definition | Design | Limited entity types available |
| Deployment model decision (desktop application vs. web application) | Contracts/Legal | Architecture decisions blocked |

### Assumptions

1. Users have basic familiarity with military map interfaces
2. Network configuration in ATARS Planning defines settings for aircraft-to-aircraft networking in flight (ground connectivity not required)
3. Template library will include at minimum: 6 template categories
4. Threat/entity library contents will be determined by Red Six based on client needs and platform capabilities

---

## 10. Open Questions

| Question | Owner | Due Date |
|----------|-------|----------|
| What is the deployment model (desktop application vs. web application)? | Contracts | TBD |
| What is the scenario transfer mechanism per platform? | Platform teams | TBD |
| What threat/entity types are available for V1? | Design | TBD |
| Are there airframe-specific scenario constraints? | Platform teams | TBD |

---

## Appendix A: Priority Summary

| Priority | Jobs | Count |
|----------|------|-------|
| **P0** | 1.1, 1.2, 2.1, 2.2, 2.3, 2.4, 3.1, 4.1, 4.2, 4.3, 8.1, 9.1 | 12 |
| **P1** | 6.1, 7.1, 7.2 | 3 |
| **P2** | 3.2, 3.3 | 2 |
| **Stretch** | 5.1 | 1 |

---

## Appendix B: Glossary

| Term | Definition |
|------|------------|
| **ATARS** | Augmented Reality Training System — Red Six's in-flight AR training platform |
| **Bullseye** | Common reference point for tactical communication (bearing/range calls) |
| **CAP** | Combat Air Patrol — defensive patrol station |
| **Constructive** | Synthetic/simulated entities (not real aircraft) |
| **IP** | Instructor Pilot |
| **JMPS** | Joint Mission Planning System |
| **LFE** | Large Force Engagement |
| **MEZ** | Missile Engagement Zone |
| **SAM** | Surface-to-Air Missile |
| **SEAD/DEAD** | Suppression/Destruction of Enemy Air Defenses |

---

## Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-01-30 | [Author] | Initial draft |
