---
name: implement-slides
description: How to implement slide decks and presentations in a React environment. Use this skill whenever the user mentions creating presentations, slide decks, or slideshows. It contains all component APIs, best practices, and integration patterns for building interactive presentations.
---

# Reveal.js with React

This skill provides comprehensive instructions for using the official `@revealjs/react` package. The packages (`@revealjs/react`, `reveal.js`, `react`, `react-dom`) are already installed.

## Theming with Volvo Cars Design System (VCDS) [CRITICAL REQUIREMENT]

Default to the Volvo Cars presentation theme from this skill unless the user explicitly asks for another visual direction. Create a `reveal-overrides.css` file and import it after `reveal.css` and `white.css`; Reveal theme variables use `--r-*` CSS custom properties.

### Next.js App Router Integration

Since this project uses the Next.js App Router, import the Reveal.js CSS files and the override file directly in the page or layout that hosts the presentation. Do not import them in the root global layout unless the entire application is a presentation.

### Critical Layout Rule

If the host app layout includes consumer-facing elements such as GTM, cookie consent, dictionaries providers, site navigation, or site footer, remove them for presentation routes. Presentations require a clean layout.

### Dark Mode Support

Volvo Cars presentations must ship with a persisted light/dark toggle. Default to `light` unless the user explicitly asks for `dark` as the initial mode.

- Keep `white.css` imported. Do not swap Reveal theme CSS files to implement dark mode.
- Keep the root `data-color-mode` attribute as the source of truth for the presentation theme.
- Seed the initial `data-color-mode` value from a cookie in the host route shell.
- Mount a single toggle outside the slide content, typically in the top-right of the presentation shell, so it does not affect slide geometry.
- Use `IconButton` with `variant="clear"` and `sun` / `moon` icons.
- On toggle, update `document.documentElement.dataset.colorMode` and persist the new mode in a cookie.
- Restore the cookie-backed mode on refresh.
- Copy the inline `app/presentation/presentationTheme.ts` and `app/presentation/PresentationThemeToggle.tsx` source from this skill and update `app/layout.tsx` using the inline example below.

```tsx
// app/page.tsx
import type { Metadata } from 'next';
import 'reveal.js/reveal.css';
import 'reveal.js/theme/white.css';
import { PresentationDeck } from './presentation/PresentationDeck';
import './presentation/reveal-overrides.css';

export default function Index() {
  return <PresentationDeck />;
}

export const metadata: Metadata = {
  title: 'Vx0 Presentation',
};
```

Use this exact override file as the baseline for Volvo Cars slide decks:

```css
/* app/presentation/reveal-overrides.css */
:root {
  --r-background: var(--v-color-background-primary);
  --r-background-color: var(--v-color-background-primary);
  --r-main-font: var(--v-font-sans-family);
  --r-main-font-size: var(--v-font-24-size);
  --r-main-color: var(--v-color-foreground-primary);
  --r-block-margin: var(--v-space-24);
  --r-heading-margin: 0 0 var(--v-space-24) 0;
  --r-heading-color: var(--v-color-foreground-primary);
  --r-heading-line-height: var(--v-font-heading-1-lineheight, 1.2);
  --r-heading-letter-spacing: normal;
  --r-heading-text-transform: none;
  --r-heading-text-shadow: none;
  --r-heading-font-weight: var(--v-font-regular-weight);
  --r-heading1-text-shadow: none;
  --r-heading-font: var(--v-font-sans-family);
  /* Actual max values: heading-1 = 2.5rem (40px), heading-2 = 2rem (32px), heading-3 = 2rem (32px). */
  --r-heading1-size: var(--v-font-heading-1-size-max);
  --r-heading2-size: var(--v-font-heading-2-size-max);
  --r-heading3-size: var(--v-font-heading-3-size-max);
  --r-heading4-size: var(--v-font-24-size);
  --r-code-font: var(--v-font-mono-family);
  --r-link-color: var(--v-color-foreground-accent-blue);
  --r-link-color-dark: var(--v-color-foreground-accent-blue);
  --r-link-color-hover: var(--v-color-foreground-accent-blue);
  --r-selection-background-color: var(--v-color-surface-accent-blue);
  --r-selection-color: var(--v-color-foreground-inverted);
  --r-overlay-element-bg-color: var(--v-color-surface-neutral);
  --r-overlay-element-fg-color: var(--v-color-foreground-primary);
}

html {
  scrollbar-gutter: auto !important;
}

.reveal {
  color: var(--r-main-color);
  background: var(--r-background-color);
}

.reveal .slides {
  text-align: start;
}

.reveal .slides section {
  height: 100%;
  min-height: 100%;
  padding: 0;
}

.reveal .controls {
  color: var(--v-color-foreground-primary);
}

.reveal .progress {
  height: var(--v-space-4);
  color: var(--v-color-foreground-primary);
  background: color-mix(
    in srgb,
    var(--v-color-foreground-primary) 18%,
    transparent
  );
}

.reveal .slide-number {
  color: var(--v-color-foreground-primary);
  background: color-mix(
    in srgb,
    var(--v-color-background-primary) 80%,
    transparent
  );
  border: 1px solid var(--v-color-ornament-primary);
}

.reveal ::selection {
  color: var(--r-selection-color);
  background: var(--r-selection-background-color);
}

:fullscreen .reveal .controls,
:fullscreen .presentation-theme-toggle,
:-webkit-full-screen .reveal .controls,
:-webkit-full-screen .presentation-theme-toggle,
:-moz-full-screen .reveal .controls,
:-moz-full-screen .presentation-theme-toggle,
:-ms-fullscreen .reveal .controls,
:-ms-fullscreen .presentation-theme-toggle {
  display: none !important;
}
```

## Styling & Layout Rules

Prefer Volvo Cars Design System utilities and components. Use a shared CSS module only for template geometry and micro-layout that the utility classes cannot express cleanly.

### App-Specific Rules

- Keep `white.css` imported. It is required for this theme.
- Keep `reveal-overrides.css` as a separate global stylesheet. Do not merge it into a CSS module.
- Create or update the shared `app/presentation/*` files from this skill and copy only the slide components you use from `references/components/*.md`.
- The persisted presentation theme toggle belongs in the presentation shell or route wrapper, not inside individual slide components.
- Use Volvo utility classes for layout, spacing, borders, colors, and typography first.
- When using large `statement-*` utilities for visual titles or statements, do not use heading elements. Use a non-heading element, preferably `<span>`.
- Use heading elements for regular heading-scale content only. Apply `heading-*` utilities directly on them.
- Use `next/image` for image slides.
- If team members can repeat the same placeholder name and title, provide an `id` field or copy the key-deduplication pattern from `TeamSlide`.

### Reveal.js Typography Interaction

Reveal applies default heading styling to heading elements. If you need large display typography with `statement-*`, use `<span>` or `<p>` so the Volvo utility class stays in control.

Good:

```tsx
<span className="m-0 text-primary statement-1">Title</span>
```

Avoid:

```tsx
<h1 className="m-0 text-primary statement-1">Title</h1>
```

Use heading elements only for regular heading-scale content:

```tsx
<h2 className="m-0 text-primary heading-3">Timeline</h2>
```

## Core Components

The `@revealjs/react` package provides several components to build decks declaratively:

- `<Deck>`: The main wrapper. It initializes Reveal.js on mount and destroys it on unmount.
- `<Slide>`: Represents a single slide (`<section>`).
- `<Stack>`: Wrap multiple `<Slide>` components to create vertical slides.
- `<Fragment>`: Reveal content incrementally.
- `<Code>`: Render syntax-highlighted code.
- `<Markdown>`: Render Reveal-compatible markdown.

### Basic Deck Example

```jsx
import { Deck, Slide, Stack } from '@revealjs/react';
import 'reveal.js/reveal.css';
import 'reveal.js/theme/white.css';
import './reveal-overrides.css';

export function Presentation() {
  return (
    <Deck>
      <Slide>
        <h1 className="heading-1">Main Title</h1>
        <p className="text-secondary">Introduction slide.</p>
      </Slide>

      {/* Vertical slides are wrapped in a Stack */}
      <Stack>
        <Slide>
          <h2 className="heading-2">Vertical 1</h2>
          <p className="text-secondary">Use a second slide when needed.</p>
        </Slide>
        <Slide>
          <h2 className="heading-2">Vertical 2</h2>
        </Slide>
      </Stack>
    </Deck>
  );
}
```

## Fragments

Use `<Fragment>` to reveal content step by step. By default, it renders as a `<span>`.

- Use the `as` prop to change the HTML element.
- Use `asChild` to merge the fragment behavior onto an existing child element.
- Use the `animation` prop for styles such as `fade-up` or `highlight-red`.
- Use the `index` prop to control the exact order.

```jsx
import { Fragment } from '@revealjs/react';

<Fragment as="p" animation="fade-up">First point</Fragment>
<Fragment asChild index={2}>
  <p>Second point</p>
</Fragment>
```

## Syntax Highlighted Code

Use the `<Code>` component to show code snippets.

- For request payloads, schemas, JSON, CLI examples, and code-heavy content in this presentation theme, prefer `<Code>` over a hand-rolled `pre > code` block.
- Import a Reveal highlight theme such as `reveal.js/plugin/highlight/monokai.css`.
- Pass the highlight plugin to `<Deck plugins={[RevealHighlight]}>`.
- Use `language`, `lineNumbers`, and `startFrom` as needed.

```jsx
import { Deck, Slide, Code } from '@revealjs/react';
import 'reveal.js/plugin/highlight/monokai.css';
import RevealHighlight from 'reveal.js/plugin/highlight';

export function Presentation() {
  return (
    <Deck plugins={[RevealHighlight]}>
      <Slide>
        <Code language="javascript" lineNumbers="1|2-3">
          {`const a = 1;
const b = 2;
const c = a + b;`}
        </Code>
      </Slide>
    </Deck>
  );
}
```

## Markdown

Use `<Markdown>` to write slides directly in markdown.

- Supports `separator="^\n---\n$"` and `verticalSeparator="^\n--\n$"`.
- Notes can be added using `Notes:`.
- Use `<Markdown src="/slides/content.md" />` to load external markdown.

## Configuration & Events

For Volvo Cars presentation decks in this app, use this default configuration:

```jsx
<Deck
  config={{
    width: 1920,
    height: 1080,
    controls: true,
    hash: true,
    center: false,
    margin: 0,
    progress: false,
    slideNumber: false,
  }}
  onSlideChange={(event) => console.log(event.indexh, event.indexv)}
/>
```

Pass a `config` object to `<Deck>` to configure Reveal.js options. Listen to events on the `<Deck>` component such as `onReady` and `onSlideChange`.

## Slide Attributes

`<Slide>` accepts standard Reveal.js `data-*` attributes via camelCase props for convenience:

- Backgrounds: `background`, `backgroundColor`, `backgroundImage`, `backgroundVideo`, `backgroundIframe`, `backgroundGradient`
- Auto-animate: `autoAnimate`, `autoAnimateId`, `autoAnimateDuration`
- Transitions: `transition`, `transitionSpeed`

## API Access

To control the deck programmatically from within the deck tree, use `useReveal()`. To access it from outside the deck tree, pass a `deckRef` to `<Deck>`.

```jsx
import { useReveal } from '@revealjs/react';

function NextButton() {
  const deck = useReveal();
  return <button onClick={() => deck?.next()}>Next slide</button>;
}
```

## Volvo Cars Slides Theme

Use this section when you need to recreate the Volvo Cars presentation template in another app. Shared files stay inline in this skill. Slide-specific source files live under `references/components/*.md`; copy only the `tsx` code blocks you need into `app/presentation/`.

### File Structure

If you are scaffolding a new presentation route, create these shared files:

- `app/page.tsx`
- `app/presentation/reveal-overrides.css`
- `app/presentation/volvo-cars-template.module.css`
- `app/presentation/PresentationDeck.tsx`
- `app/presentation/PresentationSlideFrame.tsx`
- `app/presentation/PresentationStandardSlide.tsx`
- `app/presentation/SlideHeader.tsx`
- `app/presentation/SlideFooter.tsx`
- `app/presentation/presentationTheme.ts`
- `app/presentation/PresentationThemeToggle.tsx`

If the route or deck already exists, reuse it and add only any missing shared files.

Create slide component files only as needed:

- For any slide component you use, copy the `tsx` code block from `references/components/<ComponentName>.md` to `app/presentation/<ComponentName>.tsx`.

### Read Order

- Start with the shared files in this skill. If you are scaffolding a new route, use `app/page.tsx` and `app/presentation/PresentationDeck.tsx`; otherwise reuse the existing route and deck.
- Copy `app/presentation/presentationTheme.ts` and `app/presentation/PresentationThemeToggle.tsx`, then apply the `app/layout.tsx` pattern from this skill to the existing host shell.
- Then read only the component refs you need from `references/components/*.md`.
- If you copy `PresentationDeck.tsx` unchanged, also create every slide component file it imports.

### Implementation Rules

- Most slides use `PresentationStandardSlide`; `CoverSlide` uses `PresentationSlideFrame`; `ClosingWordmarkSlide` uses neither.
- CRITICAL: always prefer the existing Volvo Cars presentation components instead of inventing new ones. This is the default path for maintaining the expected theme, rhythm, and visual language. If the raw content does not fit cleanly, first rewrite, regroup, split, shorten, retitle, or drop non-essential detail so it can pass through the appropriate existing components. Only create a custom layout as a last resort when the existing components genuinely cannot express the content clearly.
- Do not use source copy verbatim for titles, chapter titles, or `statement-*` slots. Treat source text as unsuitable for those slots if it is instructional, procedural, or meta, if it reads like a full sentence, exceeds 5 words for `statement-*` or 6 words for headings, or would wrap beyond two short lines. Rewrite it into a short audience-facing phrase, move supporting detail into body copy, or omit it if it is only process text.
- CRITICAL: after implementation is complete, review the full deck content one more time before finishing. Tighten the story flow, remove filler words, remove repeated ideas, and remove secondary detail that weakens readability so each slide earns its place in the sequence.
- Do not invent dashboard-like panel grids for scope, method, caveats, or summary slides. If the content needs multiple substantial blocks of copy, split it across slides or switch to an existing slide component.
- For scope, method, synthesis, or summary content with several short factual points, prefer `NumberedColumnsSlide` or `ThreeColumnNumbersSlide` before inventing custom panel layouts, but only when the content naturally fits them. Do not force repeated use of number slides across a deck just because they are available.
- Do not use scrollable slide bodies to force content to fit. Split oversized or multi-idea content into multiple slides or a vertical `Stack`.
- Use Reveal `Code` for code, request payloads, and schemas instead of inventing custom `pre > code` blocks.
- Do not add box shadows, elevated cards, or custom depth effects unless explicitly requested; use flat surfaces, borders, spacing, and typography.
- Do not rely on colorful borders such as `border-accent-blue` to create emphasis for blocks or panels. Where a block needs separation, prefer a secondary surface/background first, then neutral borders, spacing, and typography. Use accent styling only if the user explicitly asks for it.
- Ad hoc `Panel` or `article` blocks are for short callouts only. Do not put long bullet lists inside them, and avoid `h-full` on custom panels unless every sibling block is intentionally short and visually balanced.
- Use `SlideHeader` and `SlideFooter` via the shared frame components.
- Keep template geometry in `volvo-cars-template.module.css` and `reveal-overrides.css` separate and global.
- Reuse stable footer data such as `baseFooter` and `pagedFooter`.
- Use readonly arrays and tuples where possible.
- Implement the presentation theme toggle in the host layout or presentation shell, not inside individual slides.
- Maintain deck rhythm and variation. Do not repeat the same slide pattern many times in a row when another existing component would communicate the next point more clearly.

### Slide Component Selection

Reference files follow `references/components/<ComponentName>.md`.

- `CoverSlide`: Opening cover with a stacked large title and intro in a padded full-body column. Uses `PresentationSlideFrame`, not `PresentationStandardSlide`; preserve `bodyClassName="flex-grow p-24 px-32 stack-32"` and the intro paragraph's `data-fluid-typography="max"`.
- `SectionIntroSlide`: Section divider with a large chapter title near the top and a narrower intro block below; keep `chapterIntroSlot` and a non-heading `statement-1` title.
- `NumberedColumnsSlide`: Four to six equal-width numbered columns with dividers. Prefer this for scope, method, findings, or summary slides only when each point can stay short and the slide benefits from direct side-by-side comparison.
- `ThreeColumnNumbersSlide`: Three equal numeric columns with oversized numbers; the number uses `statement-1` with fluid typography. Prefer this over custom panel grids only when the story is exactly three concise points and that repeated numeric rhythm strengthens the sequence.
- `TimelineSlide`: Title and intro above an absolute horizontal track with evenly spaced milestones; preserve `timelineTopRow`, `timelineIntro`, `timelineTrack`, `timelinePoint`, `timelineHeadingBlock`, `timelineCopyBlock`, `timelineLineMarker`, and `timelineDotMarker`. The first item uses a line marker, later items use dots, and a single item centers instead of spacing out.
- `StatementSidebarSlide`: Large statement left, spacer gap center, bordered copy right; preserve `contentHeadingSlot`, `contentDividerGap`, `contentBodySlot`, and a non-heading `statement-1` title.
- `ThreeColumnTextSlide`: Heading row above three equal text columns; expects a three-string tuple.
- `StatementSurfaceSlide`: Lead statement left and shaded content surface right; preserve `surfaceLeadSlot`, `contentDividerGap`, `surfacePanel`, and `surfaceBodySlot`.
- `CenteredStatementSlide`: Single centered statement; use `chapterTitleSlot` and a non-heading `statement-3`.
- `TeamSlide`: Title above a centered team grid; preserve `teamTitleRow`, `teamMembersArea`, `teamMember`, and `teamMemberCompact`. Over five members it switches to a compact two-row layout and dedupes keys by `id` then `name/title`.
- `ImageTextSlide`: Copy left, image right; preserve `imageTextCopyColumn`, `imageTextBodyRow`, and `imageTextMediaColumn`. The media column must stay `relative overflow-hidden` for `next/image` `fill`.
- `TwoColumnImageTextSlide`: Title row above two image/text columns; preserve `twoImageSlideTitleRow`, `twoImageSlideColumn`, `twoImageSlideMedia`, and `twoImageSlideTextBlock`. It expects exactly two items and uses asymmetric text padding.
- `ClosingWordmarkSlide`: Centered Volvo wordmark on a full primary background.

### Copy These Shared Files

#### `app/presentation/PresentationSlideFrame.tsx`

```tsx
import type { ReactNode } from 'react';

type PresentationSlideFrameProps = {
  children: ReactNode;
  footer: ReactNode;
  header?: ReactNode;
  bodyClassName?: string;
};

export function PresentationSlideFrame({
  children,
  footer,
  header,
  bodyClassName = 'flex-grow',
}: PresentationSlideFrameProps) {
  return (
    <div className="relative h-full w-full overflow-hidden bg-primary">
      <div className="absolute top-24 end-24 bottom-24 start-24 z-default flex flex-col">
        {header}
        <div className={bodyClassName}>{children}</div>
        {footer}
      </div>
    </div>
  );
}
```

#### `app/presentation/PresentationStandardSlide.tsx`

```tsx
import type { ReactNode } from 'react';
import { PresentationSlideFrame } from './PresentationSlideFrame';
import { SlideFooter, type SlideFooterProps } from './SlideFooter';
import { SlideHeader } from './SlideHeader';

type PresentationStandardSlideProps = {
  headerTitle: string;
  footer: SlideFooterProps;
  children: ReactNode;
  bodyClassName?: string;
};

export function PresentationStandardSlide({
  headerTitle,
  footer,
  children,
  bodyClassName,
}: PresentationStandardSlideProps) {
  return (
    <PresentationSlideFrame
      header={<SlideHeader title={headerTitle} />}
      footer={<SlideFooter {...footer} className="border-t border-ornament" />}
      bodyClassName={bodyClassName}
    >
      {children}
    </PresentationSlideFrame>
  );
}
```

#### `app/presentation/SlideHeader.tsx`

```tsx
import styles from './volvo-cars-template.module.css';

type SlideHeaderProps = {
  title: string;
};

export function SlideHeader({ title }: SlideHeaderProps) {
  return (
    <header className="flex h-64 items-center border-b border-ornament px-4">
      <div className={`${styles.headerSlot} flex items-center px-32`}>
        <p className="m-0 text-primary font-16">{title}</p>
      </div>
    </header>
  );
}
```

#### `app/presentation/SlideFooter.tsx`

```tsx
import { Wordmark } from '@volvo-cars/react-icons';
import styles from './volvo-cars-template.module.css';

export type SlideFooterProps = {
  title: string;
  owner: string;
  company: string;
  securityClass: string;
  date: string;
  pageNumber?: string;
  showPageNumber?: boolean;
  className?: string;
};

export function SlideFooter({
  title,
  owner,
  company,
  securityClass,
  date,
  pageNumber = '',
  showPageNumber = false,
  className,
}: SlideFooterProps) {
  const footerClassName = [
    'flex h-64 items-center text-secondary font-16',
    className,
  ]
    .filter(Boolean)
    .join(' ');

  return (
    <footer className={footerClassName}>
      <div className={`${styles.logoSlot} flex h-64 items-center px-32`}>
        <Wordmark className="block scale-110" height={8} />
      </div>

      <div className={`${styles.infoRow} flex h-64 items-center gap-8`}>
        <span className="whitespace-nowrap">{title}</span>
        <span className="whitespace-nowrap">|</span>
        <span className="whitespace-nowrap">{owner}</span>
        <span className="whitespace-nowrap">|</span>
        <span className="whitespace-nowrap">{company}</span>
        <span className="whitespace-nowrap">|</span>
        <span className="whitespace-nowrap">Security class:</span>
        <span className="whitespace-nowrap">{securityClass}</span>
      </div>

      <div className="flex h-64 items-center px-24">
        <span className="whitespace-nowrap">{date}</span>
      </div>

      <div
        className={`${styles.pageSlot} flex h-64 items-center justify-end px-32`}
      >
        {showPageNumber ? (
          <span className="whitespace-nowrap">{pageNumber}</span>
        ) : null}
      </div>
    </footer>
  );
}
```

Copy these files as part of the Volvo Cars presentation shell.

#### `app/presentation/presentationTheme.ts`

```tsx
export const PRESENTATION_COLOR_MODE_COOKIE_NAME = 'presentation-color-mode';

export type PresentationColorMode = 'light' | 'dark';

export const DEFAULT_PRESENTATION_COLOR_MODE: PresentationColorMode = 'light';

export function getPresentationColorMode(
  value?: string | null,
): PresentationColorMode {
  return value === 'dark' ? 'dark' : DEFAULT_PRESENTATION_COLOR_MODE;
}
```

#### `app/presentation/PresentationThemeToggle.tsx`

```tsx
/** biome-ignore-all lint/suspicious/noDocumentCookie: it's ok */
'use client';

import { IconButton } from '@volvo-cars/react-icons';
import { useEffect, useState } from 'react';
import {
  PRESENTATION_COLOR_MODE_COOKIE_NAME,
  type PresentationColorMode,
} from './presentationTheme';

const COLOR_MODE_COOKIE_MAX_AGE = 60 * 60 * 24 * 365;

type PresentationThemeToggleProps = {
  initialColorMode: PresentationColorMode;
};

export function PresentationThemeToggle({
  initialColorMode,
}: PresentationThemeToggleProps) {
  const [colorMode, setColorMode] = useState(initialColorMode);

  useEffect(() => {
    document.documentElement.dataset.colorMode = colorMode;
  }, [colorMode]);

  const nextColorMode = colorMode === 'light' ? 'dark' : 'light';

  const handleClick = () => {
    document.cookie = `${PRESENTATION_COLOR_MODE_COOKIE_NAME}=${nextColorMode}; path=/; max-age=${COLOR_MODE_COOKIE_MAX_AGE}; SameSite=Lax`;
    setColorMode(nextColorMode);
  };

  return (
    <div className="presentation-theme-toggle fixed top-24 end-24 z-overlay">
      <IconButton
        icon={colorMode === 'light' ? 'moon' : 'sun'}
        aria-label={`Switch to ${nextColorMode} mode`}
        aria-pressed={colorMode === 'dark'}
        variant="clear"
        onClick={handleClick}
      />
    </div>
  );
}
```

#### `app/layout.tsx` theme toggle wiring example

```tsx
import { BuildMeta } from '@vcc-package/build-meta';
import { links } from '@volvo-cars/css/links';
import { FavIcons } from '@volvo-cars/favicons/react';
import { getMarketSite } from '@volvo-cars/market-sites';
import { cookies } from 'next/headers';
import type React from 'react';
import { PresentationThemeToggle } from './presentation/PresentationThemeToggle';
import {
  getPresentationColorMode,
  PRESENTATION_COLOR_MODE_COOKIE_NAME,
} from './presentation/presentationTheme';

export default async function Layout({
  children,
}: {
  children: React.ReactNode;
}) {
  const siteSlug = 'uk';

  const { htmlLanguage, languageDirection } = getMarketSite(siteSlug);
  const colorMode = getPresentationColorMode(
    (await cookies()).get(PRESENTATION_COLOR_MODE_COOKIE_NAME)?.value,
  );

  return (
    <html
      lang={htmlLanguage}
      dir={languageDirection}
      data-theme="centenary"
      data-color-mode={colorMode}
    >
      <head>
        <FavIcons />
        <BuildMeta projectName="volvableapp" />
        {links().map((link) => (
          <link key={link.rel + link.href} {...link} />
        ))}
      </head>
      <body className="m-0 bg-primary">
        <PresentationThemeToggle initialColorMode={colorMode} />
        {children}
      </body>
    </html>
  );
}
```

#### `app/presentation/volvo-cars-template.module.css`

```css
/* stylelint-disable volvo-cars/design-system-spacing */
.headerSlot {
  width: 31.25%;
  max-width: min(100%, 36.5625rem);
}

.logoSlot {
  width: 18.75%;
  max-width: min(100%, 21.9375rem);
}

.chapterTitleSlot {
  width: 75%;
  max-width: min(100%, 87.75rem);
}

.chapterIntroSlot {
  width: 56.25%;
  max-width: min(100%, 65.8125rem);
  margin-top: calc(var(--v-space-24) + var(--v-space-2));
}

.contentHeadingSlot {
  width: 56.25%;
  max-width: min(100%, 65.8125rem);
}

.contentDividerGap {
  flex-shrink: 0;
  width: 6.25%;
  max-width: 7.3125rem;
}

.contentBodySlot {
  width: 37.5%;
  max-width: min(100%, 43.875rem);
}

.surfaceLeadSlot {
  width: 56.25%;
  max-width: min(100%, 65.8125rem);
}

.surfacePanel {
  flex: 1 1 0;
  min-width: 0;
  max-width: 45.375rem;
}

.surfaceBodySlot {
  width: calc(100% - var(--v-space-24));
  max-width: min(100%, 43.875rem);
}

.imageTextCopyColumn {
  width: 31.25%;
  max-width: min(100%, 36.5625rem);
}

.imageTextBodyRow {
  width: 100%;
  max-width: 32.5rem;
}

.imageTextMediaColumn {
  flex: 1 1 0;
  min-width: 0;
  max-width: 74.625rem;
}

.twoImageSlideTitleRow {
  width: 31.25%;
  max-width: min(100%, 36.5625rem);
}

.twoImageSlideColumn {
  flex: 1 1 0;
  min-width: 0;
  max-width: 55.5rem;
}

.twoImageSlideMedia {
  width: 100%;
  aspect-ratio: 74 / 43;
}

.twoImageSlideTextBlock {
  width: 79.0541%;
  max-width: min(100%, 43.875rem);
}

.infoRow {
  width: 62.5%;
  min-width: 0;
  max-width: min(100%, 73.125rem);
}

.pageSlot {
  flex-shrink: 0;
  width: 6.25%;
  max-width: 7.3125rem;
  padding-block: calc(var(--v-space-8) + var(--v-space-2));
}

.timelineTopRow {
  top: 2.6875rem;
}

.timelineIntro {
  width: 37.5%;
  max-width: min(100%, 43.875rem);
}

.timelineTrack {
  top: calc(21.875rem + 6.0625rem + var(--v-space-4));
  height: 1px;
  background: var(--v-color-foreground-primary);
}

.timelineTrack::after {
  position: absolute;
  top: calc(var(--v-space-2) * -1);
  inset-inline-end: 0;
  width: calc(var(--v-space-4) + var(--v-space-2));
  height: calc(var(--v-space-4) + var(--v-space-2));
  content: '';
  border-inline-end: 1px solid var(--v-color-foreground-primary);
  border-top: 1px solid var(--v-color-foreground-primary);
  transform: rotate(45deg);
  transform-origin: center;
}

.timelinePoint {
  top: 21.875rem;
  width: 11.1111%;
  max-width: 13rem;
}

.timelineHeadingBlock {
  padding-block: calc(var(--v-space-32) + var(--v-space-8));
}

.timelineCopyBlock {
  padding-block: calc(var(--v-space-32) + var(--v-space-8));
  padding-inline: calc(var(--v-space-12) + var(--v-space-2));
}

.timelineLineMarker,
.timelineDotMarker {
  top: 6.0625rem;
}

.timelineLineMarker {
  inset-inline-start: 0;
  width: 1px;
  height: var(--v-space-8);
  background: var(--v-color-foreground-primary);
}

.timelineDotMarker {
  inset-inline-start: calc(var(--v-space-4) * -1);
  width: var(--v-space-8);
  height: var(--v-space-8);
  background: var(--v-color-foreground-primary);
  border-radius: var(--v-radius-full);
}

.teamTitleRow {
  width: 43.9103%;
  max-width: min(100%, 51.375rem);
}

.teamMembersArea {
  width: 87.5%;
  max-width: min(100%, 102.375rem);
}

.teamMember {
  width: 15.873%;
  max-width: 16.25rem;
}

.teamMemberCompact {
  width: 12.6984%;
  max-width: 13rem;
}
```

#### `app/presentation/PresentationDeck.tsx`

```tsx
'use client';

import { Deck, Slide } from '@revealjs/react';
import { SectionIntroSlide } from './SectionIntroSlide';
import { StatementSidebarSlide } from './StatementSidebarSlide';
import { StatementSurfaceSlide } from './StatementSurfaceSlide';
import { CoverSlide } from './CoverSlide';
import { ClosingWordmarkSlide } from './ClosingWordmarkSlide';
import { ImageTextSlide } from './ImageTextSlide';
import { NumberedColumnsSlide } from './NumberedColumnsSlide';
import type { SlideFooterProps } from './SlideFooter';
import { CenteredStatementSlide } from './CenteredStatementSlide';
import { TeamSlide } from './TeamSlide';
import { ThreeColumnNumbersSlide } from './ThreeColumnNumbersSlide';
import { ThreeColumnTextSlide } from './ThreeColumnTextSlide';
import { TimelineSlide } from './TimelineSlide';
import { TwoColumnImageTextSlide } from './TwoColumnImageTextSlide';

const deckConfig = {
  width: 1920,
  height: 1080,
  controls: true,
  hash: true,
  center: false,
  margin: 0,
  progress: false,
  slideNumber: false,
};

const baseFooter = {
  title: 'Title',
  owner: 'Full name, Department',
  company: 'Volvo Cars',
  securityClass: 'Proprietary',
  date: 'YYYY.MM.DD',
  pageNumber: 'XX',
  showPageNumber: false,
} satisfies SlideFooterProps;

const pagedFooter = {
  ...baseFooter,
  showPageNumber: true,
} satisfies SlideFooterProps;

const shortCopy = 'Short sample copy.';
const mediumCopy = 'Medium sample copy for layout testing.';
const longCopy =
  'Longer sample copy that wraps across lines to test spacing and rhythm.';
const sampleIntro = 'Short intro copy.';
const sampleStatement = 'Short statement copy.';

const numbersItems = [
  {
    number: '01',
    heading: 'Short point',
    body: shortCopy,
  },
  {
    number: '02',
    heading: 'Standard content',
    body: mediumCopy,
  },
  {
    number: '03',
    heading: 'Longer heading that wraps',
    body: mediumCopy,
  },
  {
    number: '04',
    heading: 'Minimal',
    body: shortCopy,
  },
  {
    number: '05',
    heading: 'Balanced content',
    body: longCopy,
  },
  {
    number: '06',
    heading: 'Upper range example',
    body: longCopy,
  },
] as const;

const threeColumnNumbersItems = [
  {
    number: '01',
    heading: 'Heading',
    body: mediumCopy,
  },
  {
    number: '02',
    heading: 'Heading',
    body: mediumCopy,
  },
  {
    number: '03',
    heading: 'Heading',
    body: mediumCopy,
  },
] as const;

const timelineItems = Array.from({ length: 5 }, () => ({
  heading: 'Heading',
  body: shortCopy,
})) satisfies readonly { heading: string; body: string }[];

const textColumns = [mediumCopy, mediumCopy, mediumCopy] as const;

const teamMembers = Array.from({ length: 7 }, () => ({
  name: 'Name Surname',
  title: 'Title / Org',
})) satisfies readonly { name: string; title: string }[];

const twoImageItems = [
  {
    imageSrc: '/presentation/slide-image-text-2.png',
    imageAlt: 'Sample exterior image',
    title: 'Header',
    body: mediumCopy,
  },
  {
    imageSrc: '/presentation/slide-image-x2-right.png',
    imageAlt: 'Sample interior image',
    title: 'Header',
    body: mediumCopy,
  },
] as const;

export function PresentationDeck() {
  return (
    <Deck config={deckConfig}>
      <Slide>
        <CoverSlide title="Title" intro={sampleIntro} footer={baseFooter} />
      </Slide>
      <Slide>
        <SectionIntroSlide
          headerTitle="Title / Chapter"
          title="Chapter"
          intro={sampleIntro}
          footer={pagedFooter}
        />
      </Slide>
      <Slide>
        <NumberedColumnsSlide
          headerTitle="Title / Chapter"
          footer={pagedFooter}
          items={numbersItems}
        />
      </Slide>
      <Slide>
        <ThreeColumnNumbersSlide
          headerTitle="Title / Chapter"
          footer={pagedFooter}
          items={threeColumnNumbersItems}
        />
      </Slide>
      <Slide>
        <TimelineSlide
          headerTitle="Title / Chapter"
          title="Timeline"
          intro={mediumCopy}
          footer={pagedFooter}
          items={timelineItems}
        />
      </Slide>
      <Slide>
        <StatementSidebarSlide
          headerTitle="Title / Chapter"
          title="Heading"
          body={mediumCopy}
          footer={pagedFooter}
        />
      </Slide>
      <Slide>
        <ThreeColumnTextSlide
          headerTitle="Title / Chapter"
          title="Heading"
          columns={textColumns}
          footer={pagedFooter}
        />
      </Slide>
      <Slide>
        <StatementSurfaceSlide
          headerTitle="Title / Chapter"
          lead={sampleStatement}
          body={mediumCopy}
          footer={pagedFooter}
        />
      </Slide>
      <Slide>
        <CenteredStatementSlide
          headerTitle="Title / Chapter"
          statement={sampleStatement}
          footer={pagedFooter}
        />
      </Slide>
      <Slide>
        <TeamSlide
          headerTitle="Title / Chapter"
          title="Team"
          members={teamMembers}
          footer={pagedFooter}
        />
      </Slide>
      <Slide>
        <ImageTextSlide
          headerTitle="Title / Chapter"
          title="Header"
          body={mediumCopy}
          imageSrc="/presentation/slide-image-text-2.png"
          imageAlt="Sample exterior image"
          footer={pagedFooter}
        />
      </Slide>
      <Slide>
        <TwoColumnImageTextSlide
          headerTitle="Title / Chapter"
          title="Header"
          items={twoImageItems}
          footer={pagedFooter}
        />
      </Slide>
      <Slide>
        <ClosingWordmarkSlide />
      </Slide>
    </Deck>
  );
}
```

## Best Practices

### Presentation Design & Storytelling

- Define the big idea and storyboard a clear beginning, middle, and end before building slides.
- Keep one idea per slide. Preserve meaning, not wording. If all source detail will not fit comfortably, move or drop secondary detail instead of shrinking it into one canvas.
- Do not use dense UI card or panel stacks as bullet lists. If a panel needs more than 3 concise bullets or a short paragraph, cut or move the rest; otherwise it is the wrong layout.
- If a layout creates large empty blocks, oversized panels, or heavily wrapped bullets, simplify it or split the content into another slide.
- Maximize visuals and keep text concise enough for large, readable type.
- Use iconography where it improves clarity.
- Use strong visual hierarchy with size, white space, and contrast.

### Technical Integration

- Use the default Reveal.js transition unless the user explicitly requests another one.
- Do not mix React state tightly with Reveal's internal navigation state unless necessary.
- Plugins are initialization-only. The `plugins` prop on `<Deck>` is captured once on mount.
- `Deck` automatically calls `reveal.sync()` when the rendered slide structure changes. `Slide` handles slide-level `data-*` attribute updates locally.
