# Tabs

> A set of selectable controls that switch between views or content panels within the same page context. Tabs keep the user on the same page while revealing different facets of content.

---

## When to use
- Switching between related content views without a page reload (e.g. Overview / Activity / Settings)
- When the user needs to compare or move between sections frequently
- Organising form sections or detail views

## When not to use
- Navigating between unrelated pages — use standard navigation links
- When the user needs to see content from multiple tabs simultaneously — use Accordion or a single scrollable page
- More than 7 tabs — consider a different navigation pattern (sidebar, dropdown)

---

## Anatomy

1. **Tab list** — horizontal row of tab items. Required
2. **Tab item** — individual selectable tab. Required
3. **Active indicator** — underline, background, or border highlighting the selected tab. Required
4. **Tab panel** — the content area associated with the selected tab. Required
5. **Tab icon** — optional icon alongside the tab label. Optional
6. **Tab badge/count** — number or status indicator on a tab. Optional

---

## Variants

### Underlined
- **Purpose:** default, lightweight tab style
- **Visual distinction:** active tab has a bottom border highlight. No background fill

### Boxed / contained
- **Purpose:** tabs that look like cards or segments
- **Visual distinction:** active tab has a filled background, others are transparent

### Pill / segment
- **Purpose:** segmented control for toggling between 2–4 views
- **Visual distinction:** rounded pill containers, selected pill filled. Commonly used for filters or view toggles

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default (inactive) | Muted text, no indicator | |
| Hover | Slight background or text colour change | |
| Active / Selected | Bold/coloured text + active indicator | |
| Focus | Focus ring on the tab | → UX Standards §13.5 |
| Disabled | Muted text, not selectable | Rare — prefer hiding tabs over disabling |

---

## Interaction spec

### Mouse / pointer
- Click a tab to switch to its panel. Active indicator moves to the clicked tab

### Keyboard
- `Tab` focuses the tab list, landing on the currently active tab
- `ArrowLeft` / `ArrowRight` moves focus between tabs (and optionally selects — two patterns exist)
- `Enter` or `Space` activates the focused tab (if using manual activation)
- `Home` / `End` focuses the first/last tab
- `Tab` again exits the tab list and enters the active panel content

Two valid patterns:
1. **Automatic activation** — arrow keys both move focus and change the active tab. Simpler, recommended for most cases
2. **Manual activation** — arrow keys move focus, Enter/Space activates. Better when tab switching triggers heavy data loads

### Touch
- Tap targets must be comfortable. Tabs may need horizontal scroll at smaller sizes

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Tabs may overflow horizontally with scroll. Show a scroll affordance (fade/arrow). Alternatively, switch to a dropdown or stacked accordion pattern |
| `md`+ | Full tab list visible. Evenly spaced or content-width tabs |

Never wrap tabs to multiple rows — this breaks the mental model. If tabs overflow, scroll horizontally or rethink the pattern.

---

## Accessibility

- **Role / semantics:** tab list: `role="tablist"`. Each tab: `role="tab"` with `aria-selected`. Each panel: `role="tabpanel"` with `aria-labelledby` referencing its tab
- **Association:** each tab has `aria-controls` pointing to its panel's id
- **Keyboard pattern:** roving tabindex — only the active tab has `tabindex="0"`, others have `tabindex="-1"`
- **Panel focus:** when a tab is activated, focus may stay on the tab (recommended) or move to the panel
- **WCAG:** 4.1.2, 2.1.1, 1.3.1

---

## Motion & animation

- **Active indicator:** slides or morphs to the new tab position, 200ms ease-out. This movement reinforces the spatial relationship between tabs
- **Panel transition:** crossfade or subtle slide between panels, 150–200ms. Do not animate if content loads asynchronously — show a skeleton instead
- **Reduced motion:** instant indicator move, no panel animation → UX Standards §13.9

---

## Common pitfalls

1. **Tabs that navigate to different pages** — tabs should switch content within the same page. If clicking a tab changes the URL and reloads, use nav links instead
2. **Wrapping tabs to multiple rows** — destroys the tab metaphor. Scroll or reduce tab count
3. **No keyboard support** — the roving tabindex pattern is essential. Don't make each tab a regular tab stop
4. **Tab panels without association** — each panel must be linked to its tab via `aria-controls` and `aria-labelledby`
5. **Content flash between tabs** — if tab content loads asynchronously, show a skeleton in the panel, not a full-page spinner
6. **All tabs have the same content type with different filters** — this is really a filter pattern, not tabs. Use Filter chips or a Select instead

---

## Tailwind notes

Tab list:
- `flex border-b` (underlined) or `flex bg-gray-100 rounded-lg p-1` (pill/segment)
- Scroll on overflow: `overflow-x-auto scrollbar-hide`

Tab item:
- Underlined: `px-4 py-2.5 text-sm font-medium border-b-2 transition-colors`
- Active: `border-[var(--color-action-primary)] text-[var(--color-action-primary)]`
- Inactive: `border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-300`

Pill variant:
- `px-3 py-1.5 rounded-md text-sm font-medium`
- Active: `bg-white shadow text-gray-900`
- Inactive: `text-gray-500 hover:text-gray-700`

Tab panel:
- `pt-4` (or appropriate spacing below the tab list)
- Use `hidden` on inactive panels (not `display:none` via JS after mount — render conditionally or use visibility)
