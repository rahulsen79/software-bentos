---
title: "Spacing & Layout"
description: "Guidelines for spacing, breakpoints, layout grids, containers, page margins, and border radius in the Volvo Cars Web Design System."
---

# Spacing & Layout — Skill Guide

## Overview

The Volvo Cars Web Design System is built on a responsive grid and an 8px spacing scale. All spacing, breakpoints, layout grids, container sizes, and page margins derive from this foundation. Designs should be fluid and adaptable—never pixel-perfect for a single device.

## Spacing

All spacing values are multiples of **8**. There are two types:

- **Fixed** spacers: constant across all breakpoints.
- **Flexible** spacers: scale up with screen size.

| Size | Type | Mobile (sm) | Tablet (md) | Desktop (lg) | Desktop wide (xl) |
| --- | --- | --- | --- | --- | --- |
| `s` | flexible | 32px | 40px | 48px | 72px |
| `m` | flexible | 48px | 56px | 64px | 88px |
| `l` | flexible | 64px | 80px | 96px | 112px |
| `4` | fixed | 4px | 4px | 4px | 4px |
| `8` | fixed | 8px | 8px | 8px | 8px |
| `16` | fixed | 16px | 16px | 16px | 16px |
| `24` | fixed | 24px | 24px | 24px | 24px |
| `32` | fixed | 32px | 32px | 32px | 32px |
| `48` | fixed | 48px | 48px | 48px | 48px |
| `64` | fixed | 64px | 64px | 64px | 64px |

### Spacing rule

**Never combine multiple spacers to achieve a custom value.** Use only the defined spacing tokens.

## Breakpoints

Four breakpoints define the responsive behavior:

| | Mobile | Tablet | Desktop | Desktop wide |
| --- | --- | --- | --- | --- |
| **Name** | `sm` | `md` | `lg` | `xl` |
| **Min width** | 0px | 480px | 1024px | 1600px |
| **Max width** | 479px | 1023px | 1599px | 2560px |
| **Design deliveries** | 375px | 768px | 1280px | 1920px |

Design deliveries use the representative screen widths above to streamline handoff between teams.

## Layout Grid

The system uses a **12-column grid**. On `sm` (mobile), the grid is limited to **8 columns**.

| | Mobile (sm) | Tablet (md) | Desktop (lg) | Desktop wide (xl) |
| --- | --- | --- | --- | --- |
| **Columns** | 8 | 12 | 12 | 12 |

## Containers

A container is a bounding box that sizes content according to grid columns. In code, use the `container` layout class and arrange children with `flex utilities` or `CSS grid`.

### Container sizes

Each size determines how many columns the container spans per breakpoint:

| Container size | Mobile (sm) | Tablet (md) | Desktop (lg) | Desktop wide (xl) |
| --- | --- | --- | --- | --- |
| `max` | 8 | 12 | 12 | 12 |
| `xl` | 8 | 12 | 12 | 12 |
| `lg` | 8 | 12 | 10 | 10 |
| `md` | 8 | 10 | 8 | 8 |
| `sm` | 8 | 8 | 6 | 6 |
| `xs` | 6 | 6 | 4 | 4 |

### Container rules

- **Viewport-independent:** if the viewport is smaller than the container, the container becomes full width.
- **Containers can be nested** (e.g., a parent container for background, a child for content).
- **Never place containers side by side.**

## Page Margins

Default page margins scale with device width. Both code and Figma provide `pagemargin` utility variables.

| Device width | Margin |
| --- | --- |
| 2560px+ | 160px (grows with viewport) |
| 1920px | 132px |
| 1680px | 96px |
| 1280px | 28px |
| 768px | 24px |
| 375px | 24px |

## Border Radius

Use rounded tokens to apply border radius to **non-interactive containers and cards** (informational elements within a page). Three sizes are available:

| Token | Use for | Examples |
| --- | --- | --- |
| `rounded-sm` | Small, compact elements with minimal internal padding | Tags, badges, chips, small inline callouts, input fields, thumbnails |
| `rounded-md` | Medium-sized components and standard content cards | Content cards, info panels, notification banners, list item containers, media cards |
| `rounded-lg` | Large, prominent containers with generous internal spacing | Hero cards, feature sections, large promotional panels, modal dialogs, full-width callout blocks |

### How to choose the right size

Match border radius to the **visual weight and internal padding** of the element:

1. **`rounded-sm`** — Use when the element is **compact and tight**. If the internal padding is small (roughly `8`–`16` spacing tokens), the radius should be small to stay proportional.
2. **`rounded-md`** — Use as the **default for most cards and containers**. This is the most common choice for standard content grouping with moderate padding (`16`–`32` spacing tokens).
3. **`rounded-lg`** — Use when the element is **large and visually prominent**, spanning multiple columns or containing rich content with generous padding (`32`+ spacing tokens). The larger radius reinforces the element's visual importance.

### Border radius rules

- **Scale radius with element size.** A large container with `rounded-sm` looks overly sharp; a small badge with `rounded-lg` looks disproportionate.
- **Be consistent within a page.** Cards at the same hierarchy level should use the same radius token.
- **These tokens are for non-interactive containers and cards.** Interactive component border radii (buttons, form controls, etc.) are defined by the component itself—do not override them with these tokens.
- **Do not use arbitrary border-radius values.** Always use the provided rounded tokens.

## Responsive Design Principles

- There is no "standard" device or viewport. Design for the full range of screen sizes.
- Prioritize user experience over pixel-perfection.
- Use flexible spacers for section-level spacing that should scale with the viewport.
- Use fixed spacers for component-internal spacing that should remain constant.

## Reference

- [viewports.fyi](https://viewports.fyi/) — viewport size data
