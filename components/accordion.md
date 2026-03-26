# Accordion

> Collapsible content sections that expand and collapse to show or hide content. Reduces visual clutter by letting users reveal information progressively.

---

## When to use
- FAQ pages or help sections
- Settings panels with many grouped options
- Long forms broken into collapsible sections
- Any content where the user doesn't need to see everything at once

## When not to use
- Content the user must see — don't hide critical information behind a click
- Switching between mutually exclusive views — use Tabs
- Very short content — the overhead of collapsing isn't worth it for a single sentence

---

## Anatomy

1. **Accordion container** — wrapper for all items. Required
2. **Item header / trigger** — clickable bar with title and expand indicator. Required
3. **Expand icon** — chevron or plus/minus icon indicating collapsible behaviour. Required
4. **Item content panel** — the content revealed on expansion. Required
5. **Dividers** — borders between items. Optional

---

## Variants

### Single expand
- **Purpose:** only one item open at a time. Opening one closes the others
- **Visual distinction:** standard accordion behaviour

### Multi expand
- **Purpose:** multiple items can be open simultaneously
- **Visual distinction:** each item operates independently

### Flush / borderless
- **Purpose:** integrated into a page without card-like borders
- **Visual distinction:** no outer border or background. Items separated by dividers only

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Collapsed | Header visible, content hidden. Chevron pointing right or down | |
| Expanded | Content visible. Chevron rotated (pointing down or up) | |
| Hover | Slight background change on the header | |
| Focus | Focus ring on the header/trigger | → UX Standards §13.5 |
| Disabled | Muted header, not expandable | Rare |

---

## Interaction spec

### Mouse / pointer
- Click the header to toggle expand/collapse
- The entire header row is clickable, not just the icon

### Keyboard
- `Enter` or `Space` toggles the focused accordion item
- `Tab` moves between accordion headers (and into expanded content)
- Optionally: `ArrowUp` / `ArrowDown` moves between headers, `Home` / `End` for first/last

### Touch
- Full header row is the tap target

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| All | Accordions work well at all sizes. No breakpoint-specific changes needed |

Accordions are inherently mobile-friendly since they stack vertically and manage content density.

---

## Accessibility

- **Role / semantics:** header triggers: `<button>` with `aria-expanded="true/false"` and `aria-controls` pointing to the content panel. Content panel: `role="region"` with `aria-labelledby` referencing the header
- **Heading level:** wrap the trigger button inside an appropriate heading element (e.g. `<h3><button>...</button></h3>`) for document structure
- **Keyboard:** full button keyboard behaviour on triggers
- **WCAG:** 4.1.2, 2.1.1, 1.3.1

---

## Motion & animation

- **Expand:** height reveals from 0 to content height, 200ms ease-out. Use `grid-template-rows: 0fr → 1fr` or JS height calculation for smooth animation
- **Collapse:** reverse, 150ms ease-in
- **Chevron rotation:** 200ms, synchronised with expand/collapse
- **Reduced motion:** instant show/hide, no height animation → UX Standards §13.9

---

## Common pitfalls

1. **Hiding essential content** — if users need the information to complete a task, don't hide it in an accordion
2. **Using `<div>` as trigger without button semantics** — the trigger must be a `<button>` (or have `role="button"`) for keyboard and screen reader support
3. **Content height animation jank** — animating `height: auto` is tricky. Use `max-height` with a generous value, or CSS grid row animation, or JS measurement
4. **No `aria-expanded` on triggers** — screen readers need to know whether an item is open or closed
5. **Accordion items that are always open** — if all items start expanded, the accordion adds no value. Consider whether the content should just be on the page

---

## Tailwind notes

Container:
- `divide-y rounded-lg border`

Header/trigger:
- `flex w-full items-center justify-between px-4 py-3 text-left text-sm font-medium hover:bg-gray-50 transition-colors`

Chevron:
- `h-5 w-5 text-gray-400 transition-transform` with `rotate-180` when expanded

Content panel:
- `grid transition-[grid-template-rows] duration-200`
- Collapsed: `grid-rows-[0fr]` with inner content in `overflow-hidden`
- Expanded: `grid-rows-[1fr]`
- Inner: `overflow-hidden px-4 pb-3`
