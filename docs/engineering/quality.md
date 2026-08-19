# Quality Guidelines

## Definition of Done

A feature is complete when:

- requirements are implemented
- acceptance criteria pass
- business rules have automated tests
- code follows the architecture guidelines
- validation is implemented
- no known critical defects remain

---

## Testing

### Unit Tests

Test important domain and application behavior.

### Integration Tests

Test important interactions with infrastructure,
such as persistence.

### API Tests

Verify that API behavior matches the specification.

---

## Code Quality

Before completing a task:

- build successfully
- tests pass
- static analysis passes
- formatting is consistent
- no unnecessary warnings are introduced

---

## Specification Validation

Implementation should be checked against the specification.

The following question should always be answerable:

> Which requirement does this code implement?