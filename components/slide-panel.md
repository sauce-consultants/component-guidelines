# Slide Panel / Drawer

> A side-anchored overlay panel that slides in from the edge of the screen. Used for detail views, forms, and secondary content that needs more space than a modal but shouldn't leave the current context.

---

## When to use
- Viewing or editing a record's details without leaving a list view
- Multi-step forms or configuration panels
- Supplementary content (filters, settings, help) that augments the current view
- When the user needs to reference the page content behind the panel

## When not to use
- Brief confirmations — use Modal
- Quick feedback — use Toast
- Full workflows that deserve their own page
- Mobile navigation menus — these are functionally drawers but see sidebar.md for nav-specific guidance

---

## Anatomy

1. **Backdrop** — semi-transparent overlay. Optional (some panels don't obscure the page)
2. **Panel container** — the sliding surface, anchored to an edge. Required
3. **Header** — title, close button, optional actions. Required
4. **Body** — scrollable content area. Required
5. **Footer** — action buttons (save, cancel). Optional
6. **Resize handle** — draggable edge to adjust panel width. Optional

---

## Variants

### Right-anchored (default)
- **Purpose:** detail views, edit forms
- **Visual distinction:** slides in from the right edge

### Left-anchored
- **Purpose:** navigation drawers, filter panels
- **Visual distinction:** slides in from the left

### Bottom-anchored (mobile)
- **Purpose:** mobile-optimised panels and action sheets
- **Visual distinction:** slides up from the bottom. Often partial-height with a drag handle to expand

### Persistent
- **Purpose:** panels that stay open alongside the page content
- **Visual distinction:** no backdrop. Page content reflows to accommodate the panel

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Closed | Not rendered or translated off-screen | |
| Open | Panel visible, backdrop shown (if applicable) | |
| Loading | Skeleton content within the panel | → UX Standards §3.2 |

---

## Interaction spec

### Mouse / pointer
- Click backdrop to close (if non-destructive content)
- Click close button to close
- Drag resize handle to adjust width (if resizable)

### Keyboard
- `Escape` closes the panel
- Focus is trapped within the panel while open (same as Modal)
- On open, focus moves to the first interactive element or the panel title
- On close, focus returns to the trigger

### Touch
- Swipe to dismiss (swipe right for right-anchored, down for bottom-anchored). Optional enhancement
- Bottom sheet: drag handle to expand/collapse
- All interactive elements meet 44×44px targets → UX Standards §13.6

### Scroll behaviour
- Panel body scrolls independently. The page behind should not scroll while the panel is open (unless the panel is persistent)

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Panel takes full width (or nearly full). Or switch to a bottom sheet pattern |
| `md` | Panel width: 400–500px |
| `lg`+ | Panel width: 480–640px. May support resizing. Page content may reflow beside it |

---

## Accessibility

- **Role / semantics:** `role="dialog"` with `aria-modal="true"` (if backdrop blocks page). `aria-labelledby` pointing to panel title
- **Focus trap:** mandatory when backdrop is present. For persistent panels, focus should be manageable but not trapped
- **Close:** Escape key and close button
- **Focus return:** on close, return focus to the element that opened the panel
- **WCAG:** 2.4.3, 2.1.2, 4.1.2

---

## Motion & animation

- **Open:** panel slides in from the edge, 250ms ease-out. Backdrop fades in simultaneously
- **Close:** reverse, 200ms ease-in
- **Bottom sheet (mobile):** slides up, 250ms ease-out
- **Reduced motion:** instant show/hide, no slide → UX Standards §13.9

---

## Common pitfalls

1. **No focus management** — same rules as Modal. Trap focus, return it on close
2. **Page scrolls behind the panel** — lock body scroll when the panel has a backdrop. Use `overflow: hidden` on the body
3. **Panel too narrow for its content** — forms in narrow panels are frustrating. Ensure adequate width, and test with real content
4. **No loading state** — if panel content loads asynchronously, show a skeleton, not a blank panel → UX Standards §3.2
5. **Stacking panels** — opening a second panel over the first is confusing. Replace the content or close the first panel before opening the second
6. **Missing close affordance on mobile** — on small screens, ensure the close button is prominent and consider swipe-to-dismiss

---

## Tailwind notes

Backdrop:
- `fixed inset-0 z-40 bg-black/50`

Panel (right-anchored):
- `fixed top-0 right-0 bottom-0 z-50 w-full sm:max-w-md lg:max-w-lg bg-white shadow-xl flex flex-col`
- Closed: `translate-x-full`
- Open: `translate-x-0`
- Transition: `transition-transform duration-250 ease-out`

Panel (bottom-anchored):
- `fixed bottom-0 left-0 right-0 z-50 rounded-t-2xl bg-white shadow-xl max-h-[85vh]`

Header:
- `flex items-center justify-between px-6 py-4 border-b flex-shrink-0`

Body:
- `flex-1 overflow-y-auto px-6 py-4`

Footer:
- `flex items-center justify-end gap-3 px-6 py-4 border-t flex-shrink-0`
