# Evals

This is what separates the repo from every dead "AI prompts" folder on the internet. We do
not *claim* a workflow is reliable. We *show* it on real cases.

## What an eval is here

A small, concrete check that a skill or agent behaves the way its instructions say it
should, run on a real input with a known-good expectation. You do not need a framework.
A folder of cases a human can eyeball is enough to start.

## Structure

```
evals/
├── README.md
└──  cases/        One folder per case: the input, and what good output looks like
```

## A case is

A folder under `cases/` containing:

- `input.*` : the exact input given to the tool (a frame image, a prompt, a spec).
- `expectation.md` : what a correct output must and must not do. Phrase as checks, e.g.
  "must label usage guidance as Recommended (confirm)", "must not invent a token name",
  "must keep the review banner".

## How to run one

1. Load the relevant skill or agent with our standards, exactly as a designer would.
2. Feed it `input.*`.
3. Check the output against `expectation.md`. Note pass/fail.
4. If it fails, the fix usually belongs in the instructions, not the case. Update the
   instruction file, then re-run.

## Which workflows to cover first

Start with the ones that ship most often and cost most when wrong: component
documentation generation, and any agent that can publish without a human in the loop.
