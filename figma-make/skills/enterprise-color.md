---
title: "Color"
description: "Guidelines for using color tokens in the Volvo Cars Web Design System."
---

# Color — Skill Guide

## Overview

Color in the Volvo Cars Web Design System is managed through **semantic tokens**, not literal color values. Every token adapts automatically to light and dark modes. Always use tokens—never hard-coded hex/rgb values—to ensure brand consistency and accessibility.

## Token Naming Convention

```
<Property>[-Category]-<Variant>
```

- **Property:** `foreground`, `background`, `surface`, `ornament`, `state`
- **Category** (optional): `accent`, `feedback`
- **Variant:** `primary`, `secondary`, `tertiary`, `inverted`, `green`, `orange`, `red`, `blue`, etc.

Examples:
- `foreground-primary` — default text color
- `surface-feedback-green` — interactive element with green feedback
- `background-secondary` — secondary page/container background

## Token Categories

### Foreground tokens
Use for **text, icons, and borders**.
- `foreground-primary`, `foreground-secondary`, `foreground-tertiary`
- `foreground-inverted`
- `foreground-accent-blue`
- `foreground-feedback-green`, `foreground-feedback-orange`, `foreground-feedback-red`

### Background tokens
Use for **non-interactive regions**: page backgrounds, sections, and content containers.
- `background-primary`, `background-secondary`
- `background-feedback-neutral`, `background-feedback-green`, `background-feedback-orange`, `background-feedback-red`

### Surface tokens
Use for **dynamic or interactive components**: buttons, cards, form controls.
- `surface-primary`, `surface-neutral`, `surface-accent-blue`
- `surface-feedback-green`, `surface-feedback-red`
- `surface-feedback-orange` — **deprecated**, do not use

### Ornament tokens
Use for **purely decorative or grouping elements** such as dividers.
- `ornament-divider`, `ornament-primary`

### State tokens
Use **on top of** existing background or surface colors to indicate interaction state (hover, pressed).
- `state-primary-subtle`, `state-inverted-medium`, etc.

## Critical Rules

### Background usage
- **At least 50% of a page's background must be `background-primary`.** It is the default.
- Use `background-secondary` only to create **depth or a raised level** relative to `background-primary`—never as the dominant background.
- Use page/section background utilities only when they create **clear hierarchy, contrast, or emphasis**.

### Surface-neutral (`bg-surface-neutral`)
- This is a **dark background** designed for **light theme**.
- It must be paired with `foreground-inverted` text (not `foreground-primary`).
- Use it for **interactive elements and components only**—never for large sections or full-page containers.

### Ornament / border rules
- `color-ornament-primary` as a border **must not** be paired with any primitive grey background such as `background-secondary`.
- **Cards or sections using `background-secondary` should not have any ornament or border color.** The raised background level alone provides sufficient visual separation; adding a border on grey creates a muddy, low-contrast appearance.
- Use ornament tokens only on `background-primary` where the border provides meaningful visual grouping.

### Accent colors
- Use accent color tokens **sparingly** to create **meaningful, intentional accent**—not as general-purpose colors.

### Feedback colors
- If a page contains **status, state, or feedback elements**, use the corresponding feedback token family:
  - Background: `background-feedback-green`, `background-feedback-orange`, `background-feedback-red`, `background-feedback-neutral`
  - Foreground: `foreground-feedback-green`, `foreground-feedback-orange`, `foreground-feedback-red`
  - Surface: `surface-feedback-green`, `surface-feedback-red`
  - Border and icon variants as available
- `foreground-feedback-*` on `background-feedback-*` is often **only meant for graphics or icons**, not body text.
- `foreground-feedback-orange` **lacks sufficient contrast** for anything other than graphics on `background-feedback-orange`.

## Recommended Color Pairings

Always stick to these pairings to guarantee AA-level contrast.

| Backdrop | Text colors | Interactive graphics | Interactive surface colors |
| --- | --- | --- | --- |
| `background-primary` | `foreground-primary`, `foreground-secondary`, `foreground-feedback-*` | `foreground-tertiary` | Any `surface-*` |
| `background-secondary` | `foreground-primary`, `foreground-secondary`, `foreground-feedback-*` | `foreground-tertiary` | Any `surface-*` |
| `background-feedback-neutral` | `foreground-primary`, `foreground-secondary` | `foreground-tertiary` | `surface-neutral`, `surface-accent-blue` |
| `background-feedback-green` | `foreground-primary`, `foreground-secondary`, `foreground-feedback-green` | `foreground-tertiary` | `surface-neutral`, `surface-feedback-green` |
| `background-feedback-orange` | `foreground-primary`, `foreground-secondary` | `foreground-tertiary`, `foreground-feedback-orange` | `surface-neutral` |
| `background-feedback-red` | `foreground-primary`, `foreground-secondary`, `foreground-feedback-red` | `foreground-tertiary` | `surface-neutral`, `surface-feedback-red` |
| `surface-neutral` | `foreground-inverted` | – | – |
| `surface-accent-blue` | `always-white` | – | – |
| `surface-feedback-green` | `always-white` | – | – |
| `surface-feedback-red` | `always-white` | – | – |

## Constraints & Warnings

1. **`foreground-tertiary`** is not contrasty enough for normal body text. Use only for **large headings** or **illustrative graphics**.
2. **`ornament-primary`** and `background-*` colors alone lack sufficient contrast to highlight interactive surfaces. Use `surface-*` tokens or additional visual indicators instead.
3. **`surface-feedback-orange`** is **deprecated**. Do not use.
4. **`surface-feedback-neutral`** is the only surface that supports `foreground-inverted` text. All other surface colors require `always-white`.

## Accessibility (WCAG 2.2 AA)

- **Body text:** minimum **4.5:1** contrast ratio against the background.
- **Large text & interactive elements:** minimum **3:1** contrast ratio.
- **Never rely on color alone** to convey meaning. Provide supplementary cues (icons, underlines, labels).
- **Focus states** must have visible outlines meeting contrast requirements.
- All components must be navigable by **keyboard, screen readers, and assistive tools**.

## Dark Mode

All tokens adapt to dark mode automatically. No additional work is needed when using semantic tokens. Dark mode can also be toggled on for specific sections (art direction) where text appears on dark backgrounds.

## Verification Step

Before choosing any background utility class, always verify the class definition and available tokens using:
- `volvo-cars-design-system_get_css_class_definitions`
- `volvo-cars-design-system_search_design_tokens`

This ensures you are using the correct, up-to-date utility and that your pairing meets contrast requirements.

## References

- [Storybook — Colour Tokens](https://developer.volvocars.com/design-system/web/?path=/docs/core-concepts-tokens-colour--docs)
- [Volvo Cars Design Portal — UI Colours](https://design.volvocars.com/colour/ui-colours/)
- [WCAG 2.2 — Contrast (Minimum)](https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum)
- [WCAG 2.2 — Non-text Contrast](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html)
- [WCAG 2.2 — Use of Color](https://www.w3.org/WAI/WCAG21/Understanding/use-of-color.html)