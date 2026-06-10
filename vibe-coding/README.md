# Vibe coding

For our Copilot vibe-coding repo and also vx0. Copilot reads instruction and prompt files directly from
a repo's own `.github/` folder, so this folder is the canonical copy you sync into that repo.

## Where these files go
- Design system Web UX playground https://github.com/volvo-cars/web-design-system-ux-playground
- Vx0 prompt agent

## Template for a new skill or prompt

Copy this into a new file and fill it in. Keep it to one task.

```
---
name: short-task-name
description: Use when <the trigger>. Produces <the output>.
---

# Short task name

## Inputs
- What this needs to run.

## Steps
1. The task, in order.
2. Keep it specific. Do not invent component or token names.

## Output
- The exact shape of the result, so it is the same every time.

## Accessibility (optional)
- Optional

## Constraints (optional)
- Optional

## Content & tone rules (optional)
- If the output requires Volvo content writing standard
```

## Files here
