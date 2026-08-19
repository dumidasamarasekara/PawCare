# PawCare — Product Vision

## 1. Vision

PawCare is a simple application that helps pet owners keep track of
their pets and their routine care activities.

The application should make it easy to answer two questions:

1. What care has my pet received?
2. What care is coming up?

---

## 2. Target User

Pet owners who want a simple way to keep track of routine pet care.

The MVP is designed for a single user.

---

## 3. Problem

Pet owners often forget:

- when a care activity was last performed
- what care their pet has received
- when the next care activity is due

The application provides a simple history and upcoming-care view.

---

## 4. MVP Scope

The MVP contains only five capabilities:

1. Manage Pets
2. Record Care Activity
3. View Care History
4. View Upcoming Care
5. View Pet Details

Anything outside these capabilities is out of scope.

---

## 5. Core Concepts

### Pet

A pet has:

- Id
- Name
- Type
- Date of Birth

### Care Activity

A care activity belongs to a pet and contains:

- Id
- Pet Id
- Activity Type
- Date
- Notes
- Next Due Date

Example activities:

- Vaccination
- Bath
- Flea Treatment
- Nail Trim

---

## 6. MVP Success

The MVP is successful when a user can:

- create a pet
- record care for that pet
- view the pet's care history
- see upcoming care
- view the pet's details

The application should remain simple and easy to understand.