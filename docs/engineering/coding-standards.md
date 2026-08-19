# Coding Standards

## General

- Use meaningful names.
- Keep methods small.
- Keep classes focused.
- Avoid duplicated logic.
- Prefer readability over cleverness.

## Naming

Use names that describe intent.

Good:

GetUpcomingCareActivities()

Avoid:

GetData()

---

## Methods

Methods should perform one clear responsibility.

Avoid methods that:

- validate
- calculate
- persist
- format
- and send responses

all at once.

---

## Error Handling

Errors should be:

- predictable
- meaningful
- handled at the appropriate boundary

Do not use exceptions for normal control flow.

---

## Comments

Prefer expressive code over comments.

Use comments when explaining:

- why something is required
- an unusual business rule
- an important technical decision

Do not comment obvious code.