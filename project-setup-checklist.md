# Project setup checklist

> The sequence of steps to follow when starting a new project. Complete these before feature development begins. Skipping or rushing this phase produces inconsistent UIs, coupled components, and rework.

**Version:** 1.0
**Status:** Active

---

## Overview

Setting up the UI foundation for a new project follows this order:

1. **Audit the brand guidelines** — confirm you have what you need to build
2. **Set up the design token layer** — translate brand decisions into code
3. **Create the dev showcase routes** — a living reference that forces isolated component development
4. **Build the component library** — implement components to spec, demonstrated in the showcase
5. **Build the layout shells** — implement the app shell and content area patterns
6. **Build the page templates** — compose layouts and components into standard page types
7. **Begin feature development** — build features using the library, not from scratch

Each step has a clear output. Do not start step 7 until steps 1–6 are done.

---

## Step 1: Audit the brand guidelines

**Input:** Brand guidelines (from client, designer, or internal team)
**Reference:** → brand-guidelines-spec.md
**Output:** A confirmed set of brand assets and a list of gaps to resolve

Run through the completeness checklist in brand-guidelines-spec.md. For every "must" item that is missing, flag it and get it resolved before proceeding. Do not fill gaps with assumptions — assumptions diverge across developers and become expensive to correct later.

At minimum you need before proceeding:

- [ ] Primary and secondary colours with hex/RGB/HSL values
- [ ] Neutral palette (background, surface, border, text hierarchy)
- [ ] Semantic colour mapping (action, status, background, text tokens)
- [ ] Font families with weights, source, and fallback stacks
- [ ] Type scale with sizes, weights, and line heights
- [ ] Spacing scale
- [ ] Border radius scale
- [ ] Icon set name and style
- [ ] Writing principles and terminology glossary

---

## Step 2: Set up the design token layer

**Input:** Audited brand guidelines
**Reference:** → UX Standards §10.2 (colour tokens), → brand-guidelines-spec.md §2.4 (semantic mapping)
**Output:** A Tailwind config and/or CSS custom properties file that encodes all brand decisions

Translate the brand guidelines into code:

- [ ] Define semantic colour tokens as CSS custom properties or Tailwind config extensions
- [ ] Configure the type scale in Tailwind (font sizes, weights, line heights, font families)
- [ ] Configure the spacing scale (if non-default)
- [ ] Configure the border radius scale
- [ ] Add the `xs` breakpoint to Tailwind config (→ UX Standards §16.2)
- [ ] Set up the z-index scale (→ layout-patterns.md, Cross-cutting concerns)
- [ ] Configure shadow/elevation scale if applicable
- [ ] Set up dark mode tokens if applicable (→ UX Standards §10.5)

The token layer is the single source of truth for visual decisions. Components reference tokens, never raw values. This makes rebranding a config change rather than a codebase search-and-replace.

---

## Step 3: Create the dev showcase routes

**Input:** A running application (even if it has no features yet)
**Output:** A set of development-only routes that serve as a living component reference, layout demo, and brand showcase

This is not optional. The dev showcase is what forces components to be built in isolation, decoupled from business logic. It is also how any team member — developer, designer, product owner, QA — can view, demo, and test every component and layout without navigating through the application's real flows.

### Route structure

Set up the following routes under a `/dev` or `/doc` prefix:

```
/dev
├── /dev/brand          — brand reference (colours, typography, icons, spacing)
├── /dev/components     — component showcase index
├── /dev/components/:id — individual component demo (e.g. /dev/components/button)
├── /dev/layouts        — layout pattern demos
└── /dev/pages          — page template demos with mock data
```

### Requirements

**Development builds only.** These routes must not be included in production builds. Strip them at build time using environment checks, route guards, or build-time tree shaking. The mechanism depends on the framework:

- **Next.js:** place under `app/(dev)/` with middleware that blocks in production, or use a custom route matcher
- **Nuxt:** use a module or middleware that registers routes only when `process.env.NODE_ENV !== 'production'`
- **SPA (React Router, Vue Router):** conditionally include routes based on environment
- **Any framework:** a build-time environment check is more reliable than a runtime guard. If the route code is not in the production bundle, it cannot be accessed

**No business logic.** Components in the showcase must render with mock/static data passed as props. No API calls, no auth checks, no state management beyond what the component itself needs. If a component cannot render without importing a service or store, it is too coupled.

**Every component must appear here.** A component that is not in the showcase is not part of the library. This is the rule that prevents one-off component implementations buried in feature code.

**Every state must be demonstrated.** For each component, show: default, hover (via interaction), focus, disabled, loading, error, empty, and any other states defined in the component spec. A component that only shows its default state in the showcase will only be built correctly in its default state.

### /dev/brand

A single page showing the project's visual identity:

- **Colour palette** — all semantic tokens rendered as swatches with their token names, hex values, and contrast ratios against common backgrounds
- **Typography** — the full type scale rendered as specimen text with size, weight, line height, and token name
- **Icons** — a grid of all icons in use, with names, at default and alternative sizes
- **Spacing** — a visual scale showing each spacing value
- **Border radii** — examples of each radius value applied to a box
- **Shadows/elevation** — examples of each shadow level

This page serves two purposes: it verifies the token layer is set up correctly, and it gives the team a single place to reference the visual system.

### /dev/components

An index page listing all components in the project library, grouped by category (matching the categories in → components/_index.md). Each links to its individual demo page.

### /dev/components/:id

A dedicated page for each component showing:

- **All variants** — e.g. button: primary, secondary, ghost, destructive, icon
- **All states** — a state table or interactive demo showing every state the component supports
- **All sizes** (if applicable) — small, medium, large
- **Responsive** — the page itself is responsive, so viewing it at different viewport widths demonstrates the component's responsive behaviour
- **Interactive** — where possible, the component should be interactive (buttons click, inputs accept text, modals open) so the team can test real behaviour, not just static renders
- **Props/config reference** — a table or list of the component's props, their types, defaults, and descriptions. This becomes the developer documentation

### /dev/layouts

Demo pages for each layout pattern the project uses (→ layout-patterns.md):

- **App shell** — the sidebar or top nav shell, rendered with placeholder navigation and a mock content region
- **Content area patterns** — each pattern (single column, dashboard grid, split view, main + aside, etc.) rendered with placeholder content inside the app shell
- **Responsive** — these pages must be tested at every breakpoint. Consider adding a breakpoint indicator that shows the current breakpoint name and width

### /dev/pages

Demo pages for each page template the project uses (→ page-templates.md):

- **Each template, every state** — the list page shown in loading, empty, populated, and error states. The detail page with data and without. The form in default, validating, and submitting states
- **Mock data** — use realistic but obviously fake data. Names like "Acme Corp" and "Jane Smith" are fine. Do not use real customer data, even in dev
- **Interactive flows** — the create form should submit (to nowhere) and show success. The delete confirmation should open and confirm. These are the flows users will test in prototyping

### Keeping the showcase alive

The showcase is not a one-time setup task — it must be maintained alongside the component library:

- When a new component is built, add it to the showcase before using it in feature code
- When a component gains a new variant or state, add it to the showcase
- When a component's behaviour changes, update the showcase
- Treat the showcase as the specification — if it works correctly in the showcase, it should work correctly everywhere. If it doesn't work in the showcase, it's not ready for use

---

## Step 4: Build the component library

**Input:** Component specs (→ components/), design tokens from step 2
**Reference:** → AGENTS.md (Phase 1: Build the component library)
**Output:** A project-specific component library, every component demonstrated in the dev showcase

Identify which components the project needs from → components/_index.md. Not every project needs all 41 components — select the set that matches the product's scope.

For each component:

- [ ] Follow the spec file: anatomy, variants, states, interaction, responsive, accessibility
- [ ] Use semantic tokens from the token layer, not raw colour/size values
- [ ] Add the component to the dev showcase with all variants and states
- [ ] Test keyboard navigation and screen reader behaviour
- [ ] Review against the common pitfalls section in the spec
- [ ] Verify responsive behaviour at `xs` through `lg`

Build order recommendation — start with the components that other components depend on:

1. **Foundation:** Button, Icon Button, Input, Badge, Spinner, Skeleton
2. **Feedback:** Toast, Alert, Modal, Slide Panel, Tooltip, Empty State
3. **Form controls:** Textarea, Select, Combobox, Checkbox, Radio Group, Toggle, Slider, Date Picker, File Upload
4. **Data display:** Card, Table, Stat Card, Avatar, Accordion, Timeline, Progress Bar
5. **Navigation:** Header, Sidebar, Breadcrumbs, Tabs, Pagination, Stepper, Footer
6. **Composite:** Form (pattern), Search, Dropdown Menu, Context Menu, Command Palette, Bulk Action Bar

---

## Step 5: Build the layout shells

**Input:** Layout pattern specs (→ layout-patterns.md), design tokens
**Output:** Reusable layout shell components, demonstrated in the dev showcase

- [ ] Choose the app shell pattern (sidebar, top nav, bottom nav, or hybrid)
- [ ] Build the shell as a reusable wrapper component
- [ ] Identify which content area patterns the project needs
- [ ] Build each content area pattern as a reusable layout component
- [ ] Add all layouts to `/dev/layouts` with placeholder content
- [ ] Test responsive behaviour — especially the sidebar/bottom nav hybrid transition
- [ ] Verify scroll behaviour: shell stays fixed, content scrolls, no competing scroll regions

---

## Step 6: Build the page templates

**Input:** Page template specs (→ page-templates.md), components from step 4, layouts from step 5
**Output:** Standard page types composed from components and layouts, demonstrated in the dev showcase

- [ ] Identify which page templates the project needs
- [ ] Build each as a composition of layout + components with mock data
- [ ] Implement every page state (loading, empty, error, populated)
- [ ] Add all pages to `/dev/pages` with mock data and interactive flows
- [ ] Verify navigation flow: list → detail → edit → back works correctly
- [ ] Verify breadcrumbs follow the patterns in → page-templates.md
- [ ] Test the delete/archive confirmation flow end-to-end

---

## Step 7: Begin feature development

**Input:** A complete component library, layout system, and page template set — all visible in the dev showcase
**Output:** Product features built by composing existing components, not building new UI from scratch

Rules during feature development:

- **Use the library.** If a feature needs a button, use the button from the library. Do not create one-off component implementations in feature code
- **Extend the library, not the feature.** If a feature needs a component variant that doesn't exist, add the variant to the library and the showcase first, then use it in the feature. This keeps the library as the single source of truth
- **Flag gaps.** If a feature needs a component not covered by the guidelines, flag it. It may warrant a new guideline file in this repo
- **Keep business logic out of components.** Components receive data via props and emit events. They do not import services, call APIs, or access global state. Feature code handles data fetching and state management; components handle rendering and interaction
