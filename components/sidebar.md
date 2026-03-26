# Sidebar

> A vertical navigation panel typically anchored to the left side of the screen. Used for applications with deeper navigation hierarchies or many sections.

---

## When to use
- Applications with 8+ navigation sections
- Multi-level navigation (sections with sub-items)
- Dashboards, admin panels, and tools where the user moves between many views frequently
- When navigation needs to be always visible for orientation

## When not to use
- Simple sites with 5 or fewer top-level pages — use Header nav
- Content-focused sites where maximising reading width matters — sidebar takes space
- Mobile-only apps — use bottom nav or drawer pattern instead

---

## Anatomy

1. **Container** — vertical panel, fixed or sticky on the left. Required
2. **Logo / brand** — at the top. Optional (may be in header instead)
3. **Nav items** — links with icon + label. Required
4. **Section dividers** — visual separators or section labels for grouped nav items. Optional
5. **Sub-items** — nested links revealed under a parent item. Optional
6. **Collapse toggle** — button to collapse the sidebar to icon-only mode. Recommended
7. **Footer area** — user menu, settings, or help link at the bottom. Optional

---

## Variants

### Expanded (default)
- **Purpose:** full sidebar with icons and labels
- **Visual distinction:** 240–280px wide, labels visible alongside icons

### Collapsed / icon-only
- **Purpose:** maximise content area while keeping navigation accessible
- **Visual distinction:** 64–72px wide, icons only. Labels appear in tooltips on hover. Sub-items may appear as flyout menus

### Overlay (mobile)
- **Purpose:** sidebar as a drawer on small screens
- **Visual distinction:** slides in from the left over content, with a backdrop. Triggered by a hamburger or toggle

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Expanded, showing current section highlighted | |
| Collapsed | Icon-only, tooltips on hover | |
| Active item | Highlighted background, bold text or accent colour | |
| Hover | Background tint on hovered item | |
| Sub-items expanded | Parent item shows expanded indicator, children visible and indented | |
| Mobile overlay | Sidebar slides in, backdrop covers content | |

---

## Interaction spec

### Mouse / pointer
- Click nav items to navigate. Click parent items to expand/collapse sub-items
- Click collapse toggle to switch between expanded and icon-only
- In icon-only mode, hover shows tooltip with label. Click navigates directly; items with children show flyout sub-menu on hover or click

### Keyboard
- `Tab` moves through nav items top to bottom
- `Enter` activates links or toggles sub-item expansion
- `Escape` closes mobile overlay
- `ArrowDown` / `ArrowUp` for moving within the nav (optional enhancement)

### Touch
- Mobile: swipe right from edge to open (optional), tap hamburger to open
- Tap backdrop to close overlay
- Nav items are full-width tap targets

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Sidebar hidden. Triggered as an overlay/drawer via hamburger. Backdrop covers content |
| `md` | May show as collapsed/icon-only by default |
| `lg`+ | Expanded sidebar visible. Content area adjusts width |

→ UX Standards §16.4

The main content area must adjust its left margin or padding based on sidebar state to prevent overlap.

---

## Accessibility

- **Role / semantics:** `<nav aria-label="Sidebar navigation">` or `<aside>` containing a `<nav>`
- **Current page:** `aria-current="page"` on the active nav item
- **Collapse state:** toggle button has `aria-expanded` and `aria-label="Collapse sidebar"` / `"Expand sidebar"`
- **Sub-items:** parent item has `aria-expanded`. Sub-item list uses `role="group"` or nested `<ul>`
- **Mobile overlay:** focus trap while open. Focus returns to trigger on close
- **Icon-only mode:** every nav item must retain its accessible name via `aria-label` even when the text label is hidden
- **WCAG:** 2.4.1, 2.4.8, 4.1.2

---

## Motion & animation

- **Collapse/expand:** width transition, 200ms ease-in-out. Labels fade out/in
- **Mobile overlay:** slide from left, 250ms ease-out. Backdrop fades in simultaneously
- **Sub-item expand:** height reveal with ease-out, 150ms
- **Reduced motion:** instant width/visibility changes → UX Standards §13.9

---

## Common pitfalls

1. **Content area doesn't reflow** — when the sidebar collapses or expands, the main content must adjust its width. Use CSS grid or margin transitions, not fixed widths
2. **Icon-only mode loses labels** — every item must have `aria-label` and a tooltip in collapsed mode
3. **No mobile treatment** — a sidebar that stays visible on mobile steals 240px of a 375px screen. Switch to an overlay
4. **Sub-items always expanded** — if the sidebar has many items, keep sub-items collapsed by default. Only the active section's sub-items should be expanded
5. **Sidebar fights the header** — decide whether the sidebar sits below the header or extends full-height. Either works, but mixing creates layout conflicts
6. **Missing focus management on mobile** — when the mobile sidebar opens, trap focus inside it. When it closes, return focus to the trigger

---

## Tailwind notes

Container:
- Expanded: `fixed left-0 top-0 bottom-0 w-64 bg-white border-r z-30 flex flex-col`
- Collapsed: `w-16` with `overflow-hidden` on labels or use conditional rendering
- Mobile: same fixed positioning but add `translate-x-0` (open) / `-translate-x-full` (closed) with `transition-transform`

Backdrop:
- `fixed inset-0 bg-black/50 z-20` shown when mobile sidebar is open

Nav items:
- `flex items-center gap-3 px-3 py-2.5 rounded-md text-sm transition-colors`
- Active: `bg-gray-100 text-[var(--color-action-primary)] font-medium`
- Icon: `h-5 w-5 flex-shrink-0`

Content offset:
- `ml-64 lg:ml-64 md:ml-16` (adjust based on sidebar state). Or use CSS grid: `grid-cols-[var(--sidebar-width)_1fr]`
