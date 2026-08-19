# Git Workflow

**One task = one branch = one pull request.** No exceptions, including small changes.

The only remote is `https://github.com/dumidasamarasekara/PawCare.git`. Never push to another
repository.

---

## 1. The Rule

`main` is never committed to directly. Every task from `tasks.md` gets its own branch, its own
PR, and its own review — even a one-line change.

This keeps each piece of work small enough to read in a few minutes, which is the point of
Principle VII in the [constitution](../product/constitution.md).

---

## 2. Branch Names

```
<feature-number>-<feature-slug>/<task-id>-<short-description>
```

Examples:

```
001-record-care/T003-add-pet-repository
001-record-care/T007-validate-next-due-date
002-upcoming-care/T002-upcoming-care-endpoint
```

The feature number and slug match the folder in `specs/`. The task id matches `tasks.md`.
Anyone reading the branch name should know exactly which task it implements.

Work that is not a spec task — documentation, tooling, project setup — has no task id, so use:

```
docs/<description>          e.g. docs/git-workflow
chore/<description>         e.g. chore/add-ci-pipeline
```

The branch-and-PR rule still applies to these.

---

## 3. The Cycle

```bash
# 1. Start from an up-to-date main
git checkout main
git pull

# 2. Branch for the task
git checkout -b 001-record-care/T003-add-pet-repository

# 3. Do the work, committing as you go
git add -A
git commit -m "feat: add pet repository interface and EF Core implementation"

# 4. Push and open the PR
git push -u origin 001-record-care/T003-add-pet-repository
gh pr create --fill

# 5. After the PR is merged, clean up
git checkout main
git pull
git branch -d 001-record-care/T003-add-pet-repository
```

---

## 4. Commit Messages

Use a short prefix so history is scannable:

| Prefix | Use for |
|---|---|
| `feat:` | New behavior |
| `fix:` | Bug fix |
| `test:` | Tests only |
| `refactor:` | Restructuring with no behavior change |
| `docs:` | Documentation |
| `chore:` | Build, config, tooling |

Write what changed and why, not how. One line is usually enough.

---

## 5. Pull Requests

Every PR must say which task it implements. The
[PR template](../../.github/pull_request_template.md) asks for this — fill it in.

A PR is ready to merge when:

- it implements exactly one task
- the build passes and tests pass
- business rules in the change have tests
- it follows the [architecture](architecture.md) and [coding standards](coding-standards.md)
- a reviewer has approved it

Keep PRs small. If a PR is hard to review in one sitting, the task was too big — split it.

---

## 6. Branches Are Not Feature State

Spec Kit tracks the current feature in `.specify/feature.json`, **not** in the git branch. That
file is machine-local and gitignored.

So switching branches does not change which feature Spec Kit thinks you are working on. If a
script reports "Feature directory not found", set `SPECIFY_FEATURE_DIRECTORY` or re-run the
specify command — do not assume the branch name fixed it.
