# Design System

The visual language for the PawCare UI. Everything here is extracted from the design canvas
[DesignSystem/PawCare Design System.dc.html](DesignSystem/PawCare%20Design%20System.dc.html) — open
that file to see the components rendered. This markdown file is the version AI agents and developers
read while building.

The low-fi structural wireframes behind these screens are in
[DesignSystem/PawCare Wireframes.dc.html](DesignSystem/PawCare%20Wireframes.dc.html).

Colors are written in `oklch()`, which modern browsers support directly. Copy the values as-is.

---

## 1. Color

### Primary

| Name | Value | Used for |
|---|---|---|
| Coral | `oklch(66% 0.17 35)` | Brand, primary buttons, active nav, links |
| Soft Cream | `oklch(97% 0.015 78)` | Screen background |
| Muted Sage | `oklch(68% 0.08 150)` | Success, general care accent |
| Warm Yellow | `oklch(83% 0.13 95)` | Warning, nail trim accent |

### Supporting

| Name | Value | Used for |
|---|---|---|
| Off-White | `oklch(99% 0.006 75)` | Card and input surfaces |
| Charcoal | `oklch(27% 0.02 50)` | Body text |
| Muted Gray | `oklch(56% 0.015 60)` | Secondary text, captions, labels |
| Soft Teal | `oklch(70% 0.08 200)` | Info, bath/grooming accent |
| Border | `oklch(90% 0.015 75)` | Dividers, icon-button border |
| Input border | `oklch(88% 0.015 75)` | Field outlines |

### Semantic

| Name | Value |
|---|---|
| Success | `oklch(68% 0.08 150)` |
| Warning | `oklch(83% 0.13 95)` |
| Info | `oklch(70% 0.08 200)` |
| Error | `oklch(60% 0.18 25)` |

### Coral variants

Coral is the only color with a full set of shades, because it carries every primary action.

| Purpose | Value |
|---|---|
| Base | `oklch(66% 0.17 35)` |
| Hover (darker fill) | `oklch(60% 0.17 35)` |
| Soft background | `oklch(93% 0.045 35)` |
| Soft hover | `oklch(89% 0.05 35)` |
| Text on soft background | `oklch(50% 0.14 35)` |

---

## 2. Typography

Two fonts, loaded from Google Fonts:

- **Poppins** (500/600/700) — headings and brand
- **Inter** (400/500/600/700) — everything else

| Style | Font / size / weight | Notes |
|---|---|---|
| Display | Poppins 36 / 700 | Marketing-size headline |
| Page heading | Poppins 26 / 700 | Screen titles |
| Section heading | Poppins 19 / 600 | Groups within a screen |
| Body | Inter 16 / 400 | Descriptions and content |
| Secondary | Inter 14 / 500, muted gray | Supporting text |
| Caption | Inter 12 / 600, uppercase, `letter-spacing: 0.06em`, muted gray | Section labels like "UPCOMING CARE" |

---

## 3. Spacing, Radius, Shadow

**Spacing scale (px):** `4 · 8 · 12 · 16 · 24 · 32 · 48` — use these values, don't invent in-between ones.

**Radius (px):**

| Element | Radius |
|---|---|
| Input | 14 |
| Button | 16 |
| Card | 24 |
| Hero image | 32 |
| Avatars, pills, badges | 999 (full round) |

**Shadow** — soft and warm, never harsh:

| Level | Value |
|---|---|
| Card | `0 10px 30px -18px oklch(40% 0.05 60 / 0.4)` |
| List row (subtle) | `0 6px 16px -12px oklch(40% 0.05 60 / 0.4)` |
| Primary button | `0 12px 24px -10px oklch(60% 0.17 35 / 0.6)` |

---

## 4. Components

### Buttons

| Variant | Background | Text | Notes |
|---|---|---|---|
| Primary | Coral | White | One per screen. Padding `14px 26px`, radius 14, Inter 15/700 |
| Secondary | Coral soft `oklch(93% 0.045 35)` | `oklch(50% 0.14 35)` | Same size as primary |
| Ghost | Transparent | Coral | Hover background `oklch(95% 0.02 75)` |
| Icon | Off-white, 1px border | — | 48px circle, holds a 36px icon |

States: **default** → **hover** (darker fill, `oklch(60% 0.17 35)`) → **disabled**
(background `oklch(90% 0.02 60)`, text `oklch(70% 0.02 60)`, `cursor: not-allowed`).

### Cards

Base card: off-white background, radius 24, padding 18, card shadow. Four variants in use:

| Variant | Contents |
|---|---|
| Pet card | 56px circular avatar + name (15/700) + type (12, muted) |
| Activity card | 44px icon tile (radius 14, soft accent background) + activity name + date · note |
| Highlighted card | Coral-soft background, white icon tile — for care that is due |
| Summary card | Caption label + Poppins 26/700 number + muted supporting line |

### Inputs

Padding `12px 14px`, radius 14, border `1.5px solid oklch(88% 0.015 75)`, off-white background,
Inter 14. Label sits above at 12/600 with 6px gap.

| State | Treatment |
|---|---|
| Default | Gray border |
| Focused | Coral border, no browser outline |
| Error | Error border `oklch(60% 0.18 25)`, background `oklch(93% 0.05 25)`, message below in error color at 11/600 |

Field types used: text, select, date, textarea.

### Navigation

- **Mobile** — bottom pill bar, radius 28, four items: Home · Pets · Care · More.
  Active item is coral; inactive is `oklch(78% 0.02 60)`.
- **Desktop** — 200px left sidebar, radius 24: brand, then Home · Pets · Upcoming Care.
  Active item is a coral-soft pill with coral text.

---

## 5. Activity Visual Language

Every care activity uses the same icon and accent color wherever it appears. Icons come from the
`ActivityIcon` component ([DesignSystem/ActivityIcon.dc.html](DesignSystem/ActivityIcon.dc.html)),
which supports exactly four shapes: `cross`, `drop`, `leaf`, `paw`.

| Activity | Icon | Accent | Soft background |
|---|---|---|---|
| Vaccination | `cross` | `oklch(66% 0.17 35)` (coral) | `oklch(93% 0.045 35)` |
| Bath / Grooming | `drop` | `oklch(70% 0.08 200)` (teal) | `oklch(93% 0.03 200)` |
| Flea Treatment | `leaf` | `oklch(72% 0.15 55)` (orange) | `oklch(94% 0.05 55)` |
| Nail Trim | `paw` | `oklch(83% 0.13 95)` (yellow) | `oklch(95% 0.05 95)` |
| General Care | `paw` | `oklch(68% 0.08 150)` (sage) | `oklch(93% 0.035 150)` |

The icon sits in a rounded tile filled with the soft background — 52px/radius 16 in a card,
44px/radius 14 in a list, 32px/radius 10 in a compact row.

Do not assign a new color or icon to an activity in a single screen. If a new activity type is
added, add it to this table first.

---

## 6. Screens

Five mobile screens cover the whole MVP, plus one desktop layout.

| # | Screen | Key elements | Primary action |
|---|---|---|---|
| 1 | Pet Home / Pet List | Greeting, "My Pets" section, pet cards showing name, type · age, and next upcoming care | `+ Add Pet` (dashed row at the end of the list) |
| 2 | Pet Details | Back arrow + name, hero image, name + type · age, Upcoming Care list, Recent Care list | `+ Record Care` (fixed to the bottom) |
| 3 | Record Care Activity | Fields: Pet, Activity Type, Date, Notes, Next Due | `Save Activity` (fixed to the bottom) |
| 4 | Care History | Vertical timeline — circular icon with connector line, then date, activity type, note | — |
| 5 | Upcoming Care | Grouped by This Week / Next Week / Month; each row shows activity, pet name, and date | — |

**Desktop** is the same Pet Details content in a wider layout: sidebar, hero image on the left,
details on the right, Upcoming and Recent side by side. It is a responsive layout, not a
different application.

---

## 7. Empty States

Every list has one. Same shape each time: a round soft-colored circle with a paw icon, a bold
line, and a muted line.

| List | Message | Hint |
|---|---|---|
| Pets | "No pets yet" | "Add your first pet to get started" |
| Care | "No care recorded" | "Record an activity to see it here" |

---

## 8. Using This Document

- Reuse what is here. If a component already exists, do not build a variant of it.
- If something you need is genuinely missing, add it to this file and to the design canvas —
  don't define it inside one screen.
- Behaviour rules (which screen, which action, what the user sees first) live in
  [ux-guidelines.md](ux-guidelines.md). This file covers the visuals.
