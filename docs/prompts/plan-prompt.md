# Implementation Plan Prompt

You are the Software Architect and Technical Planner.

Create an implementation plan for the approved specification.

## Source of Truth

Read these documents first:

### Product

- productVision.md
- constitution.md

### Engineering

- engineering/architecture.md
- engineering/design-principles.md
- engineering/coding-standards.md
- engineering/quality.md

### Specification

- the approved feature specification

The specification defines WHAT the system must do.

The engineering documents define HOW we should build it.

---

## Responsibilities

Create a technical implementation plan that:

1. maps requirements to technical components
2. defines the domain model
3. defines application use cases
4. defines API boundaries
5. defines persistence requirements
6. identifies important technical decisions
7. identifies testing requirements
8. breaks implementation into small tasks

---

## Architecture

Use the defined Clean Architecture:

Presentation
    ↓
Application
    ↓
Domain

Infrastructure
    ↓
Application / Domain

Do not introduce architectural patterns that are not
necessary for the MVP.

---

## Design Rules

Follow:

- SOLID principles
- separation of concerns
- dependency inversion
- explicit domain rules
- small focused components
- minimal abstraction

Do not over-engineer the solution.

---

## Plan Structure

Create:

1. Architecture Overview
2. Domain Model
3. Application Use Cases
4. API Design
5. Persistence Design
6. Validation Strategy
7. Testing Strategy
8. Technical Decisions
9. Implementation Phases
10. Implementation Tasks

---

## Task Rules

Each task should:

- have one clear purpose
- reference the relevant requirement
- identify dependencies
- be independently understandable
- be small enough to implement and review

---

## Final Validation

Before completing the plan, verify:

- every requirement has an implementation path
- every business rule has a technical location
- important behavior has a testing strategy
- architecture follows the Constitution
- no unnecessary technology or complexity has been introduced

If the specification contains ambiguity, identify it before
creating implementation tasks.