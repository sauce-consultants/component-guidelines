# Digital brand guidelines — what they must contain

> A checklist for creating or evaluating brand guidelines before a build begins. If a brand guideline does not cover the sections below, the team will make ad-hoc decisions that diverge across screens, developers, and projects.

**Version:** 1.0
**Status:** Active

---

## How to use this document

This is not a brand guideline. It is a specification for what a brand guideline must deliver so that a UI can be built consistently from it.

Use it in two situations:

1. **Creating brand guidelines** for a new product — use this as the section checklist
2. **Receiving brand guidelines** from a client or designer — audit against this list and flag gaps before development begins

Requirement levels follow the Sauce convention:

- **must** — the build cannot proceed without this. Gaps will cause inconsistency
- **should** — strongly recommended. Absence creates risk but can be resolved during build
- **could** — useful refinement. Can be deferred without blocking work

---

## 1. Logo

### 1.1 Primary logo (must)

The primary logo in its default orientation and colour. Provide the canonical version that appears in the header, splash screen, and anywhere the brand is represented at full fidelity.

### 1.2 Logo variants (must)

Every variant the product will need:

- **Full colour** — for use on the primary background
- **Reversed / white** — for use on dark backgrounds or overlays
- **Monochrome** — for low-colour contexts (email footers, print, greyscale)
- **Icon-only / symbol** — the mark without the wordmark, for favicons, avatars, compact headers, and mobile home screen icons

For each variant, state which backgrounds it may and may not be placed on.

### 1.3 Clear space (must)

Define the minimum clear space around the logo, expressed as a proportion of the logo itself (e.g. "the height of the letter 'S' on all sides"). Do not define clear space in fixed pixel values — the logo will render at different sizes.

### 1.4 Minimum size (must)

The smallest size the logo may be rendered at before it becomes illegible or loses detail. Provide minimums for:

- Full logo (wordmark + symbol)
- Symbol only

Express as a minimum width in pixels for digital use.

### 1.5 Misuse rules (should)

Explicit examples of what not to do: stretching, rotating, recolouring outside the approved palette, placing on busy backgrounds, adding effects. These feel obvious but prevent drift over time.

---

## 2. Colour palette

### 2.1 Primary colours (must)

The core brand colours — typically one to three. For each colour provide:

- **Name** — a human-readable label (not just "Blue")
- **Hex value**
- **RGB value**
- **HSL value** (useful for programmatic manipulation)
- **Tailwind mapping** — which Tailwind palette shade it corresponds to, or whether it requires a custom value

### 2.2 Secondary colours (should)

Supporting colours used for accents, illustrations, or secondary UI elements. Same format as primary.

### 2.3 Neutral palette (must)

A greyscale ramp used for text, borders, backgrounds, and disabled states. This is the workhorse of any UI — without a defined neutral scale, every developer will pick slightly different greys.

Provide at minimum:

- Background (lightest)
- Surface (cards, panels)
- Border
- Muted text
- Body text
- Heading text (darkest)

### 2.4 Semantic colour mapping (must)

How brand and neutral colours map to UI meanings. This bridges the brand palette to the semantic token layer defined in → UX Standards §10.2.

| Token | Purpose | Value |
|---|---|---|
| `--color-action-primary` | Primary buttons, links | Brand primary |
| `--color-action-primary-hover` | Hover state for primary actions | Darker shade of brand primary |
| `--color-status-error` | Error text, destructive actions | Red (specify which) |
| `--color-status-warning` | Warnings | Amber (specify which) |
| `--color-status-success` | Success confirmations | Green (specify which) |
| `--color-status-info` | Informational alerts | Blue (specify which) |
| `--color-bg-page` | Page background | Neutral lightest |
| `--color-bg-surface` | Card/panel background | Neutral surface |
| `--color-border-default` | Default borders | Neutral border |
| `--color-text-primary` | Body text | Neutral body |
| `--color-text-secondary` | Muted/supporting text | Neutral muted |
| `--color-text-heading` | Headings | Neutral darkest |

This table is the single most important deliverable in the brand guidelines for UI development. Without it, developers will guess — and they will guess differently.

### 2.5 Dark mode palette (should)

If the product will support dark mode, provide the equivalent values for every token above. If dark mode is not planned, state this explicitly so the token architecture can still accommodate it later (→ UX Standards §10.5).

### 2.6 Contrast verification (must)

Confirm that all text/background colour combinations meet WCAG contrast ratios (→ UX Standards §10.3). Do not leave this to developers to discover and fix. Flag any brand colour that fails contrast and provide an approved accessible alternative.

---

## 3. Typography

### 3.1 Font families (must)

Specify the font families used in the product:

- **Primary typeface** — used for body text and most UI elements
- **Secondary typeface** (if any) — used for headings, display text, or accents
- **Monospace typeface** (if needed) — used for code, data, or tabular numbers

For each, provide:

- Family name
- Weights available (and which weights the product actually uses)
- Source (Google Fonts, Adobe Fonts, self-hosted, system font stack)
- Fallback stack
- Licensing — confirm the licence covers web embedding

### 3.2 Type scale (must)

A defined scale of font sizes used across the product. Do not leave developers to pick sizes ad-hoc.

Provide a named scale, for example:

| Name | Size | Weight | Line height | Use |
|---|---|---|---|---|
| `display` | 36px | 700 | 1.2 | Hero headings |
| `h1` | 30px | 700 | 1.25 | Page titles |
| `h2` | 24px | 600 | 1.3 | Section headings |
| `h3` | 20px | 600 | 1.35 | Subsection headings |
| `h4` | 16px | 600 | 1.4 | Card/group headings |
| `body` | 16px | 400 | 1.5 | Default body text |
| `body-sm` | 14px | 400 | 1.5 | Secondary text, table cells |
| `caption` | 12px | 400 | 1.4 | Timestamps, metadata |

Sizes must respect the minimums in → UX Standards §11.1. Line heights must respect → UX Standards §11.3.

### 3.3 Heading hierarchy rules (should)

Clarify how heading levels map to the type scale across different page types. A settings page and a marketing landing page may use different starting levels. Define the convention so headings are semantically correct, not just visually sized (→ UX Standards §11.5).

### 3.4 Font weight usage (should)

State which weights are used and where. Constrain to two or three weights maximum (→ UX Standards §11.6). A guideline that provides six weights without specifying when to use each is worse than one that provides two with clear rules.

---

## 4. Spacing and layout

### 4.1 Spacing scale (must)

A defined spacing scale used for padding, margins, and gaps. Align with Tailwind's default spacing scale where possible (multiples of 4px: 4, 8, 12, 16, 20, 24, 32, 40, 48, 64).

If the brand requires a custom scale, define it explicitly and map it to Tailwind config values.

### 4.2 Layout grid (should)

Define the page-level grid:

- Number of columns (typically 12)
- Gutter width
- Margin width at each breakpoint
- Maximum content width

### 4.3 Component spacing conventions (should)

Standard spacing values for common patterns:

- Space between form fields
- Space between a label and its input
- Padding inside cards and panels
- Padding inside buttons
- Space between list items
- Space between page sections

These can be ranges rather than fixed values, but providing them prevents inconsistency.

---

## 5. Elevation and depth

### 5.1 Shadow scale (should)

If the design uses elevation (shadows to imply layering), define a scale:

| Level | Use | Value |
|---|---|---|
| `shadow-sm` | Subtle lift — cards, inputs | ... |
| `shadow-md` | Elevated — dropdowns, popovers | ... |
| `shadow-lg` | High prominence — modals, command palette | ... |

Tailwind's default shadow scale is a reasonable starting point. If the brand uses a custom shadow style (coloured shadows, sharp shadows, no shadows), specify this.

### 5.2 Border style (should)

Default border width, colour, and radius for:

- Cards and containers
- Inputs and form controls
- Dividers and separators

### 5.3 Border radius scale (must)

A defined radius scale:

| Token | Value | Use |
|---|---|---|
| `rounded-sm` | ... | Badges, small elements |
| `rounded-md` | ... | Buttons, inputs |
| `rounded-lg` | ... | Cards, panels |
| `rounded-full` | 9999px | Avatars, pills |

A product that uses inconsistent border radii looks unfinished immediately.

---

## 6. Iconography

### 6.1 Icon set (must)

Name the icon set the product uses. One set only (→ UX Standards §12.1). Specify:

- Library name (Lucide, Heroicons, Phosphor, custom, etc.)
- Style (outline, filled, duotone)
- Default size (→ UX Standards §12.2 — 24px grid)
- Stroke weight (if applicable)

### 6.2 Icon colour rules (should)

How icons inherit colour. The default should be `currentColor` (→ UX Standards §12.3). If specific icons use brand colour or status colour, define the rule.

---

## 7. Imagery and illustration

### 7.1 Photography style (could)

If the product uses photography, describe the style: subject matter, colour treatment, mood. Provide example images.

### 7.2 Illustration style (could)

If the product uses illustrations (empty states, onboarding, marketing), describe the style: flat, isometric, line art, colour palette, character style. Provide examples.

### 7.3 Image treatment (should)

Rules for how images are displayed: aspect ratios, cropping conventions, border radius, overlay treatments.

---

## 8. Tone of voice

### 8.1 Writing principles (must)

Three to five principles that describe how the product communicates. These should be specific enough to resolve disagreements, not generic aspirations like "friendly and professional."

Example: "We explain what happened and what to do next. We never blame the user. We use the same word for the same thing every time."

### 8.2 Terminology glossary (should)

A list of product-specific terms and their approved usage. This prevents the inconsistency described in → UX Standards §18.4.

| Term | Use | Do not use |
|---|---|---|
| Job | A unit of work assigned to a team member | Task, order, assignment |
| Customer | The end client | User, client, account |

### 8.3 Formatting conventions (should)

- Date format (e.g. "26 Mar 2026", not "03/26/2026")
- Time format (e.g. "14:30" or "2:30 PM")
- Number format (e.g. "1,000" or "1000")
- Currency format
- Capitalisation rules (→ UX Standards §18.3 — sentence case)

---

## 9. Motion and animation

### 9.1 Animation personality (could)

Describe the feel of motion in the product: snappy and minimal, smooth and expressive, playful, restrained. This guides decisions when the UX standards provide ranges (e.g. 150–250ms for transitions — should this product lean toward 150 or 250?).

### 9.2 Easing and duration tokens (should)

If the brand has specific timing preferences, define tokens:

| Token | Value | Use |
|---|---|---|
| `duration-fast` | 150ms | Button states, toggles |
| `duration-normal` | 200ms | Panels, dropdowns |
| `duration-slow` | 350ms | Page transitions, modals |
| `ease-default` | cubic-bezier(...) | Standard easing |

These must respect the constraints in → UX Standards §14.

---

## 10. Component-level visual specs

### 10.1 Component tokens (should)

For high-frequency components, provide specific visual values. At minimum cover:

- **Buttons** — height, padding, font size, radius per variant (primary, secondary, destructive)
- **Inputs** — height, padding, border, font size, placeholder colour
- **Cards** — padding, radius, shadow, border
- **Modals** — width, padding, overlay colour and opacity

These values, combined with the component behaviour specs in the component guidelines, give a developer everything needed to build a component without guessing.

### 10.2 Design tokens file (could)

Provide tokens as a structured file (JSON, CSS custom properties, or Tailwind config extension) that developers can import directly. This removes interpretation entirely and is the most reliable delivery format.

---

## Completeness checklist

Use this to audit a brand guideline before build begins. Items marked (must) block development if missing.

| # | Item | Level | Present? |
|---|---|---|---|
| 1.1 | Primary logo | must | |
| 1.2 | Logo variants (reversed, mono, icon-only) | must | |
| 1.3 | Clear space rules | must | |
| 1.4 | Minimum size | must | |
| 1.5 | Misuse rules | should | |
| 2.1 | Primary colours with hex/RGB/HSL | must | |
| 2.2 | Secondary colours | should | |
| 2.3 | Neutral palette | must | |
| 2.4 | Semantic colour mapping | must | |
| 2.5 | Dark mode palette | should | |
| 2.6 | Contrast verification | must | |
| 3.1 | Font families with weights and source | must | |
| 3.2 | Type scale | must | |
| 3.3 | Heading hierarchy rules | should | |
| 3.4 | Font weight usage | should | |
| 4.1 | Spacing scale | must | |
| 4.2 | Layout grid | should | |
| 4.3 | Component spacing conventions | should | |
| 5.1 | Shadow scale | should | |
| 5.2 | Border style | should | |
| 5.3 | Border radius scale | must | |
| 6.1 | Icon set | must | |
| 6.2 | Icon colour rules | should | |
| 7.1 | Photography style | could | |
| 7.2 | Illustration style | could | |
| 7.3 | Image treatment | should | |
| 8.1 | Writing principles | must | |
| 8.2 | Terminology glossary | should | |
| 8.3 | Formatting conventions | should | |
| 9.1 | Animation personality | could | |
| 9.2 | Easing and duration tokens | should | |
| 10.1 | Component tokens | should | |
| 10.2 | Design tokens file | could | |
