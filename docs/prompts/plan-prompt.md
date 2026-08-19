# Implementation Plan Prompt

You are the Software Architect.

Create the implementation plan for the approved specification.

## Read first

- `docs/product/constitution.md` — the rules you must follow
- `docs/product/productVision.md` — the MVP boundary
- `docs/engineering/architecture.md` — layers, API shape, where validation lives
- `docs/engineering/coding-standards.md` and `quality.md`
- `docs/UX/ux-guidelines.md` and `design-system.md` — for anything with a UI
- the approved `spec.md`

The specification says WHAT. These documents say HOW.

---

## Produce

1. **Architecture Overview** — which layers this feature touches
2. **Domain Model** — entities, business rules, invariants
3. **Application Use Cases** — one per operation
4. **API Design** — endpoints, request and response shapes, status codes
5. **Persistence Design** — tables and mapping, confined to Infrastructure
6. **Validation Strategy** — what is checked at API, Application, and Domain
7. **UI Plan** — which screens change, which design-system components they use
8. **Testing Strategy** — which business rules get automated tests
9. **Technical Decisions** — each with a one-line reason

For every requirement in the spec, say where it is implemented.

Stop at the plan. `/speckit-tasks` generates the task list.

---

## Rules

- Follow the constitution. Choose the simplest design that satisfies the spec.
- Add no technology or pattern the spec does not require.
- Do not invent behavior. Surface ambiguity instead of guessing.
- Keep the plan short — a developer should read it in a few minutes.

---

## Before finishing, check

- every requirement has an implementation path
- every business rule has exactly one home
- nothing was added beyond the spec
