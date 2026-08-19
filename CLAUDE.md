# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository currently is

PawCare is a greenfield project. **No application code exists yet** — the repository contains only
engineering/product documentation ([docs/](docs/)) and a GitHub Spec Kit v0.16.3 scaffold ([.specify/](.specify/)).
There is no build, no test runner, and no package manifest. Those commands should be documented here once
the first implementation feature lands.

**The only remote for this project is `https://github.com/dumidasamarasekara/PawCare.git`** (public,
wired as `origin`, tracking `main`). Never add a second remote, push elsewhere, or create another
repository for this project. The local folder is still named `PetCare`; the product and repo are PawCare.

## Git workflow — one task, one branch, one PR

**Never commit to `main`.** Every task in `tasks.md` gets its own branch and its own pull request,
including small ones. Full rules in [docs/engineering/git-workflow.md](docs/engineering/git-workflow.md).

```bash
git checkout main && git pull
git checkout -b 001-record-care/T003-add-pet-repository   # <feature>/<task-id>-<description>
# ... work, commit with feat:/fix:/test:/refactor:/docs:/chore: prefix ...
git push -u origin 001-record-care/T003-add-pet-repository
gh pr create --fill
```

Every PR states which task and which requirement it implements — the
[PR template](.github/pull_request_template.md) asks for both.

When running `/speckit-implement`, branch before the first task and open a PR per task; do not
batch several tasks into one branch.

Note: branches do **not** drive Spec Kit's feature state — that lives in `.specify/feature.json`
(machine-local, gitignored). Switching branches does not switch the current feature.

Work is expected to arrive through the Spec-Driven Development (SDD) workflow below, not as ad-hoc code changes.

## Spec-Driven Development workflow

Spec Kit is initialized with the `claude` integration, `script: ps` (PowerShell), and
`invoke_separator: "-"` ([.specify/integration.json](.specify/integration.json)) — so commands are invoked as
`/speckit-specify`, not `/speckit.specify`. The skills live in [.claude/skills/](.claude/skills/).

Normal cycle (also encoded in [.specify/workflows/speckit/workflow.yml](.specify/workflows/speckit/workflow.yml),
which adds human approval gates after specify and plan):

```
/speckit-specify  →  /speckit-clarify  →  /speckit-plan  →  /speckit-tasks  →  /speckit-implement
```

Supporting commands: `/speckit-constitution`, `/speckit-checklist`, `/speckit-analyze` (cross-artifact
consistency check), `/speckit-converge`, `/speckit-taskstoissues`.

### Feature state is file-based, not branch-based

This project has no git branches driving state. The "current feature" is resolved, in priority order, from:

1. `$env:SPECIFY_FEATURE_DIRECTORY`
2. `.specify/feature.json` (gitignored, machine-local; written by `create-new-feature.ps1`)

Feature artifacts live in `specs/<NNN>-<short-name>/`: `spec.md`, `plan.md`, `tasks.md`, `research.md`,
`data-model.md`, `quickstart.md`, `contracts/`. If a script errors with "Feature directory not found", set
`SPECIFY_FEATURE_DIRECTORY` or run the specify command — do not guess a path.

### Scripts (PowerShell, [.specify/scripts/powershell/](.specify/scripts/powershell/))

```powershell
# Create a feature directory + spec.md from the template, and persist feature state
.\.specify\scripts\powershell\create-new-feature.ps1 -Json "Record care activity for a pet"

# Resolve paths only, no validation (safe/read-only — does not write feature.json)
.\.specify\scripts\powershell\check-prerequisites.ps1 -PathsOnly

# Gate checks: plan.md required; add -RequireTasks -IncludeTasks for the implement phase
.\.specify\scripts\powershell\check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks
```

Templates resolve through a priority stack: `.specify/templates/overrides/` → `.specify/presets/` →
`.specify/extensions/` → `.specify/templates/` (core). Customize templates via `overrides/`, never by editing
the core templates.

## Source of truth (read before specifying, planning, or implementing)

Documents outrank code and outrank inference. Ordered by authority:

| Document | Role |
|---|---|
| [docs/product/constitution.md](docs/product/constitution.md) | Non-negotiable engineering principles (mirrored at [.specify/memory/constitution.md](.specify/memory/constitution.md) — keep both in sync) |
| [docs/product/productVision.md](docs/product/productVision.md) | Product intent and the MVP boundary |
| [docs/engineering/architecture.md](docs/engineering/architecture.md) | Clean Architecture layers, domain model, API shape, validation/error placement |
| [docs/engineering/coding-standards.md](docs/engineering/coding-standards.md) | Naming, method size, comment policy |
| [docs/engineering/quality.md](docs/engineering/quality.md) | Definition of Done, test levels |
| [docs/engineering/git-workflow.md](docs/engineering/git-workflow.md) | Branch-per-task, PR rules, branch naming, commit prefixes |
| [docs/UX/ux-guidelines.md](docs/UX/ux-guidelines.md) | UX principles — screen inventory, navigation, empty states, error presentation |
| [docs/UX/design-system.md](docs/UX/design-system.md) | Visual language — tokens, components, activity icons/colors (extracted from the design canvas) |
| [docs/UX/DesignSystem/](docs/UX/DesignSystem/) | Design canvases (`.dc.html`) — wireframes (structure) and design system (visuals); `design-system.md` is the readable form of the latter |
| [docs/prompts/](docs/prompts/) | Role prompts for the specify and plan stages |

There is no `engineering/design-principles.md` in this project — `coding-standards.md` +
`architecture.md` cover that role. Do not add a reference to it.

## Architecture the code must follow

Clean Architecture with dependencies pointing inward:

```
Presentation (REST API / React UI)  →  Application (use cases)  →  Domain (entities, business rules)
Infrastructure (PostgreSQL, repositories, logging)  →  Application / Domain contracts
```

Domain model (deliberately small):

```
Pet (Id, Name, Type, DateOfBirth)  1 ──── *  CareActivity (Id, PetId, ActivityType, Date, Notes, NextDueDate)
```

Planned API surface:

```
POST /pets            GET /pets            GET /pets/{id}
POST /pets/{id}/care-activities
GET  /pets/{id}/care-history
GET  /pets/{id}/upcoming-care
```

Rules that are easy to violate and matter here:

- **Business rules never live in controllers or UI components.** "A care activity must belong to an existing
  pet" is an application/domain rule, not a request-validation detail.
- **The Domain layer takes no dependency on EF Core, SQL, HTTP, or UI** — no ORM attributes, no connection logic.
- Application depends on abstractions (`IPetRepository`); Infrastructure implements them.
- Validation is layered: presentation (shape/format) → application (workflow, e.g. pet must exist) → domain
  (invariants, e.g. `NextDueDate` cannot precede the activity date).
- Errors are converted to API responses at the presentation boundary; internal exceptions are never exposed.

## Constraints that shape every decision

- **MVP scope is exactly five capabilities**: Manage Pets, Record Care Activity, View Care History, View
  Upcoming Care, View Pet Details. Anything else is out of scope — say so rather than building it.
- **Do not invent requirements or business behavior.** When a spec is ambiguous, surface the ambiguity; if an
  assumption is unavoidable, make the smallest one and document it explicitly.
- **Explicitly rejected for the MVP** (per architecture.md §15): microservices, event-driven architecture,
  CQRS, event sourcing, distributed caching, message brokers. Introduce them only when a requirement forces it.
- Persistence for the MVP is PostgreSQL; persistence concerns stay in Infrastructure.
- When principles conflict, the governance order is: correctness → simplicity → maintainability → performance.
- Every piece of implemented behavior must answer: *which requirement does this code implement?*
