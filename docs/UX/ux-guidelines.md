# UX Guidelines

## Purpose

This document defines the UX principles and constraints for the
PawCare application.

The purpose is to ensure that the user experience remains consistent
across all features and that AI agents use the approved UX design
rather than inventing new patterns.

The following UX documents are the primary UX source of truth:

| Document | Role |
|---|---|
| [DesignSystem/PawCare Wireframes.dc.html](DesignSystem/PawCare%20Wireframes.dc.html) | Low-fi wireframes — screen structure and layout |
| [DesignSystem/PawCare Design System.dc.html](DesignSystem/PawCare%20Design%20System.dc.html) | The design canvas — tokens, components, all six screens, rendered |
| [design-system.md](design-system.md) | The same visual language written out for developers and AI agents |
| [ux-guidelines.md](ux-guidelines.md) | This file — how the UI should behave |

Wireframes show *what goes where*; the design system shows *what it looks like*.
When the two disagree, the design system wins — it is the later artifact.

When these documents conflict with an implementation preference,
the UX documents should be followed unless an explicit product
decision changes them.

---

# 1. Simple and Focused

PawCare is an intentionally simple MVP.

The user should be able to understand what to do on a screen
without needing instructions.

Prefer:

- fewer actions
- clear labels
- obvious primary actions
- simple navigation
- progressive disclosure when necessary

Avoid:

- unnecessary screens
- unnecessary configuration
- complex workflows
- excessive information
- features outside the MVP

---

# 2. Pet First

The application should always make the selected pet clear.

When displaying pet-related information, show:

- pet name
- pet type
- pet image/avatar when appropriate

Users should never be uncertain about which pet they are
viewing or modifying.

For example:

> Bruno · Golden Retriever

is preferable to displaying only:

> Pet Details

---

# 3. Important Information First

Prioritize information based on what the user needs most.

For the PawCare MVP, the priority is generally:

1. Pet identity
2. Upcoming care
3. Recent care
4. Additional pet information

Upcoming care should be visually easy to discover.

Do not hide important care information behind unnecessary navigation.

---

# 4. Clear Primary Actions

Every important screen should have one obvious primary action.

Examples:

Pet Details:

> Record Care

Record Care:

> Save Activity

Avoid presenting multiple actions with equal visual importance.

Secondary actions should remain visually subordinate.

---

# 5. Consistent Visual Language

All screens must use the approved design system.

Reuse:

- colors
- typography
- spacing
- border radius
- shadows
- buttons
- cards
- inputs
- icons
- navigation
- activity indicators

Do not create a new visual treatment for an individual feature
when an existing design-system component can be reused.

---

# 6. Friendly and Approachable

PawCare should feel like a friendly consumer application.

The visual language should be:

- warm
- calm
- playful
- modern
- approachable

Use pet imagery and illustrations where they add value.

Avoid making the application feel like:

- enterprise software
- hospital/clinical software
- an administration dashboard
- a complex productivity tool

The application should feel like something a pet owner would
enjoy using.

---

# 7. Calm Visual Hierarchy

The interface should not compete for the user's attention.

Use:

- whitespace
- typography
- subtle color
- cards
- icons
- visual grouping

to create hierarchy.

Avoid:

- excessive colors
- excessive badges
- unnecessary animations
- large numbers of competing buttons
- heavy borders
- excessive shadows

---

# 8. Activity Visual Language

Care activities should be visually recognizable.

Use consistent icons and accent colors for activity types.

Example:

| Activity | Icon | Accent |
|---|---|---|
| Vaccination | cross | Coral |
| Bath / Grooming | drop | Teal |
| Flea Treatment | leaf | Orange |
| Nail Trim | paw | Yellow |
| General Care | paw | Sage |

The exact color values and icon names come from
[design-system.md](design-system.md) §5.

Do not create new activity colors independently in individual screens.
If a new activity type is needed, add it to the design system first.

---

# 9. Navigation

Navigation should remain minimal.

The MVP should not require complex navigation structures.

Users should be able to move naturally between:

```text
Pets
  ↓
Pet Details
  ↓
Care History  ·  Record Care  ·  Upcoming Care
```

Rules:

- Every screen below the top level has a back arrow in the top-left.
- Mobile uses a bottom navigation bar; desktop uses a left sidebar.
  Both are defined in the design system — do not invent a third pattern.
- Do not add tabs, breadcrumbs, or nested menus to the MVP.

---

# 10. Screens

The MVP is exactly six screens. Each one is drawn in the design canvas.

| Screen | Purpose | Primary action |
|---|---|---|
| Pet Home / Pet List | See all pets and what care is next | Add Pet |
| Pet Details | Everything about one pet | Record Care |
| Record Care Activity | Add a care record | Save Activity |
| Care History | Past care for one pet, newest first | — |
| Upcoming Care | Care that is due, grouped by time | — |
| Desktop layout | Pet Details adapted to a wide screen | Record Care |

Do not add screens beyond these. If a feature seems to need one,
it is probably outside the MVP.

---

# 11. Empty States

Every list must have an empty state — never show a blank screen.

An empty state has a soft round icon, a short bold line explaining
what is missing, and a muted line saying what to do next.

| List | Message | Hint |
|---|---|---|
| Pets | No pets yet | Add your first pet to get started |
| Care | No care recorded | Record an activity to see it here |

---

# 12. Errors and Feedback

Validation errors appear next to the field that caused them, not
in a popup or a banner at the top of the page.

An invalid field gets a red border and a short message underneath.
Write messages in plain language:

> Name is required

not:

> Validation failed for property 'Name'

Never show raw exception text or technical error codes to the user.

---

# 13. Responsive Behavior

The same application works on mobile and desktop. Desktop is a wider
arrangement of the same content — not a different product with
different features.

Design mobile first, then let the layout expand.