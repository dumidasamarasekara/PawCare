# Architecture Guidelines

## 1. Architecture Style

The PawCare application follows **Clean Architecture**.

The main goal is to keep business logic independent from
frameworks, databases, UI and external services.

The high-level structure is:

```text
┌─────────────────────────────┐
│        Presentation         │
│       API / React UI        │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│        Application          │
│       Use Cases / Logic     │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│           Domain            │
│ Entities / Business Rules   │
└─────────────────────────────┘
               ▲
               │
┌──────────────┴──────────────┐
│       Infrastructure        │
│ Database / External Systems │
└─────────────────────────────┘
```

The exact project structure may evolve during implementation,
but these responsibilities must remain clear.

---

## 2. Domain Layer

The Domain represents the core business concepts and rules.

For PawCare, examples include:

- Pet
- CareActivity
- CareActivityType

The Domain may contain:

- Entities
- Value Objects
- Domain rules
- Domain services when genuinely required

The Domain must NOT depend on:

- Database technology
- HTTP
- UI frameworks
- External APIs
- Infrastructure implementations

### Example

The rule:

> A care activity must belong to an existing pet.

is a business rule and should not exist only inside an API controller.

---

## 3. Application Layer

The Application layer represents the actions the system can perform.

Examples:

```text
CreatePet
GetPet
RecordCareActivity
GetCareHistory
GetUpcomingCare
```

The Application layer is responsible for:

- coordinating use cases
- validating application-level rules
- calling domain behavior
- coordinating persistence through abstractions
- returning appropriate results

The Application layer should not contain:

- HTTP-specific behavior
- UI logic
- database-specific implementation details

---

## 4. Presentation Layer

The Presentation layer exposes the application to users.

For the MVP this may include:

- REST API
- React UI

The Presentation layer is responsible for:

- receiving requests
- basic input validation
- mapping requests to application commands/queries
- returning responses
- handling HTTP/UI concerns

The Presentation layer must not contain core business rules.

### Avoid

```text
Controller
    ↓
Business Logic
    ↓
Database
```

### Prefer

```text
Controller
    ↓
Application Use Case
    ↓
Domain
    ↓
Persistence Abstraction
```

---

## 5. Infrastructure Layer

Infrastructure contains technical implementations required by
the application.

Examples:

- PostgreSQL
- Entity Framework Core
- Repository implementations
- External services
- Logging
- Messaging

Infrastructure implements interfaces/contracts required by
the Application or Domain layers.

The business logic should not need to know how infrastructure
is implemented.

---

## 6. Dependency Rule

Dependencies should point toward the core of the application.

```text
Presentation
      ↓
Application
      ↓
Domain

Infrastructure
      ↓
Application / Domain
```

The Domain should have the fewest dependencies.

A change to the database should not require changing
the business rules.

---

## 7. Dependency Inversion

High-level application logic should depend on abstractions
rather than concrete infrastructure implementations.

For example:

```text
Application
     ↓
IPetRepository
     ↑
     │
PetRepository
     ↓
PostgreSQL
```

The Application knows about `IPetRepository`.

It does not need to know whether the data comes from:

- PostgreSQL
- SQL Server
- an in-memory database
- another storage mechanism

---

## 8. Separation of Concerns

Each layer should have one clear responsibility.

| Layer | Responsibility |
|---|---|
| Presentation | User/API interaction |
| Application | Use cases and workflows |
| Domain | Business rules |
| Infrastructure | Technical implementations |

Avoid placing unrelated responsibilities in the same layer.

---

## 9. Domain Model

The initial domain model is intentionally small.

```text
Pet
 ├── Id
 ├── Name
 ├── Type
 └── DateOfBirth

CareActivity
 ├── Id
 ├── PetId
 ├── ActivityType
 ├── Date
 ├── Notes
 └── NextDueDate
```

Relationship:

```text
Pet 1 ─────────── * CareActivity
```

A Pet can have many Care Activities.

A Care Activity belongs to exactly one Pet.

---

## 10. API Design

The API should expose application capabilities rather than
database operations.

Example:

```text
POST   /pets
GET    /pets
GET    /pets/{id}

POST   /pets/{id}/care-activities
GET    /pets/{id}/care-history
GET    /pets/{id}/upcoming-care
```

API design should remain simple and aligned with the
application's use cases.

Avoid exposing internal database structures unnecessarily.

---

## 11. Persistence

The MVP uses PostgreSQL for persistence.

Persistence concerns belong in Infrastructure.

The Domain should not contain:

```text
SQL
Entity Framework attributes
Database connection logic
Database-specific queries
```

Persistence models may differ from domain models when there
is a clear reason to keep them separate.

For this small MVP, avoid unnecessary complexity.

---

## 12. Error Handling

Errors should be handled at the appropriate boundary.

Example:

```text
Pet does not exist
        ↓
Application detects business/application error
        ↓
Presentation converts it to an appropriate API response
```

Do not expose internal exceptions or infrastructure details
to users.

Errors should be:

- predictable
- meaningful
- consistent

---

## 13. Validation

Validation happens at different levels.

### Presentation Validation

Protect the application boundary.

Examples:

- required fields
- valid formats
- invalid request structure

### Application Validation

Protect application workflows.

Example:

```text
Pet must exist before recording care.
```

### Domain Validation

Protect business invariants.

Example:

```text
NextDueDate cannot be earlier than the activity date.
```

Do not rely only on UI validation.

---

## 14. Testing Architecture

Testing should exist at multiple levels.

```text
                 ┌───────────────┐
                 │   UI / API    │
                 │    Tests      │
                 └───────┬───────┘
                         │
                 ┌───────▼───────┐
                 │ Integration   │
                 │    Tests      │
                 └───────┬───────┘
                         │
                 ┌───────▼───────┐
                 │    Domain     │
                 │    Tests      │
                 └───────────────┘
```

Focus testing effort on important business behavior.

---

## 15. Avoid Over-Engineering

The architecture should support the MVP without creating
unnecessary complexity.

Do NOT introduce something simply because it is popular.

Examples of things that are not required initially:

- Microservices
- Event-driven architecture
- CQRS
- Event sourcing
- Distributed caching
- Message brokers
- Complex design patterns

If a future requirement justifies them, they can be introduced later.

---

## 16. Architecture Decision Rule

When making a technical decision, ask:

1. Does the requirement need it?
2. Does it simplify or complicate the system?
3. Does it respect the Constitution?
4. Does it improve maintainability?
5. Can we defer the decision until it is actually needed?

Prefer the simplest architecture that satisfies the requirements.

---

## 17. Architecture Summary

The PawCare application should follow this principle:

> **Keep the business logic independent, keep boundaries clear,
> and use technology to support the business rather than allowing
> technology to define the business.**

The architecture should be simple enough for a junior developer
to understand and strong enough to evolve as the product grows.
