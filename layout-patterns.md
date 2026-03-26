# Layout patterns

> A catalogue of full-page layout patterns for web applications. Each pattern describes when to use it, how it is structured, how it adapts across breakpoints, and what to watch out for.

**Version:** 1.1
**Status:** Active

---

## How to use this document

This is a pattern catalogue, not a prescriptive spec. Most products use two or three of these patterns across different page types. At the start of a project:

1. Choose a **shell pattern** — the outer wrapper that provides global navigation and structure
2. Choose **content area patterns** for each page type — these live inside the shell's content region
3. Build each as a reusable layout component

Patterns are organised into two groups:

- **Shell patterns** (section A) define the outermost application structure: global navigation, persistent chrome, and the content region where pages render. Every application has exactly one shell.
- **Content area patterns** (section B) define how individual pages organise their content within the shell's content region. These patterns do not own the full viewport — they fill whatever space the shell gives them.

Patterns reference the Tailwind breakpoint scale defined in → UX Standards §16.2: `xs` (375px), `sm` (640px), `md` (768px), `lg` (1024px), `xl` (1280px), `2xl` (1536px).

---

## Cross-cutting concerns

These apply to all layout patterns. Read this section before the individual patterns.

### Content width

Most readable content should be constrained to a maximum width. Unconstrained text running edge-to-edge on a wide monitor is unusable (→ UX Standards §11.2 — 80 character max line length for body text).

| Content type      | Max width               | Rationale                                        |
| ----------------- | ----------------------- | ------------------------------------------------ |
| Prose / body text | 65ch–80ch (~720px)      | Readability                                      |
| Forms             | 480–640px               | Prevents inputs stretching to absurd widths      |
| Cards in a grid   | Container-driven        | Cards size to their grid column, not a fixed max |
| Data tables       | 100% of available space | Tables benefit from horizontal room              |
| Dashboards        | 100% of available space | Dense layouts use all available space            |

Use `max-w-prose`, `max-w-screen-md`, or similar Tailwind utilities. Apply the constraint to the content area, not the layout shell.

### Scroll strategy

Every layout must have a clear scroll model. Ambiguity here causes janky behaviour — competing scroll regions, content trapped behind sticky elements, or the entire page bouncing when it shouldn't.

**Full-page scroll:** The entire page scrolls as one unit. The simplest model. Use when there is a single content flow and no persistent navigation that must remain visible.

**Fixed shell, scrolling content:** The outer shell (header, sidebar) is fixed or sticky. The main content area scrolls independently. Use when navigation must remain accessible at all times.

**Multiple scroll regions:** Two or more areas of the page scroll independently (e.g. a sidebar list and a detail panel). Use sparingly — each independent scroll region adds complexity. Never nest scrollable regions inside scrollable regions without clear visual boundaries.

Rules:

- The page body must not scroll if a fixed shell layout is in use — only the content region scrolls
- On mobile, fixed shells typically collapse and the layout reverts to full-page scroll
- Scroll regions must have visible boundaries so users understand what will move

### Sticky elements

Elements that stick on scroll (headers, toolbars, filter bars) must:

- Have a defined `z-index` from a project-wide z-index scale (see below)
- Not overlap or obscure content when the page scrolls to an anchor or when the browser's find-in-page highlights a match
- Account for their own height in the scroll offset — content below a sticky header must not be hidden behind it
- Collapse or unpin on mobile if they consume too much vertical space

### Z-index scale

Define a project-wide z-index scale. Do not use arbitrary z-index values. A reasonable default:

| Layer              | z-index | Use                                |
| ------------------ | ------- | ---------------------------------- |
| Base content       | 0       | Default page content               |
| Sticky elements    | 10      | Sticky headers, toolbars           |
| Dropdown / popover | 20      | Dropdown menus, popovers, tooltips |
| Overlay            | 30      | Slide panels, drawers              |
| Modal backdrop     | 40      | Modal dimming overlay              |
| Modal              | 50      | Modal dialogs                      |
| Toast              | 60      | Toast notifications                |
| Command palette    | 70      | Command palette, global search     |

Adapt the specific values to the project, but maintain the ordering. The point is that every developer can look up where a component sits rather than guessing and incrementing.

### Responsive layout shifts

When a layout adapts across breakpoints, the transition must not be jarring. Common strategies:

- **Sidebar collapses:** Sidebar disappears below `lg` and becomes a hamburger-triggered drawer or bottom nav
- **Columns reduce:** A three-column layout becomes two at `md` and one at `sm`
- **Content reorders:** Secondary content (sidebar widgets, filters) moves below primary content on small screens rather than disappearing

Content must never be lost at smaller breakpoints — it should reflow, collapse, or become accessible through a toggle. Hiding content entirely is acceptable only for decorative or supplementary elements.

→ UX Standards §16.3 for reflow rules, §16.4 for navigation adaptation.

---

# A. Shell patterns

Shell patterns define the outermost application structure. They own the full viewport, provide global navigation, and create a **content region** where page-level patterns render.

Every application has exactly one shell. Do not nest shells. Content area patterns (section B) always render inside the shell's content region.

---

## Shell 1: Sidebar app shell

The most common shell for admin panels, back-office tools, and SaaS products with navigation deeper than one level. The sidebar provides persistent, always-visible navigation. The content region fills the remaining viewport and is where content area patterns render.

### When to use

- Admin panels and back-office tools
- Products with a persistent navigation hierarchy deeper than one level
- SaaS products where users move frequently between sections
- Documentation sites with a table of contents

### When not to use

- Public-facing marketing sites (use top nav shell or no shell)
- Simple single-task flows (use top nav shell or standalone single column)
- Mobile-first products where the sidebar would dominate the screen

### Anatomy

```
┌──────┬──────────────────────────────┐
│      │          Header              │
│      ├──────────────────────────────┤
│ Side │                              │
│ bar  │       Content region         │
│      │    (page patterns render     │
│      │         here)                │
│      │                              │
└──────┴──────────────────────────────┘
```

1. **Sidebar** — fixed height (viewport), scrolls independently if content overflows. Contains navigation, section links, or a tree structure. Typical width: 240–280px
2. **Header** — spans the content region (not the sidebar). Contains page title, breadcrumbs, and page-level actions
3. **Content region** — fills remaining width, scrolls independently. This is where content area patterns from section B render

### Responsive behaviour

| Breakpoint | Behaviour                                                                                                           |
| ---------- | ------------------------------------------------------------------------------------------------------------------- |
| `xs`–`md`  | Sidebar hidden. Navigation moves to a hamburger-triggered drawer or bottom nav bar. Content region takes full width |
| `lg`       | Sidebar visible, fixed. Content region fills remaining width                                                        |
| `xl`+      | Sidebar visible. Content region may introduce its own max-width constraint to prevent over-stretching               |

### Scroll strategy

Fixed shell, scrolling content. The sidebar and header remain in place. The content region scrolls independently. On mobile, reverts to full-page scroll.

### Variants

**Collapsible sidebar** — sidebar can be toggled between full width (with labels) and icon-only width (~64px). Useful when screen space is at a premium but persistent nav is still valuable. The collapsed state must show tooltips on icon hover (→ component: tooltip.md).

**Sidebar below header** — header spans the full viewport width, sidebar sits below it. Use when the header contains global controls (account menu, notifications, search) that should not be visually subordinate to the sidebar.

```
┌─────────────────────────────────────┐
│              Header                 │
├──────┬──────────────────────────────┤
│ Side │                              │
│ bar  │       Content region         │
│      │                              │
└──────┴──────────────────────────────┘
```

**Right sidebar** — content on the left, sidebar on the right. Used for contextual panels: properties, details, activity logs. Often combined with a left nav sidebar in complex tools.

### Common pitfalls

1. **Sidebar does not scroll independently.** If the sidebar content is longer than the viewport, it must scroll within its own container. Do not let it push the page layout.
2. **Content region has no max-width on very wide screens.** At 2560px, a content region running the full remaining width after a 256px sidebar is too wide. Constrain or centre the content within.
3. **Mobile sidebar is just hidden with no alternative.** Navigation must remain accessible on mobile. Provide a drawer, bottom nav, or hamburger menu.
4. **Sidebar width is not consistent.** The sidebar must not resize based on content. Use a fixed width.
5. **No clear active state in sidebar navigation.** The current page or section must be visually highlighted.

### Tailwind notes

```
<!-- Sidebar app shell -->
<div class="flex h-screen">
  <aside class="hidden lg:flex lg:w-64 lg:flex-col border-r overflow-y-auto">
    <!-- Sidebar navigation -->
  </aside>
  <div class="flex-1 flex flex-col min-w-0">
    <header class="border-b"><!-- Page header --></header>
    <main class="flex-1 overflow-y-auto p-4 sm:p-6 lg:p-8">
      <!-- CONTENT REGION: content area patterns render here -->
    </main>
  </div>
</div>
```

`min-w-0` on the content column prevents flex children from overflowing when content is wider than the available space (e.g. wide tables).

---

## Shell 2: Top nav app shell

A simpler shell for products with shallow navigation. The top nav provides a horizontal navigation bar, leaving the full viewport width available for content.

### When to use

- Products with shallow navigation (5–8 top-level sections)
- SaaS products where the primary navigation fits in a single horizontal bar
- Products that need maximum content width (no sidebar eating horizontal space)

### When not to use

- Deep navigation hierarchies — a top nav cannot hold nested sections well
- Products with more than 8 top-level navigation items (it will overflow or become cramped)

### Anatomy

```
┌─────────────────────────────────────┐
│              Top nav                │
├─────────────────────────────────────┤
│                                     │
│          Content region             │
│       (page patterns render         │
│            here)                    │
│                                     │
└─────────────────────────────────────┘
```

1. **Top nav** — fixed or sticky, full width. Contains logo, navigation links, and utility items (search, account, notifications)
2. **Content region** — below the nav, full width or constrained depending on page type. Content area patterns from section B render here

### Responsive behaviour

| Breakpoint | Behaviour                                                                                                       |
| ---------- | --------------------------------------------------------------------------------------------------------------- |
| `xs`–`sm`  | Nav items collapse into a hamburger menu or move to a bottom nav bar. Logo and key utility items remain visible |
| `md`       | Nav items may partially show (priority items visible, overflow in a "more" menu)                                |
| `lg`+      | All nav items visible in the horizontal bar                                                                     |

### Scroll strategy

Header is sticky. Content uses full-page scroll. If the header has a secondary row (tabs, breadcrumbs), consider whether both rows stick or only the primary row.

### Variants

**Top nav + sub-nav** — a secondary horizontal bar below the primary nav for section-level tabs or breadcrumbs. Common in products where each top-level section has its own set of sub-pages.

```
┌─────────────────────────────────────┐
│           Primary nav               │
├─────────────────────────────────────┤
│         Sub-nav / tabs              │
├─────────────────────────────────────┤
│          Content region             │
└─────────────────────────────────────┘
```

**Top nav + page sidebar** — some pages within a top-nav product need a sidebar (e.g. settings). The sidebar is page-specific, not global. It sits inside the content region, not at the shell level.

### Common pitfalls

1. **Navigation items overflow at medium breakpoints.** Plan for the in-between state where not all items fit but the hamburger menu hasn't triggered yet. Use a "more" dropdown or earlier collapse.
2. **Sticky header is too tall.** A sticky header taller than 64px on desktop or 56px on mobile consumes too much viewport. Keep it compact.
3. **Sub-nav creates two sticky rows stacking up.** Two sticky rows eating 120px+ of vertical space is too much. Consider only pinning the primary nav and letting the sub-nav scroll, or combining them.
4. **Content region is not offset for sticky header.** Content starts hidden behind the sticky header. Apply top padding or offset equal to the header height.

### Tailwind notes

```
<!-- Top nav app shell -->
<div class="min-h-screen">
  <header class="sticky top-0 z-10 border-b bg-white">
    <!-- Global navigation -->
  </header>
  <main class="max-w-screen-xl mx-auto px-4 sm:px-6 lg:px-8 py-6">
    <!-- CONTENT REGION: content area patterns render here -->
  </main>
</div>
```

---

## Shell 3: Bottom nav app shell

A mobile-native shell where primary navigation sits at the bottom of the screen, within thumb reach. This is the standard pattern for mobile-first web apps, PWAs, and responsive products that need a distinct mobile navigation experience.

### When to use

- Mobile-first or mobile-only web applications and PWAs
- Products where the primary audience is on phones and the top 3–5 actions must be instantly reachable
- As the mobile breakpoint counterpart to a sidebar app shell (sidebar on desktop, bottom nav on mobile)

### When not to use

- Desktop-only products — bottom nav wastes space and feels out of place on large screens
- Products with more than 5 top-level navigation items — bottom nav cannot hold them without scrolling or overflow, which defeats the purpose
- Products with deep navigation hierarchies — bottom nav only supports one level

### Anatomy

```
┌─────────────────────────────────────┐
│           Status bar (OS)           │
├─────────────────────────────────────┤
│         Page header (optional)      │
├─────────────────────────────────────┤
│                                     │
│          Content region             │
│       (page patterns render         │
│            here)                    │
│                                     │
│                                     │
├─────────────────────────────────────┤
│  🏠   📋   ➕   💬   👤             │
│         Bottom nav bar              │
└─────────────────────────────────────┘
```

1. **Bottom nav bar** — fixed to the bottom of the viewport. Contains 3–5 navigation items, each with an icon and short label. The active item is visually highlighted
2. **Page header** — optional, at the top of the content region. Contains page title and page-level actions. Not a global nav — the bottom bar handles that
3. **Content region** — fills the space between the page header and the bottom nav. Content area patterns from section B render here

### Responsive behaviour

| Breakpoint | Behaviour                                                                                  |
| ---------- | ------------------------------------------------------------------------------------------ |
| `xs`–`sm`  | Bottom nav visible. This is the primary use case                                           |
| `md`       | Bottom nav may still be appropriate for tablet if the product is touch-first               |
| `lg`+      | Bottom nav should not be used. Switch to a sidebar or top nav shell at desktop breakpoints |

### Scroll strategy

Content region uses full-page scroll (or scrolls within its container if the bottom nav is fixed). The bottom nav bar is fixed to the viewport bottom and must not scroll with content.

### Safe area handling

On devices with home indicators or rounded corners (modern iPhones, Android gesture nav), the bottom nav must account for the safe area inset. Without this, the nav bar sits behind the home indicator and becomes difficult to tap.

```css
padding-bottom: env(safe-area-inset-bottom);
```

### Hybrid with sidebar app shell

Many products use a sidebar on desktop and a bottom nav on mobile. These are not two separate shells — they are one responsive shell that switches navigation pattern at a breakpoint.

```
/* xs–md: bottom nav shell */
┌─────────────────────────────────────┐
│         Page header                 │
├─────────────────────────────────────┤
│                                     │
│          Content region             │
│                                     │
├─────────────────────────────────────┤
│         Bottom nav bar              │
└─────────────────────────────────────┘

/* lg+: sidebar app shell */
┌──────┬──────────────────────────────┐
│      │          Header              │
│ Side ├──────────────────────────────┤
│ bar  │       Content region         │
│      │                              │
└──────┴──────────────────────────────┘
```

Implementation: both navigation elements exist in the DOM. The sidebar is hidden below `lg` and the bottom nav is hidden at `lg`+. The content region is the same component in both cases — content area patterns do not need to know which navigation is active.

```
<aside class="hidden lg:flex lg:w-64 lg:flex-col ...">
  <!-- Sidebar: desktop -->
</aside>

<nav class="fixed bottom-0 inset-x-0 lg:hidden ...">
  <!-- Bottom nav: mobile -->
</nav>
```

The navigation items should be consistent between sidebar and bottom nav. If the sidebar has 12 items, the bottom nav shows the top 4–5 with a "More" item that opens the full menu.

### Nav item design

- Each item must have an icon and a text label. Do not use icon-only items — recognition drops significantly without labels (→ UX Standards §12.4)
- Labels must be short: one word, two at most
- The active item must be visually distinct (colour, weight, or filled vs outline icon)
- Touch targets must be at least 44×44px (→ UX Standards §13.6). Items should expand to fill available width equally
- 3–5 items maximum. At 5 items on a 375px screen, each item gets 75px — enough for an icon and short label. Beyond 5, items become too cramped

### Common pitfalls

1. **Bottom nav overlaps content.** The content region must have bottom padding equal to the nav bar height so the last content item is not hidden behind it.
2. **No safe area inset.** On modern phones, the nav sits behind the home indicator without `env(safe-area-inset-bottom)`. Always add it.
3. **Too many items.** More than 5 items forces tiny touch targets or horizontal scrolling. Use a "More" item to access additional navigation.
4. **Icon-only items without labels.** Always include text labels. Users cannot reliably identify icon meanings, especially across different products.
5. **Bottom nav visible on desktop.** Hide it at `lg`+ and switch to sidebar or top nav. A bottom nav bar on a 1440px screen looks wrong.
6. **Navigation items differ between mobile and desktop.** The core navigation should be the same. The bottom nav may show fewer items, but the items it does show must match their sidebar/top nav equivalents exactly — same labels, same destinations.

### Tailwind notes

```
<!-- Bottom nav app shell -->
<div class="min-h-screen pb-16 lg:pb-0"> <!-- pb accounts for fixed nav height -->
  <header class="border-b lg:hidden">
    <!-- Page header (mobile) -->
  </header>
  <main class="px-4 sm:px-6 py-4">
    <!-- CONTENT REGION: content area patterns render here -->
  </main>
  <nav class="fixed bottom-0 inset-x-0 z-10 border-t bg-white pb-[env(safe-area-inset-bottom)] lg:hidden">
    <div class="flex justify-around items-center h-16">
      <!-- Nav items -->
    </div>
  </nav>
</div>

<!-- Hybrid: sidebar + bottom nav shell -->
<div class="flex h-screen">
  <aside class="hidden lg:flex lg:w-64 lg:flex-col border-r overflow-y-auto">
    <!-- Sidebar: desktop -->
  </aside>
  <div class="flex-1 flex flex-col min-w-0">
    <header class="border-b"><!-- Header --></header>
    <main class="flex-1 overflow-y-auto p-4 sm:p-6 lg:p-8 pb-20 lg:pb-8">
      <!-- CONTENT REGION -->
    </main>
    <nav class="fixed bottom-0 inset-x-0 z-10 border-t bg-white pb-[env(safe-area-inset-bottom)] lg:hidden">
      <div class="flex justify-around items-center h-16">
        <!-- Nav items -->
      </div>
    </nav>
  </div>
</div>
```

---

# B. Content area patterns

Content area patterns define how individual pages organise their content. They render inside a shell's content region — they do not own the full viewport.

Key difference from shell patterns:

- They fill available space rather than setting `h-screen`
- They do not include global navigation — the shell handles that
- Their responsive behaviour adapts to the content region width, which may be narrower than the viewport (e.g. when a sidebar shell is present)

Where a content area pattern is commonly used inside a sidebar app shell, a combined anatomy diagram and Tailwind example are provided.

---

## Content 1: Single column

### When to use

- Content-focused pages: articles, settings, profile
- Authentication flows: login, register, password reset
- Simple forms and wizards
- Any page where the user's task is linear and focused

### When not to use

- Data-dense operational views — too much vertical scrolling
- Interfaces where the user needs to see two related panels simultaneously

### Anatomy

```
┌─────────────────────────────────────┐
│                                     │
│         Content (centred,           │
│         max-width constrained)      │
│                                     │
│                                     │
│                                     │
└─────────────────────────────────────┘
```

1. **Content area** — centred, width-constrained (typically `max-w-screen-md` or `max-w-prose`)

Note: the page header (title, breadcrumbs, actions) is provided by the shell. If used standalone (e.g. an auth page with no shell), add a header and footer directly.

### Responsive behaviour

| Breakpoint | Behaviour                                                  |
| ---------- | ---------------------------------------------------------- |
| `xs`–`sm`  | Content takes full width with horizontal padding (16–24px) |
| `md`       | Content begins to centre with increasing side margins      |
| `lg`+      | Content reaches max-width, centred in available space      |

### Scroll strategy

Content scrolls within the shell's content region. If used standalone, full-page scroll.

### Variants

**Narrow single column** — `max-w-lg` (~512px). For authentication screens and focused forms where a wide layout would feel empty.

**Wide single column** — `max-w-screen-lg` (~1024px). For settings pages or content with tables or wide form layouts that need more room than prose width allows.

### Inside a sidebar app shell

```
┌──────┬──────────────────────────────┐
│      │          Header              │
│      ├──────────────────────────────┤
│ Side │                              │
│ bar  │    ┌──────────────────┐      │
│      │    │  Single column   │      │
│      │    │  content         │      │
│      │    └──────────────────┘      │
│      │                              │
└──────┴──────────────────────────────┘
```

```
<!-- Inside shell's <main> -->
<div class="max-w-screen-md mx-auto">
  <!-- Single column content -->
</div>
```

### Common pitfalls

1. **Content stretches to full width of the content region.** Always constrain. A form input running 1100px wide inside a sidebar shell is unusable.
2. **Footer floats mid-screen on short-content pages.** Use a min-height or flex layout to push the footer to the bottom when content is short (standalone use only).
3. **No horizontal padding on mobile.** Content must not touch the screen edges. Minimum 16px padding at `xs`.

### Tailwind notes

```
<!-- Standalone (no shell, e.g. auth pages) -->
<div class="min-h-screen flex flex-col">
  <header>...</header>
  <main class="flex-1 w-full max-w-screen-md mx-auto px-4 sm:px-6 lg:px-8">
    ...
  </main>
  <footer>...</footer>
</div>

<!-- Inside a shell's content region -->
<div class="max-w-screen-md mx-auto">
  ...
</div>
```

---

## Content 2: Dashboard grid

### When to use

- Operational dashboards and monitoring screens
- Analytics and reporting views
- Home screens that aggregate data from multiple sources
- Any view where the user needs a high-level overview of several metrics or data sets at once

### When not to use

- Content that is naturally sequential (use single column)
- Screens with a single primary data set (use a table in a single column or split view)

### Anatomy

```
┌─────────────────────────────────────┐
│  ┌──────┐ ┌──────┐ ┌──────┐         │
│  │ Stat │ │ Stat │ │ Stat │  ...    │
│  └──────┘ └──────┘ └──────┘         │
│  ┌──────────────┐ ┌──────────────┐  │
│  │              │ │              │  │
│  │    Chart     │ │    Table     │  │
│  │              │ │              │  │
│  └──────────────┘ └──────────────┘  │
│  ┌─────────────────────────────────┐│
│  │          Full-width widget      ││
│  └─────────────────────────────────┘│
└─────────────────────────────────────┘
```

1. **Stat row** — key metrics in a horizontal row of stat cards (→ component: stat-card.md)
2. **Widget grid** — cards containing charts, tables, lists, or summaries arranged in a grid

Page-level filters and date range selectors live in the shell's header area, not inside the dashboard grid.

### Responsive behaviour

| Breakpoint | Behaviour                                                            |
| ---------- | -------------------------------------------------------------------- |
| `xs`–`sm`  | Single column stack. All widgets full width, ordered by priority     |
| `md`       | Two-column grid. Stat cards may remain in a horizontal scroll row    |
| `lg`       | Three or four-column grid depending on widget count                  |
| `xl`+      | Full grid. Consider additional columns if the content region is wide |

### Scroll strategy

Content scrolls within the shell's content region. Individual widgets do not scroll internally unless they contain a data table, in which case the table scrolls within its card.

### Grid system

Use CSS Grid for the widget layout. Define a consistent column count and let widgets span columns:

| Widget size | Columns at `lg`      | Use                        |
| ----------- | -------------------- | -------------------------- |
| Small       | 1 of 3 or 1 of 4     | Stat cards, single metrics |
| Medium      | 2 of 3 or 2 of 4     | Charts, small tables       |
| Large       | 3 of 3 or full width | Wide tables, timelines     |

Widgets must be the same height within a row, or use auto-placement with `grid-auto-rows: minmax(...)` to maintain visual rhythm.

### Inside a sidebar app shell

This is the most common combination for admin products — sidebar navigation with a dashboard as the home page.

```
┌──────┬──────────────────────────────┐
│      │          Header              │
│      ├──────────────────────────────┤
│ Side │ ┌──────┐┌──────┐┌──────┐     │
│ bar  │ │ Stat ││ Stat ││ Stat │     │
│      │ └──────┘└──────┘└──────┘     │
│      │ ┌────────────┐┌────────────┐ │
│      │ │   Chart    ││   Table    │ │
│      │ └────────────┘└────────────┘ │
└──────┴──────────────────────────────┘
```

```
<!-- Inside shell's <main> -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 lg:gap-6">
  <div class="lg:col-span-2"><!-- wide widget --></div>
  <div><!-- standard widget --></div>
  <div class="md:col-span-2 lg:col-span-3"><!-- full-width widget --></div>
</div>
```

Note: at `lg`, the content region inside a sidebar shell is roughly `viewport width - 256px`. A three-column grid that works in a full-width shell may need to become two columns inside a sidebar shell. Test with realistic content.

### Common pitfalls

1. **Widgets are different heights, creating a ragged grid.** Either enforce consistent heights per row or use masonry-style auto-placement deliberately.
2. **All widgets load simultaneously, creating a blank screen.** Each widget should load independently with its own skeleton state (→ UX Standards §3.3, → component: skeleton.md).
3. **No empty state for individual widgets.** A widget with no data must show its own empty state, not drag down the whole dashboard (→ UX Standards §5).
4. **Stat cards wrap awkwardly between breakpoints.** The stat row should use a horizontal scroll at small sizes rather than wrapping to two rows.
5. **Dashboard has no clear visual hierarchy.** Place the most important metrics at the top. Do not give every widget equal visual weight.
6. **Grid assumes full viewport width.** Inside a sidebar shell, the available width is reduced. Column counts and breakpoints may need adjusting.

### Tailwind notes

```
<!-- Standalone -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 lg:gap-6">
  ...
</div>

<!-- Inside a shell — same markup, but test column counts at each breakpoint
     accounting for the reduced available width -->
```

---

## Content 3: Split view (master–detail)

### When to use

- Email and messaging interfaces
- Record lists with detail views (CRM contacts, support tickets, inventory items)
- Any workflow where the user selects from a list and views detail without leaving the list context

### When not to use

- Lists where the detail view is a full page of content (use navigation to a detail page instead)
- Mobile-primary products — the split view doesn't work on narrow screens and requires a fallback

### Anatomy

```
┌──────────────┬──────────────────────┐
│              │                      │
│   Master     │      Detail          │
│   (list)     │      (selected)      │
│              │                      │
│              │                      │
│  ▶ Item 1    │                      │
│    Item 2    │                      │
│    Item 3    │                      │
│              │                      │
└──────────────┴──────────────────────┘
```

1. **Master panel** — a scrollable list or table of records. Typical width: 320–400px, or 30–40% of the content region
2. **Detail panel** — shows the full content of the selected record. Fills remaining width
3. **Divider** — a visible border between panels. May be draggable to resize (optional)

### Responsive behaviour

| Breakpoint | Behaviour                                                                                                                                                    |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `xs`–`md`  | Split view is not used. Show the list as a full-width view. Selecting an item navigates to a full-width detail view with a back button to return to the list |
| `lg`       | Split view activates. Both panels visible side by side                                                                                                       |
| `xl`+      | Panels have more room. Detail panel may introduce its own content width constraint                                                                           |

The transition between stacked (mobile) and split (desktop) must preserve state — the selected item must remain selected when the viewport crosses the breakpoint.

### Scroll strategy

Both panels scroll independently within the shell's content region. Neither panel should cause the other to scroll.

### Variants

**Three-column split** — master list, detail, and a third panel for sub-detail or actions (e.g. email list → email content → reply composer). Use only on `xl`+ and collapse to two or one column below.

**Resizable panels** — the divider is draggable. Provide a minimum width for each panel (master: 240px, detail: 400px). Double-clicking the divider resets to default proportions.

### Inside a sidebar app shell

This is the standard layout for CRM, support ticket, and messaging views within an admin product.

```
┌──────┬──────────────┬──────────────┐
│      │              │              │
│ Side │   Master     │   Detail     │
│ bar  │   (list)     │   (selected) │
│      │              │              │
│      │  ▶ Item 1    │              │
│      │    Item 2    │              │
│      │    Item 3    │              │
│      │              │              │
└──────┴──────────────┴──────────────┘
```

```
<!-- Inside shell's <main>, replace default padding with flex layout -->
<div class="flex-1 flex min-h-0">
  <div class="w-80 border-r overflow-y-auto hidden lg:block">
    <!-- Master list -->
  </div>
  <div class="flex-1 overflow-y-auto p-4 sm:p-6 lg:p-8">
    <!-- Detail -->
  </div>
</div>
```

Important: the split view replaces the shell's default `<main>` padding and scroll behaviour. The shell's `<main>` should use `flex` and `min-h-0` instead of `overflow-y-auto` when hosting a split view, so that the master and detail panels manage their own scroll.

The master panel width needs to account for the sidebar. A 320px master panel + 256px sidebar = 576px consumed before the detail panel gets any space. On a 1024px viewport at the `lg` breakpoint, that leaves only 448px for detail — tight but workable. Test with real content.

### Common pitfalls

1. **No empty state for the detail panel.** When no item is selected, the detail panel must show a helpful empty state, not a blank white space.
2. **Mobile fallback loses the user's place.** When navigating from list to detail on mobile, ensure the back button returns to the exact scroll position in the list.
3. **Master panel is too wide or too narrow.** Test with real data. A master panel showing truncated titles or a detail panel that feels cramped means the proportions are wrong.
4. **Both panels scroll the shell's content region.** Each panel must be an independent scroll container. The shell's content region should not scroll when hosting a split view.
5. **Selected state is not visible in the master list.** The currently viewed item must be clearly highlighted in the list.
6. **Split view breakpoint doesn't account for the sidebar.** Inside a sidebar shell, the `lg` breakpoint gives less horizontal space. The split view may need to delay activation to `xl`, or the master panel may need to be narrower.

### Tailwind notes

```
<!-- Standalone (full viewport) -->
<div class="flex h-screen">
  <div class="w-80 border-r overflow-y-auto hidden lg:block">
    <!-- Master list -->
  </div>
  <div class="flex-1 overflow-y-auto">
    <!-- Detail -->
  </div>
</div>

<!-- Inside a sidebar shell — the shell provides h-screen and flex.
     The split view fills the shell's content region. -->
```

---

## Content 4: Main + aside

### When to use

- Detail pages where the primary content needs a supporting sidebar: a record detail with a summary panel, a form with contextual help, an article with metadata
- Settings or configuration pages where the main area has the form and the aside shows a preview or status
- Any page with a clear primary/secondary content relationship where the secondary content should remain visible while scrolling the primary

### When not to use

- Pages where both columns are equally important (use equal split instead)
- List-to-detail workflows where the left column is a selectable list (use split view / master–detail instead)
- Mobile-only products — the aside collapses below the main content and may lose its value

### Anatomy

```
┌──────────────────────┬─────────────┐
│                      │             │
│       Main           │   Aside     │
│       content        │   (fixed    │
│                      │    width)   │
│                      │             │
│                      │             │
│                      │             │
└──────────────────────┴─────────────┘
```

1. **Main content** — the primary area, fills remaining width. Scrolls if content overflows
2. **Aside** — fixed width (typically 280–360px), right-aligned. Contains supporting content: summaries, metadata, related items, contextual actions, or help text

The aside is not navigation — it is content that supports the main area. If it contains navigation, use the sidebar app shell instead.

### Responsive behaviour

| Breakpoint | Behaviour                                                                                          |
| ---------- | -------------------------------------------------------------------------------------------------- |
| `xs`–`md`  | Single column. Aside content moves below the main content, or collapses into an expandable section |
| `lg`       | Two-column layout activates. Main + aside side by side                                             |
| `xl`+      | More breathing room. Main content may introduce its own max-width constraint                       |

### Scroll strategy

Two options, chosen based on aside content length:

**Aside scrolls with main content** — the simplest model. Both columns sit in a single scrolling container. Use when the aside is short and the user doesn't need to reference it while scrolling deep into the main content.

**Aside is sticky** — the aside sticks to the top of the content region so it remains visible while the main content scrolls. Use when the aside contains information the user needs to reference continuously (e.g. a summary card, a live preview, or action buttons).

### Variants

**Left aside** — aside on the left, main content on the right. Less common but used when the supporting content is read before the main content (e.g. a table of contents or a filter panel that isn't navigation).

### Inside a sidebar app shell

```
┌──────┬──────────────────────┬──────┐
│      │                      │      │
│ Side │       Main           │Aside │
│ bar  │       content        │      │
│      │                      │      │
│      │                      │      │
└──────┴──────────────────────┴──────┘
```

```
<!-- Inside shell's <main> -->
<div class="lg:flex lg:gap-6 lg:items-start">
  <div class="flex-1 min-w-0">
    <!-- Main content -->
  </div>
  <aside class="lg:w-80 lg:flex-shrink-0 lg:sticky lg:top-0">
    <!-- Aside -->
  </aside>
</div>
```

Note: sidebar (256px) + aside (320px) = 576px consumed. On a 1024px viewport at `lg`, the main content gets 448px. This is tight — consider whether the aside should collapse to below-content at `lg` and only appear alongside at `xl`, or reduce the aside width.

### Common pitfalls

1. **Aside competes with the main content for attention.** The aside must be visually subordinate — lighter background, smaller text, or a card container that separates it from the main flow.
2. **Sticky aside extends beyond the viewport.** If the aside content is taller than the viewport, `position: sticky` will not work as expected. Either make the aside scroll internally (`overflow-y-auto` with a max-height) or let it scroll with the page.
3. **Aside disappears entirely on mobile.** The aside content should reflow below the main content, not vanish. If the aside contains actions (e.g. "Approve" button), those actions must still be reachable.
4. **Aside width squeezes the main content inside a sidebar shell.** Test at `lg` with real content. If the main area is too narrow, delay the aside layout to `xl` or reduce aside width.
5. **Main content and aside have no visual separation.** Use a border, gap, or background difference so the two areas are clearly distinct.

### Tailwind notes

```
<!-- Inside a shell's content region -->
<div class="lg:flex lg:gap-8 lg:items-start">
  <div class="flex-1 min-w-0">
    <!-- Main content -->
  </div>
  <aside class="mt-6 lg:mt-0 lg:w-80 lg:flex-shrink-0 lg:sticky lg:top-6">
    <!-- Aside: flows below on mobile, sticky sidebar on desktop -->
  </aside>
</div>
```

`min-w-0` on the main content prevents flex overflow. `lg:items-start` keeps the aside top-aligned when using sticky positioning.

---

## Content 5: Centered card

### When to use

- Authentication pages: login, register, forgot password, reset password, verify email
- Single-action screens: accept an invite, confirm a deletion, enter a code
- Onboarding steps that need focused attention with no distractions
- Any page where the entire task fits in a single, contained card

### When not to use

- Pages with more content than fits in a card (use single column instead)
- Pages that need navigation or a sidebar — the centered card is intentionally isolated
- Screens within the main app shell — centered card is typically used outside the shell (pre-auth, standalone flows)

### Anatomy

```
┌─────────────────────────────────────┐
│                                     │
│                                     │
│         ┌───────────────┐           │
│         │     Logo      │           │
│         │               │           │
│         │   Card        │           │
│         │   content     │           │
│         │               │           │
│         │   [Action]    │           │
│         └───────────────┘           │
│                                     │
│           Footer link               │
└─────────────────────────────────────┘
```

1. **Background** — fills the full viewport. May be a solid colour, gradient, or subtle pattern. Keeps focus on the card
2. **Card** — centred both horizontally and vertically. Fixed max-width (typically 400–480px). Contains the form or action
3. **Logo** — above or inside the card. The only branding element on the page
4. **Footer link** — below the card. Secondary actions: "Back to login", "Don't have an account?", legal links

### Responsive behaviour

| Breakpoint | Behaviour                                                                                                                                                                                           |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `xs`–`sm`  | Card expands to full width with horizontal padding (16–24px). Vertical centering may shift to top-aligned with top padding to ensure the card is visible without scrolling. Background may simplify |
| `md`+      | Card is centred horizontally and vertically at its max-width                                                                                                                                        |

### Scroll strategy

No scroll in most cases — the card should fit within the viewport. If the card content is long (e.g. a registration form with many fields), the page scrolls as a single unit with the card remaining centred horizontally.

### Variants

**Card with side illustration** — at wider breakpoints, the card sits alongside a branded illustration or image in a 50/50 or 40/60 split. On mobile, the illustration is hidden or reduced to a small header image.

```
┌──────────────────┬──────────────────┐
│                  │                  │
│   Illustration   │   ┌──────────┐   │
│   or branded     │   │  Card    │   │
│   panel          │   │  content │   │
│                  │   └──────────┘   │
│                  │                  │
└──────────────────┴──────────────────┘
```

### Common pitfalls

1. **Card is not vertically centred.** Use `min-h-screen` with flexbox centering (`items-center justify-center`). Do not rely on margin hacks or fixed top positioning.
2. **Card does not adapt on mobile.** At `xs`, the card should drop its fixed width and fill the screen with padding. A 400px card on a 375px screen overflows.
3. **Form within the card does not handle errors well.** Space is tight — validation messages must be inline and concise. The card height will grow, which is fine, but it must not overflow the viewport without scrolling.
4. **No way to navigate away.** Even isolated screens need an escape: "Back to login", a logo link to the marketing site, or at minimum a browser back action that works.
5. **Background competes with the card.** The background must be subtle. High-contrast patterns, busy images, or strong gradients pull attention from the form.

### Tailwind notes

```
<!-- Standalone centered card (no shell) -->
<div class="min-h-screen flex flex-col items-center justify-center px-4 bg-gray-50">
  <div class="w-full max-w-md">
    <div class="text-center mb-8">
      <!-- Logo -->
    </div>
    <div class="bg-white rounded-lg shadow-md p-6 sm:p-8">
      <!-- Card content -->
    </div>
    <p class="mt-4 text-center text-sm text-gray-500">
      <!-- Footer link -->
    </p>
  </div>
</div>

<!-- Variant: card with side illustration -->
<div class="min-h-screen flex">
  <div class="hidden lg:flex lg:w-1/2 bg-brand-600 items-center justify-center">
    <!-- Illustration panel -->
  </div>
  <div class="flex-1 flex items-center justify-center px-4">
    <div class="w-full max-w-md">
      <!-- Card -->
    </div>
  </div>
</div>
```

---

## Content 6: Equal split

### When to use

- Comparison views: side-by-side diffs, before/after, plan comparison
- Dual-input workflows: source and destination, original and translation, code and preview
- Collaborative views: two datasets or contexts that the user needs to work with simultaneously
- Any page where two content areas are equally important and neither is subordinate

### When not to use

- When one side is clearly primary and the other is supporting (use main + aside instead)
- When the left column is a selectable list driving the right column (use split view / master–detail instead)
- Mobile-only products — two equal columns on a narrow screen is not usable

### Anatomy

```
┌──────────────────┬──────────────────┐
│                  │                  │
│    Left panel    │   Right panel    │
│                  │                  │
│                  │                  │
│                  │                  │
│                  │                  │
└──────────────────┴──────────────────┘
```

1. **Left panel** — 50% of available width
2. **Right panel** — 50% of available width
3. **Divider** — a visible border or gap between panels. May be draggable if the user benefits from adjusting the ratio

Both panels have equal visual weight. Neither has a header that the other doesn't — if one panel has a title bar, both should.

### Responsive behaviour

| Breakpoint | Behaviour                                                                                                                                                                                                       |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `xs`–`md`  | Single column. Panels stack vertically. The order should reflect the natural workflow — typically left above right, but this depends on context (e.g. in a translation tool, the source language may go on top) |
| `lg`       | Two equal columns side by side                                                                                                                                                                                  |
| `xl`+      | Panels have more room. Content within each panel may introduce its own max-width or padding                                                                                                                     |

### Scroll strategy

Two options:

**Independent scroll** — each panel scrolls independently. Use when the panels contain different-length content and the user needs to position each panel's scroll independently (e.g. comparing two documents at different points).

**Synchronised scroll** — both panels scroll together. Use when the content is directly aligned (e.g. side-by-side diff, before/after). Requires JavaScript to sync scroll positions.

**Page scroll** — both panels sit in the normal page flow and scroll as one unit. Use when both panels are short and the user views them as a single page, not two independent contexts.

### Variants

**Unequal split** — 60/40 or 40/60 instead of 50/50. One panel is slightly more important or typically has more content. Still distinct from main + aside because neither side is a supporting sidebar — both are working areas.

**Split with shared toolbar** — a toolbar above both panels that contains actions affecting both (e.g. "Swap sides", "Reset", "Save"). The toolbar spans the full width.

```
┌─────────────────────────────────────┐
│             Toolbar                 │
├──────────────────┬──────────────────┤
│    Left panel    │   Right panel    │
│                  │                  │
└──────────────────┴──────────────────┘
```

### Inside a sidebar app shell

```
┌──────┬──────────────────┬──────────┐
│      │                  │          │
│ Side │   Left panel     │  Right   │
│ bar  │                  │  panel   │
│      │                  │          │
│      │                  │          │
└──────┴──────────────────┴──────────┘
```

```
<!-- Inside shell's <main> -->
<div class="flex flex-col lg:flex-row lg:h-full min-h-0">
  <div class="flex-1 min-w-0 overflow-y-auto border-b lg:border-b-0 lg:border-r">
    <!-- Left panel -->
  </div>
  <div class="flex-1 min-w-0 overflow-y-auto">
    <!-- Right panel -->
  </div>
</div>
```

The available width after the sidebar is split equally. On a 1024px screen with a 256px sidebar, each panel gets ~384px. This is workable for many use cases but tight for content-heavy panels. Test with real content and consider delaying the split to `xl` if needed.

### Common pitfalls

1. **Panels are not actually equal.** If using flexbox, both panels must use `flex-1`. Do not give one a fixed width and the other `flex-1` — that creates a main + aside, not an equal split.
2. **Stacking order on mobile is wrong.** When panels stack vertically, the order must follow the user's workflow. If the right panel is the "output" of the left panel, left should stack on top.
3. **No visual separation between panels.** Without a border or gap, two equal panels look like one wide area with a layout glitch. Always separate them visually.
4. **Synchronised scroll is not smooth.** If syncing scroll positions via JavaScript, use `requestAnimationFrame` and throttle the scroll handler. Laggy sync is worse than no sync.
5. **Content overflows horizontally within panels.** Each panel must handle its own overflow. Long text wraps, wide tables scroll horizontally within the panel, and code blocks use horizontal scroll or wrapping.
6. **Panels don't fill the available height.** For independent-scroll panels inside a shell, both panels must stretch to fill the content region height. Use `h-full` or flex layout.

### Tailwind notes

```
<!-- Independent scroll (inside a shell's content region) -->
<div class="flex flex-col lg:flex-row lg:h-full min-h-0">
  <div class="flex-1 min-w-0 overflow-y-auto p-4 sm:p-6 border-b lg:border-b-0 lg:border-r">
    <!-- Left panel -->
  </div>
  <div class="flex-1 min-w-0 overflow-y-auto p-4 sm:p-6">
    <!-- Right panel -->
  </div>
</div>

<!-- Page scroll (simpler, no independent scroll) -->
<div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
  <div><!-- Left panel --></div>
  <div><!-- Right panel --></div>
</div>
```

`min-w-0` on both panels prevents flex children from overflowing when content (e.g. code blocks or tables) is wider than 50%.

---

## Content 7: Full bleed / canvas

### When to use

- Map interfaces
- Visual editors (design tools, diagram builders, whiteboard)
- Media viewers (image galleries, video players)
- Any interface where the primary content needs every available pixel

### When not to use

- Text-heavy content — reading full-bleed text is unpleasant
- Standard CRUD applications — they don't need the space and will look empty

### Anatomy

```
┌─────────────────────────────────────┐
│  ┌─ Toolbar ──────────────────────┐ │
│  └────────────────────────────────┘ │
│                                     │
│            Canvas                   │
│       (fills content region)        │
│                                     │
│                          ┌────────┐ │
│                          │ Panel  │ │
│                          └────────┘ │
└─────────────────────────────────────┘
```

1. **Canvas** — the primary content area, filling the entire content region
2. **Toolbar** — floating or pinned to an edge, overlaying the canvas. Contains tools and actions
3. **Panels** — floating or slide-out panels for properties, layers, settings. Overlay the canvas rather than displacing it

### Responsive behaviour

| Breakpoint | Behaviour                                                                                                                                         |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `xs`–`sm`  | Toolbar condenses to essential tools. Panels become full-screen overlays or bottom sheets. Touch gestures (pinch, pan) replace mouse interactions |
| `md`       | Toolbar shows more tools. Panels may appear as slide-overs                                                                                        |
| `lg`+      | Full toolbar. Panels can dock to sides without obscuring too much canvas                                                                          |

### Scroll strategy

No scroll in the content region. The canvas handles its own pan/zoom/scroll behaviour. Set `overflow: hidden` on the content region container.

### Inside a sidebar app shell

Common for tools that pair persistent navigation (sidebar) with a visual workspace.

```
┌──────┬──────────────────────────────┐
│      │ ┌─ Toolbar ───────────────┐  │
│ Side │ └─────────────────────────┘  │
│ bar  │                              │
│      │         Canvas               │
│      │                              │
│      │                   ┌────────┐ │
│      │                   │ Panel  │ │
│      │                   └────────┘ │
└──────┴──────────────────────────────┘
```

```
<!-- Inside shell's <main>, replace default scroll with overflow-hidden -->
<div class="flex-1 relative overflow-hidden">
  <div class="absolute inset-0">
    <!-- Canvas -->
  </div>
  <div class="absolute top-4 left-4 z-10">
    <!-- Toolbar -->
  </div>
</div>
```

### Common pitfalls

1. **Browser scroll appears.** The canvas must prevent any scrolling in the content region. Set `overflow: hidden` on the container.
2. **Floating UI elements are not reachable on mobile.** Toolbars and panels must be large enough to tap (→ UX Standards §13.6 — 44px touch targets) and must not overlap each other.
3. **No keyboard access to canvas tools.** Keyboard shortcuts must be documented and discoverable (→ component: command-palette.md).
4. **Canvas does not respect reduced motion.** Pan and zoom animations must respect `prefers-reduced-motion` (→ UX Standards §13.9).

### Tailwind notes

```
<!-- Standalone (full viewport) -->
<div class="h-screen w-screen overflow-hidden relative">
  <div class="absolute inset-0"><!-- Canvas --></div>
  <div class="absolute top-4 left-4 z-10"><!-- Toolbar --></div>
  <div class="absolute top-4 right-4 z-10"><!-- Panel --></div>
</div>

<!-- Inside a shell — fills the content region instead of the viewport -->
```

---

## Content 8: Multi-panel (workspace)

### When to use

- IDE-style interfaces
- Data exploration tools (query builder + results + schema browser)
- Complex operational tools where the user works across multiple data contexts simultaneously
- Any interface where power users need to see 3+ information panels at once

### When not to use

- Most applications — this pattern is complex and overwhelming for casual users
- Mobile-targeted products — this does not adapt to small screens gracefully

### Anatomy

```
┌─────────┬───────────────────┬───────┐
│         │     Toolbar       │       │
│         ├───────────────────┤       │
│         │                   │ Right │
│  Left   │    Primary        │ panel │
│  panel  │    content        │       │
│         │                   │       │
│         ├───────────────────┤       │
│         │  Bottom panel     │       │
└─────────┴───────────────────┴───────┘
```

1. **Left panel** — navigation, file tree, or entity browser
2. **Primary content** — the main working area
3. **Right panel** — properties, details, or contextual information
4. **Bottom panel** — output, logs, terminal, or secondary results
5. **Toolbar** — actions relevant to the primary content

All panels are typically resizable and collapsible.

### Responsive behaviour

| Breakpoint | Behaviour                                                                                                               |
| ---------- | ----------------------------------------------------------------------------------------------------------------------- |
| `xs`–`md`  | This pattern should not be used. Provide a simplified single-panel mobile view with navigation to switch between panels |
| `lg`       | Two panels visible (left + primary, or primary + right). Other panels available as toggleable overlays                  |
| `xl`+      | Three or four panels visible. Full workspace layout                                                                     |

### Scroll strategy

Each panel scrolls independently. No scroll on the content region container. The layout fills the available height completely.

### Panel management

- Each panel must be independently collapsible via a toggle
- Collapsed panels should show a narrow strip (icon or label) that can be clicked to expand
- Panel sizes should persist across sessions (store in local storage)
- Minimum and maximum widths must be enforced during resize
- Double-clicking a resize handle should reset the panel to its default size

### Inside a sidebar app shell

This combination creates a very dense interface (sidebar + 3–4 panels). Use with caution — it is appropriate for professional tools (IDE, data platform) but overwhelming for general admin products.

```
┌──────┬──────┬───────────────┬───────┐
│      │      │   Toolbar     │       │
│      │      ├───────────────┤       │
│ Side │ Left │               │ Right │
│ bar  │ panel│   Primary     │ panel │
│      │      │               │       │
│      │      ├───────────────┤       │
│      │      │ Bottom panel  │       │
└──────┴──────┴───────────────┴───────┘
```

The sidebar provides application-level navigation. The workspace panels provide tool-level navigation and context. These are distinct concerns — do not merge the sidebar into the left panel.

### Common pitfalls

1. **Too many panels open by default.** Start with a sensible default (two panels visible) and let users open more. Do not overwhelm on first load.
2. **Panel resize is janky.** Use CSS Grid or a resize observer, not JavaScript-driven width calculations on mousemove.
3. **Panels have no minimum width.** A panel resized to 50px wide is useless. Enforce minimums (typically 200px for content panels, 48px for collapsed state).
4. **No keyboard shortcut to toggle panels.** Power users (the audience for this pattern) expect keyboard control. Provide shortcuts and list them in the command palette.
5. **Mobile has no fallback.** Do not serve this layout on mobile. Provide a different experience.

### Tailwind notes

CSS Grid is the right tool for this pattern. Flexbox becomes unwieldy with multiple resizable regions.

```
<!-- Inside shell's content region -->
<div class="flex-1 grid grid-cols-[auto_1fr_auto] grid-rows-[auto_1fr_auto] min-h-0">
  <aside class="row-span-3 w-64 border-r overflow-y-auto">
    <!-- Left panel -->
  </aside>
  <header class="border-b"><!-- Toolbar --></header>
  <main class="overflow-y-auto"><!-- Primary content --></main>
  <div class="border-t overflow-y-auto h-48"><!-- Bottom panel --></div>
  <aside class="row-span-3 w-72 border-l overflow-y-auto">
    <!-- Right panel -->
  </aside>
</div>
```

---

## Content 9: Stacked sections (landing page)

### When to use

- Marketing and product landing pages
- Onboarding flows that tell a story
- Feature overview pages
- Any long-form page where content is organised in distinct, full-width sections

### When not to use

- Application screens — this is a content pattern, not an app pattern
- Pages with interactive data — stacked sections are for reading, not operating

### Anatomy

```
┌─────────────────────────────────────┐
│           Hero section              │
├─────────────────────────────────────┤
│           Section 1                 │
├─────────────────────────────────────┤
│     Section 2 (alt background)      │
├─────────────────────────────────────┤
│           Section 3                 │
├─────────────────────────────────────┤
│              CTA                    │
├─────────────────────────────────────┤
│              Footer                 │
└─────────────────────────────────────┘
```

1. **Sections** — full-width containers with alternating backgrounds for visual rhythm. Each section has its own content width constraint
2. **Hero** — the first section, typically taller, with a headline and primary CTA
3. **CTA section** — a focused call-to-action near the bottom, before the footer

This pattern is typically used standalone (with a top nav shell or no shell at all) rather than inside a sidebar app shell.

### Responsive behaviour

| Breakpoint | Behaviour                                                                                                                                   |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `xs`–`sm`  | Sections stack vertically. Multi-column content within sections becomes single column. Hero image may move below hero text. Padding reduces |
| `md`       | Two-column layouts within sections. Hero may show image alongside text                                                                      |
| `lg`+      | Full layout. Sections use generous vertical padding for breathing room                                                                      |

### Scroll strategy

Full-page scroll. No sticky elements beyond the header. Scroll-triggered animations (fade in, slide up) are acceptable here but must respect `prefers-reduced-motion` (→ UX Standards §13.9).

### Common pitfalls

1. **Sections have no vertical rhythm.** Use consistent vertical padding across sections (e.g. `py-16 md:py-24 lg:py-32`). Inconsistent spacing looks haphazard.
2. **Content width is not constrained within sections.** Sections span full width for background colour, but the content inside must be width-constrained.
3. **Too many sections.** Each section competes for attention. If the page has more than 6–8 sections, consider whether some content can be consolidated or moved to sub-pages.
4. **Hero has no clear hierarchy.** The hero must have one headline, one supporting line, and one primary CTA. Multiple competing messages dilute impact.

### Tailwind notes

```
<!-- Section pattern -->
<section class="bg-white">
  <div class="max-w-screen-xl mx-auto px-4 sm:px-6 lg:px-8 py-16 md:py-24">
    <!-- Section content -->
  </div>
</section>

<section class="bg-gray-50">
  <div class="max-w-screen-xl mx-auto px-4 sm:px-6 lg:px-8 py-16 md:py-24">
    <!-- Alternating background -->
  </div>
</section>
```

---

# C. Choosing and combining patterns

## Pattern selection guide

| Page type                  | Shell                           | Content area pattern            |
| -------------------------- | ------------------------------- | ------------------------------- |
| Login / register           | None (standalone)               | Centered card                   |
| Forgot password / verify   | None (standalone)               | Centered card                   |
| Dashboard / overview       | Sidebar (+ bottom nav mobile)   | Dashboard grid                  |
| Data list with detail      | Sidebar (+ bottom nav mobile)   | Split view (master–detail)      |
| Data list (simple)         | Sidebar, top nav, or bottom nav | Single column (wide) with table |
| Record detail with summary | Sidebar (+ bottom nav mobile)   | Main + aside                    |
| Form with contextual help  | Sidebar (+ bottom nav mobile)   | Main + aside                    |
| Settings (many sections)   | Sidebar (+ bottom nav mobile)   | Single column (wide)            |
| Settings (few sections)    | Top nav or bottom nav           | Single column                   |
| Form / wizard              | Sidebar, top nav, or bottom nav | Single column                   |
| Documentation              | Sidebar app shell               | Single column (wide)            |
| Comparison / diff          | Sidebar (+ bottom nav mobile)   | Equal split                     |
| Translation / dual input   | Sidebar (+ bottom nav mobile)   | Equal split                     |
| Map / editor               | Sidebar (+ bottom nav mobile)   | Full bleed / canvas             |
| IDE / power tool           | Sidebar app shell               | Multi-panel                     |
| Landing / marketing        | Top nav shell or none           | Stacked sections                |
| Email / messaging          | Sidebar (+ bottom nav mobile)   | Split view (master–detail)      |
| Profile / account          | Sidebar, top nav, or bottom nav | Single column                   |
| Mobile-first app           | Bottom nav (+ sidebar desktop)  | Varies by page                  |

## Nesting rules

1. **One shell per application.** Do not nest a sidebar shell inside a top nav shell or vice versa. Pick one. A sidebar that switches to a bottom nav at mobile breakpoints is one responsive shell, not two
2. **Content area patterns fill the shell's content region.** They do not set their own viewport height or include global navigation
3. **Content area patterns can be mixed per route.** The dashboard page uses a dashboard grid, the tickets page uses a split view, the settings page uses a single column — all within the same sidebar shell
4. **Some content patterns modify the shell's content region.** Split view and canvas patterns replace the shell's default padding and scroll behaviour. The shell must accommodate this — typically by making the content region a flex container that content patterns can control
5. **Do not nest content area patterns inside each other.** A dashboard grid inside a split view inside a sidebar shell is too deep. If a design requires this, reconsider the information architecture
