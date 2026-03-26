# Tooltip

> A contextual hint that appears on hover or focus, providing brief supplementary information about an element.

---

## When to use
- Explaining icon-only buttons → UX Standards §8.4
- Providing additional context for truncated text
- Clarifying a label or control without cluttering the interface
- Showing keyboard shortcuts or secondary information

## When not to use
- Critical information the user must see — tooltips are easy to miss. Use inline text or an Alert
- Long content or interactive elements — use a Popover instead
- Information needed on touch-only devices where hover isn't available — provide the info another way
- As a substitute for a proper label → UX Standards §7.1

---

## Anatomy

1. **Trigger** — the element the tooltip is attached to (button, icon, text). Required
2. **Tooltip container** — small floating box with the text. Required
3. **Arrow / caret** — triangular pointer connecting the tooltip to its trigger. Optional but recommended
4. **Text** — brief description, one sentence maximum. Required → UX Standards §8.4

---

## Variants

### Standard
- **Purpose:** short text hint
- **Visual distinction:** dark background, white text, small size

### Rich (borderline popover)
- **Purpose:** slightly more content — a label plus a keyboard shortcut or brief description
- **Visual distinction:** may include secondary text or a shortcut badge. Still non-interactive

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Hidden | Not rendered or visibility hidden | Default state |
| Visible | Positioned near the trigger, fully opaque | Shown on hover/focus |

---

## Interaction spec

### Mouse / pointer
- Appears after a short delay on hover (200–300ms) → UX Standards §8.4
- Disappears when the pointer leaves the trigger. Brief exit delay (100ms) prevents flicker when moving between trigger and tooltip

### Keyboard
- Appears when the trigger receives focus
- Disappears when focus moves away
- `Escape` dismisses the tooltip while keeping focus on the trigger

### Touch
- Tooltips are not reliably accessible on touch devices. Do not rely on them as the only source of information
- Long-press to show tooltip is an optional enhancement but not a substitute for proper labelling

### Positioning
- Default position: above the trigger
- Auto-flip to below, left, or right if the default position would be clipped by the viewport
- Keep the tooltip within the viewport at all times

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| All | Same behaviour. Positioning flips to stay in viewport |

On touch devices, ensure the information conveyed by the tooltip is also available through another means (visible label, `aria-label`, or helper text).

---

## Accessibility

- **Association:** use `aria-describedby` on the trigger, pointing to the tooltip's id. This makes the tooltip content available to screen readers
- **Dismissible:** `Escape` must hide the tooltip → WCAG 1.4.13
- **Persistent on hover:** the tooltip must remain visible while the pointer is over the tooltip itself (not just the trigger) → WCAG 1.4.13
- **Not interactive:** tooltips must not contain links, buttons, or form elements. If interaction is needed, use a Popover
- **WCAG:** 1.4.13 (Content on Hover or Focus), 4.1.2

---

## Motion & animation

- **Show:** fade in + translate 4px from the trigger side, 150ms ease-out, after 200ms hover delay
- **Hide:** fade out, 100ms ease-in
- **Reduced motion:** instant show/hide, no translate → UX Standards §14

---

## Common pitfalls

1. **Tooltip as the only label** — tooltips supplement labels, they don't replace them. Icon buttons need `aria-label` in addition to a tooltip
2. **Interactive content in a tooltip** — links or buttons inside a tooltip are inaccessible. Use a Popover for interactive content
3. **No hover delay** — tooltips that appear instantly on hover create visual noise. Add a 200–300ms delay
4. **Not dismissible via Escape** — WCAG requires it
5. **Tooltip clipped by containers** — use a portal or fixed positioning to prevent parent `overflow: hidden` from clipping
6. **Relying on tooltips for touch users** — on mobile, hover doesn't exist. The information must be available another way

---

## Tailwind notes

Container:
- `absolute z-50 rounded-md bg-gray-900 px-2.5 py-1.5 text-xs text-white shadow-lg`
- Max width: `max-w-xs`

Arrow:
- CSS triangle or a rotated `<div>`: `h-2 w-2 rotate-45 bg-gray-900`

Positioning:
- Use `absolute` + JS-calculated positioning. Or use a tooltip library that handles positioning and flipping.
- Top: `bottom-full left-1/2 -translate-x-1/2 mb-2`
- Bottom: `top-full left-1/2 -translate-x-1/2 mt-2`

Transition:
- `transition-opacity duration-150` with conditional `opacity-0` / `opacity-100`
