## Overview

Turn a Figma component spec frame into a complete, publish-ready component documentation page in our house style, matching the structure of our **Chips** and **List item** docs. Given a frame image (and optional links/notes), produce a full page with intro, table of contents, anatomy, usage, variants, states, spacing, accessibility, **Examples**, and a **Considerations** section with **Do / Don't** callouts.

**Run this in agent chat (@mention), not the text-selection menu.**

---

## How to run it

1. Open Notion AI chat.
2. Drag in the exported Figma frame (PNG/WebP). One frame per run.
3. Type `@Document a design system web component`.
4. Optionally add in the same message:
    - Component name (if the frame title is unclear)
    - Figma / Storybook / Astro links
    - Notes: which variants, sizes, or states to emphasize or skip
5. Send. The AI creates a new sub-page with the finished doc.

---

## Inputs

**Required**

- One Figma frame image of the component spec.

**Optional**

- Component name override
- Source links (Figma, Storybook, Astro)
- Notes on variants / states / scope

If a required input is missing, ask for it once, then proceed.

---

## Stance: document like a senior design system designer

Do not just transcribe the frame. Reason about the component the way an experienced DS designer would, drawing on established conventions from prominent libraries (Material, Polaris, Carbon, HIG, Fluent, Atlassian) and from the internal reference library below:

- **Recommended**: what comparable components conventionally include but the frame doesn't show (likely states, accessibility minimums, when-not-to-use, content rules). Phrase as a recommendation to confirm, and name the basis where useful (e.g. *"Comparable empty-state patterns in Material and Polaris also define a `disabled` action state. Recommended: confirm in Figma."*).

Prefer a confirmed-to-confirm recommendation over a bare `TBD`: a `TBD` is only acceptable when no sensible convention exists. This tiering is what separates a useful doc from a transcription.

---

## DS reference library: borrow conventions from here

When enriching with Recommended content, prefer our curated internal pages over free recall, and cite them. These are ordinary Notion pages capturing cross-library conventions we have already vetted.

- Interaction states
- Accessibility
- Centenary Theme: Spacing
- **Centenary Theme:** Colors
- **Centenary Theme: Shapes**
- Centenary Theme: Layout
- **Centenary Theme: Typography** 

---

## Reference pages: match this format

Before building, **open and read these pages** and treat them as the canonical format for layout, section order, callout styling, and tone. Mirror their structure and styling; do **not** copy their content. If a rule written below ever conflicts with these pages, follow the pages.

- Button
- Chips
- List item

---

## Step 1: Read the frame (the Observed tier)

Capture what the frame actually shows. This is your ground truth:

- **Component name**: usually the frame's large heading.
- **One-line definition**: what the component helps users do.
- **Anatomy**: labeled parts (numbered callouts, leader lines), and which are optional. Watch for footnotes like "*Title or body is always required."
- **Variants**: distinct named versions (e.g. left-aligned vs centered, with media vs text-only).
- **Sizes**: small / large, with any px values.
- **States**: default, hover, pressed, selected, loading, disabled. Note light **and** dark theme if both are shown.
- **Examples**: any in-context usages pictured, with their captions.
- **Sources**: Figma / Storybook / Astro links visible in the frame's "Sources" area.

If both light and dark themes appear, document states once and note that each is provided in both themes.

After capturing the Observed tier, do a second pass for the **Recommended tier**: for each section, ask what comparable components in the reference library and named public libraries conventionally include that this frame omits, and add it as a confirm-to-recommend note. This is where the design thinking lives.

---

## Step 2: Build the page in this exact order

Include a section **only if** the frame supports it. Mark conditional ones below. Keep the order. Cross-check the result against the **Reference pages** above; the finished page should be indistinguishable in format from them.
Each section (from 3 to 16) including its content should be nested within a Toggle heading 2.

1. **Review banner**: a yellow/amber callout block at the very top of the page (above the title), with ⚠️ emoji, reading:
    
    <aside>
    💡
    
    > ⚠️ **Draft, not published.** This page contains Recommended content that has not been confirmed. Review carefully, confirm each Recommended item against Figma and the source of truth, then delete those notes **and this banner** before publishing.
    > 
    </aside>
    
    This banner exists only during review. It must be the topmost block. See the closing rule below.
    
2. **Title + icon**: H1 with the component name. Set a representative page icon.
3. **Intro line**: one sentence under the title. *"X helps users … "*
4. **Table of content**: a Notion `/Table of contents` block, with toggle heading 3 “Table of content”
5. **Anatomy** *(if parts are labeled)*: numbered list of parts, each tagged `(Optional)` where shown; keep required-field footnotes.
6. **Usage / How to use**: when to use, plus a short "When not to use" list if the frame implies misuse cases.
7. **Variants**: one short subsection per variant, each with a one-line "when to use."
8. **Size** *(if multiple sizes)*: describe each and where it fits; include px values if shown.
9. **States**: list supported states; render the support list as inline `code` chips, e.g. `Default`, `Hover`, `Pressed`, `Disabled`.
10. **Interaction** *(if behavior matters: dismiss, single vs multi-select, loading)*.
11. **Spacing**: gaps and padding, with px values as inline `code`.
12. **Content guidelines** *(if relevant)*: Title / Body / Media / Icon rules as short bullets.
13. **Accessibility / Target size**: minimum target sizes as inline `code` (e.g. `40px`, `56px`).
14. **Examples**: see formatting rules below. Always include this section.
15. **Considerations**: Do / Don't callouts. Always include this section.
16. **Figma**: the Figma link as a `/bookmark` (web bookmark) block.
17. **Storybook** / **Astro**: the source link as a `/bookmark` block.

---

## Section formatting rules

**Table of content**: use the native Table of contents block so it stays in sync. Don't hand-type the list.

**States support line**: after the description, add `Supported states:` followed by each state as inline code, comma-separated.

**Spacing / sizes / px values**: always wrap measurements in inline `code` (`8px`, `16px`, `40px`) so they read as tokens, matching our docs.

**Examples**: for each example:

- Insert an image block (see "Images" below).
- Add a one-line caption directly beneath it as gray/secondary text, phrased as *"Example: …"* (e.g. *"Example: Navigation list item with media, grouped with text-only item."*).
- Group related examples under the variant they illustrate.

**Considerations (Do / Don't)**: this is the most important formatting requirement.

- Create a **Do** group and a **Don't** group.
- Each item is its own **callout block** (one point per callout), so they stack like cards: 
- **Do**: green-background callout, e.g. *"Do use meaningful imagery."* 
- **Don't**: red-background callout, e.g. *"Don't overload body text."*
- Keep each point to one short line. Derive them from the frame's stated rules; if few are explicit, add safe, generic ones consistent with our system (concise labels, consistent media sizing, no decorative-only media, avoid truncation).

**Source links**: Figma, Storybook, and Astro each get their own bookmark block under their own heading, exactly as in the samples.

**Marking Recommended content**: every Recommended item must be visually distinct so a reviewer can find and strip it. Prefix each with **Recommended (confirm):** in bold, or place it in its own gray callout. Never blend a Recommended note into surrounding Observed prose.

**Publishing**: the review banner and all Recommended notes are draft-only scaffolding. The published documentation should contain Observed facts plus the Recommended items the reviewer has confirmed and rewritten as plain statements. Before publishing, the reviewer deletes the top banner and any unconfirmed Recommended notes. Remind the user of this in your chat reply when you finish, not inside the page body.

---

## Content & tone rules

- Sentence case for body; keep headings as named above.
- Labels and chip/option text: short keywords, 1–3 words. Avoid sentences in labels.
- Concise prose: one to three sentences per paragraph.
- Don't pad. If a section would only restate the heading, drop it.
- Use our terminology consistently (e.g. "media," "leading/trailing slot," "state layer," "target size") when the frame uses it.

---

## Images

Chat mode can add images three ways. Prefer them in this order:

1. **Reuse the source frame**: when the frame already contains a clean example or state diagram, insert that image (or the relevant crop) directly into the matching section. This keeps the doc faithful to Figma.
2. **Generate a placeholder illustration**: only when an example needs a neutral visual that isn't in the frame (e.g. an empty-state graphic). Use Notion AI image generation. *Available on paid plans only.* If generation isn't available, insert an empty image block captioned *"Add screenshot from Figma."* instead.
3. **Leave a marked slot**: for in-context screenshots that must come from a real product view, add an empty image block with a caption noting what to drop in.

Every image gets a caption. Never fabricate UI that misrepresents the component.

---

## Quality bar (the skill's own Do / Don't)

✅ Match the section order and house style of the Chips / List item docs.
✅ Use inline `code` for every px value and state name.
✅ Make each Do and each Don't its own colored callout.
✅ Keep Examples captioned and grouped by variant.
✅ Enrich thin frames with Recommended conventions, clearly labeled and cited where possible.

🚫 Don't present a Recommended item as an Observed fact.
🚫 Don't fall back to bare `TBD` when a sensible convention exists; recommend and flag instead.
🚫 Don't write long labels or full sentences inside chips/options/labels.
🚫 Don't merge Do and Don't into one block or skip the colored callouts.
🚫 Don't output a flat wall of text; use headings, callouts, and the ToC block.
