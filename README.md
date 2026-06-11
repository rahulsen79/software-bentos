# AI UX skills

A shared library of reusable skill and instruction files for the AI tools our design team
uses: Figma Make, Notion AI, Copilot, and Lovable.

The job of this repo is simple: store these files in one place so designers reuse them
instead of rewriting the same prompts, and make them easy to add to from a browser.

## How it is organized

One folder per tool. Each tool folder holds that tool's reusable files, in the format that
tool actually reads.

```
design-studio-ai/
├── figma-make/    instructions (Guidelines.md) + skills (SKILL.md)
├── notion-ai/     instructions + skills (.md pages) + agents
└── vibe-coding/   skills for internal vibe-coding tool and repo (ux-playground, vx0)
```

There is no shared folder. A reusable instruction like a house style has to be repackaged
into each tool's own file anyway (a Guidelines.md for Make, a copilot-instructions.md for
Copilot, a Knowledge entry for Lovable), so each tool folder keeps its own copy in the
right format. If you change a rule, update it in each tool folder it applies to.

## Two kinds of file

- **Instructions:** always-on context a tool loads every time (style, naming, the rules
  for trustworthy output). One per tool, usually.
- **Skills / prompts:** one reusable task each, used on demand. Add as many as you like.

## Using a file

Open the tool's folder. Its README tells you exactly where each file goes inside that tool.
Grab the file, drop it in, done.

## Contributing

You do not need an IDE or git. You can add and edit everything from your browser. See
[`CONTRIBUTING.md`](CONTRIBUTING.md).

## Owner

Assign a maintainer before this goes live: `<name>`. They review changes and keep each
tool folder current.

