# Notion skill template 2026

**Role:** One sentence describing who the AI is acting as and its area of expertise.

**Input:** One sentence describing what the AI will receive (format, source, expected shape).

**Task:** One sentence describing what the AI should do with the input.

## Output format
Define the exact structure. Pick one:
- Prose / bullets / table - specify which.
- `##` sections - list the exact section names and order.
- Schema - show field names, types, and whether each is required.

If a section has no data in the source, write *“Not captured. Add to follow-up”* and move the gap to **Open questions**.

## Rules (guardrails)

- **Constraint 1** (e.g. don't fabricate).
- **Constraint 2** (e.g. flag uncertainty with ⚠️).
- **Constraint 3** (e.g. Do not answer questions the task did not ask. Move tangents into **Open questions**).
- **Constraint 4** (e.g. Prefer bullets over paragraphs. One idea per bullet).


## Always include an “Open questions” section
At the end of every output, list:
- Gaps in the input.
- Ambiguities you had to flag.
- Items marked *“Not captured”*.
- Anything that needs human follow-up before the result can be trusted.
