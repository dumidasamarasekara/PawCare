# PawCare Constitution

<!-- Mirror of .specify/memory/constitution.md — keep both in sync. -->

PawCare is a small demo application used for a workshop. These principles keep the code
simple enough that any developer can read it, explain it, and extend it in one sitting.
They apply to humans and to AI agents equally.

## Core Principles

### I. Simplicity First

The MVP MUST stay small and easy to understand. Pick the simplest thing that satisfies the
requirement. Do not add layers, patterns, frameworks, or infrastructure "for later".

If you cannot explain why a piece of complexity exists, remove it.

Not needed here: microservices, CQRS, event sourcing, message brokers, distributed caching.

### II. Clear Domain Model

Business concepts MUST appear in the code by name. The domain is deliberately tiny:

```
Pet (Id, Name, Type, DateOfBirth)  1 ──── *  CareActivity (Id, PetId, ActivityType, Date, Notes, NextDueDate)
```

Use these names in classes, methods, and tests. Do not hide them behind generic names like
`Item`, `Record`, or `DataService`.

### III. User Data Integrity

Invalid care records MUST NOT be accepted. Validate at the right place:

| Layer | Checks | Example |
|---|---|---|
| API | shape and format | `Name` is required, `Date` is a real date |
| Application | workflow rules | the pet must exist before recording care for it |
| Domain | invariants | `NextDueDate` cannot be before the activity date |

Invalid data MUST fail clearly with a useful message. Internal exceptions are never returned
to the caller — the API layer turns them into a proper error response.

### IV. Testable Behavior

Every business rule MUST have an automated test. Test what the rule does, not how it is wired.

Good: "recording care for a pet that does not exist returns an error".
Not useful: "the repository's `Add` method was called once".

Tests are written as part of the work, not after it.

### V. Separation of Concerns

UI, business logic, and persistence stay in separate layers, with dependencies pointing inward:

```
Presentation (API / React)  →  Application (use cases)  →  Domain (entities, rules)
Infrastructure (PostgreSQL, repositories)  →  Application / Domain contracts
```

Rules that are easy to break:

- Business rules MUST NOT live in controllers or React components.
- The Domain layer MUST NOT reference EF Core, SQL, HTTP, or any UI framework.
- Application code depends on interfaces (`IPetRepository`); Infrastructure implements them.

### VI. Use the Approved Design

The UI MUST be built from the existing design system — its colors, typography, spacing, radius,
shadows, buttons, cards, inputs, navigation, and activity icons.

Do not invent a new visual treatment for one screen when a design-system component already
covers it. If something is genuinely missing, add it to
[design-system.md](../UX/design-system.md) first, then use it.

The MVP is six screens: Pet List, Pet Details, Record Care, Care History, Upcoming Care, and the
desktop layout. Every list needs an empty state.

### VII. Incremental Delivery

Work MUST be split into small pieces that can each be built, tested, and demoed on their own.

Each task states what it delivers and how you know it is done. If a task cannot be verified
by itself, it is too big — split it.

## Scope and Technical Constraints

The MVP is exactly five capabilities. Anything else is out of scope — say so rather than
building it.

1. Manage Pets
2. Record Care Activity
3. View Care History
4. View Upcoming Care
5. View Pet Details

API surface:

```
POST /pets            GET /pets            GET /pets/{id}
POST /pets/{id}/care-activities
GET  /pets/{id}/care-history
GET  /pets/{id}/upcoming-care
```

Persistence is PostgreSQL, and all persistence code stays in Infrastructure.

## How We Work

- **Requirements come first.** Every piece of code MUST answer: *which requirement does this
  implement?* Do not invent business behavior.
- **Ambiguity is surfaced, not guessed.** If a spec is unclear, say so. If an assumption is
  unavoidable, make the smallest one and write it down in the spec or plan.
- **Documentation is short.** Specs, plans, and task lists MUST be readable in a few minutes.
  Clarity beats completeness — this is a workshop, not a compliance artifact.
- **Read the docs before building.** In order of authority: this constitution →
  [productVision.md](productVision.md) →
  [architecture.md](../engineering/architecture.md) →
  [coding-standards.md](../engineering/coding-standards.md) →
  [quality.md](../engineering/quality.md).
  For anything with a UI, also read [ux-guidelines.md](../UX/ux-guidelines.md) (behavior)
  and [design-system.md](../UX/design-system.md) (visuals).

## Governance

This constitution outranks habit, preference, and anything an AI agent infers on its own.

When two principles conflict, prefer in this order:

1. Correctness
2. Simplicity
3. Maintainability
4. Performance

Deviating is allowed when a requirement forces it — but the deviation MUST be intentional and
written down in the feature's spec or plan, with the reason.

Amendments: propose the change, get it agreed, update this file and its mirror at
[.specify/memory/constitution.md](../../.specify/memory/constitution.md), and bump the version.
MAJOR = a principle removed or redefined; MINOR = a principle or section added; PATCH =
wording and clarifications.

**Version**: 1.1.1 | **Ratified**: 2026-08-18 | **Last Amended**: 2026-08-19
