# Workshop Prompt — Generate the Constitution

Use this during the workshop to regenerate the constitution live, after you have explained to the
audience what a constitution is and why it comes first.

## Before you run it

1. Empty (or delete) both constitution files so the audience sees them generated from nothing:
   - `.specify/memory/constitution.md`
   - `docs/product/constitution.md`
2. Make sure the rest of `docs/` is intact — the prompt tells the agent to read those documents.
3. Run it as `/speckit-constitution` with the prompt below as the argument, or paste the prompt on
   its own. Both work; the slash command is the better demo because it shows the SDD workflow.

Expected result: seven principles, three supporting sections, governance, and a version footer —
written to both files, identical apart from relative link paths.

---

## The prompt — copy everything in the box

```text
Generate the project constitution for PawCare.

CONTEXT
PawCare is a small demo application built live in a workshop. It helps pet owners track their
pets and routine care. It is NOT a production enterprise system. Read these before writing:
  - docs/prompts/constituion-prompt.md   (the source principles)
  - docs/product/productVision.md        (MVP scope)
  - docs/engineering/architecture.md     (Clean Architecture layers)
  - docs/engineering/quality.md          (Definition of Done)
  - docs/engineering/git-workflow.md     (branch and PR rules)
  - docs/UX/design-system.md             (the approved visual language)

STYLE — this matters more than completeness
Write for a junior developer who has to read this once and explain it back. Short sentences,
plain language, concrete examples over abstract rules. Use a small table or code block where it
makes a rule clearer than a paragraph would. No enterprise vocabulary, no ceremony, no rules that
do not apply to this app. If a principle needs more than a short paragraph plus an example, it is
too complicated — simplify it.

WRITE EXACTLY SEVEN PRINCIPLES, IN THIS ORDER
  I.   Simplicity First — simplest thing that works; name what we are NOT using
       (microservices, CQRS, event sourcing, message brokers, distributed caching).
  II.  Clear Domain Model — business concepts appear in code by name. Show the model:
       Pet (Id, Name, Type, DateOfBirth) 1 ── * CareActivity (Id, PetId, ActivityType, Date,
       Notes, NextDueDate). Ban generic names like Item, Record, DataService.
  III. User Data Integrity — invalid care records are never accepted. Include a three-row table
       showing which layer validates what: API = shape/format, Application = workflow rules
       (the pet must exist), Domain = invariants (NextDueDate cannot precede the activity date).
       Internal exceptions are never returned to the caller.
  IV.  Testable Behavior — every business rule has a test. Contrast a good behavioral test with
       a useless mock-assertion test. Tests are part of the work, not after it.
  V.   Separation of Concerns — dependencies point inward. Show the layer diagram. List the three
       rules that are easiest to break (no business rules in controllers or React components; the
       Domain references no EF Core, SQL, HTTP or UI; Application depends on interfaces).
  VI.  Use the Approved Design — build the UI from the existing design system; do not invent a
       visual treatment for one screen. Missing pieces go into design-system.md first. Name the
       six screens and require an empty state on every list.
  VII. Incremental Delivery — split work into independently testable pieces. State the rule:
       one task = one branch = one pull request, main is never committed to directly, branches
       named <feature>/<task-id>-<description>.

THEN THESE THREE SECTIONS
  - "Scope and Technical Constraints" — the five MVP capabilities, the planned API surface, and
    PostgreSQL with persistence confined to Infrastructure.
  - "How We Work" — requirements come first (every change answers "which requirement does this
    implement?"); surface ambiguity instead of guessing; keep all generated documentation short
    and readable in a few minutes; read the docs before building, listing them in authority order
    with UX docs called out for anything with a UI.
  - "Governance" — the constitution outranks habit and anything an agent infers. Conflict order:
    correctness, simplicity, maintainability, performance. Deviations must be intentional and
    written down in the feature's spec or plan. Amendment procedure and the semantic versioning
    policy (MAJOR = principle removed/redefined, MINOR = principle or section added, PATCH =
    wording).

OUTPUT
Write the same constitution to BOTH files, adjusting only the relative link paths:
  - .specify/memory/constitution.md   (prepend the Sync Impact Report as an HTML comment)
  - docs/product/constitution.md      (note at the top that it mirrors the .specify copy)
Footer on both: **Version**: 1.0.0 | **Ratified**: <today> | **Last Amended**: <today>
Use ISO dates. Leave no [PLACEHOLDER] tokens behind.

Do not write application code, and do not create a feature spec. Constitution only.
```

---

## Talking points while it generates

- The constitution is the one document that outranks everything else, including the agent's own
  judgement. Everything downstream — spec, plan, tasks — is checked against it.
- Seven principles is deliberate. A constitution nobody remembers is a constitution nobody follows.
- Principles VI and VII exist because the workshop already has a design system and a git rule.
  Yours would differ — that is the point: this file encodes *your* team's non-negotiables.
- Two copies stay in sync because Spec Kit reads `.specify/memory/`, while humans read `docs/`.

## If you want the exact file from before the workshop

The prompt above produces version **1.0.0** dated today, which reads better live — the audience
watches an initial ratification rather than an amendment. The body text is the same either way.
To reproduce the pre-workshop file byte-for-byte instead, change the footer line to:

```text
Footer on both: **Version**: 1.2.0 | **Ratified**: 2026-08-18 | **Last Amended**: 2026-08-19
```
