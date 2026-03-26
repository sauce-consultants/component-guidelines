# Icon Button

> A compact button that uses an icon as its only visible content. Used for actions where space is limited and the icon is universally understood.

---

## When to use
- Toolbars where multiple actions compete for space
- Table row actions (edit, delete, copy)
- Close buttons on modals, panels, and toasts
- Actions where the icon is universally recognised (close ✕, menu ☰, search 🔍)

## When not to use
- When the action is ambiguous without a text label — use a standard Button with icon + label instead
- As the primary action on a screen — primary actions need clear text labels
- For actions a first-time user needs to find without guessing — add a label

---

## Anatomy

1. **Container** — the clickable hit area. Required. Must be at least 44×44px for touch
2. **Icon** — a single icon centred within the container. Required
3. **Tooltip** — text hint on hover/focus describing the action. Required → UX Standards §8.4, §12.4

---

## Variants

### Default
- **Purpose:** standard actions in toolbars and compact layouts
- **Visual distinction:** transparent or subtle background, icon in muted colour, background on hover

### Destructive
- **Purpose:** delete, remove, close actions
- **Visual distinction:** icon or hover state uses danger colour

### Ghost
- **Purpose:** very low emphasis, blends with content
- **Visual distinction:** no visible container until hover/focus

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Icon visible, no or subtle background | |
| Hover | Background fill appears, icon colour may intensify | Tooltip appears |
| Focus | Visible focus ring | Must not be hidden → UX Standards §13.5 |
| Active / Pressed | Darker background or slight scale | |
| Disabled | Reduced opacity, no tooltip, no pointer events | |
| Loading | Spinner replaces icon, same dimensions | Rare but handle it |

---

## Interaction spec

### Mouse / pointer
- Single click triggers action
- Hover reveals tooltip after a short delay (200–300ms)

### Keyboard
- `Enter` or `Space` triggers the action
- Focusable via `Tab`
- Tooltip should appear on focus as well as hover

### Touch
- Minimum tap target: 44×44px even if the visible icon is smaller → UX Standards §13.6
- Long-press may show tooltip on mobile (optional enhancement)

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Icon buttons are often preferred at small sizes where text buttons don't fit. Ensure tap targets are met |
| `md`+ | Consider switching to labelled buttons where space allows, especially for less obvious actions |

In toolbars, icon buttons can remain icon-only across all breakpoints as long as tooltips are present.

---

## Accessibility

- **Role / semantics:** `<button>` element
- **Accessible name:** `aria-label` is mandatory since there is no visible text label → UX Standards §12.4
- **Tooltip:** must appear on both hover and focus. Tooltip text should match or closely align with `aria-label`
- **Keyboard nav:** standard button keyboard behaviour
- **WCAG:** 2.1 — 4.1.2 (Name, Role, Value), 1.1.1 (Non-text Content)

---

## Motion & animation

- **Hover:** background fade in, 150ms ease-out
- **Tooltip:** fade + slight translate-y, 150ms ease-out, 200ms delay before appearing
- **Reduced motion:** remove translate; fade is acceptable → UX Standards §14

---

## Common pitfalls

1. **Missing `aria-label`** — an icon button without an accessible name is invisible to screen readers. Every icon button must have one
2. **No tooltip** — sighted users also need to know what the button does. Tooltips are not optional for icon buttons
3. **Tap target too small** — a 24px icon needs padding to reach the 44px minimum. Use padding on the button, not on the icon
4. **Using an `<img>` or `<svg>` as the clickable element** — wrap the icon in a `<button>`. The icon is content, the button is the control
5. **Inconsistent icon set** — icon buttons across a screen must use icons from the same set with matching weight and style → UX Standards §12.1

---

## Tailwind notes

Structural pattern:
- Base: `inline-flex items-center justify-center rounded-md transition-colors`
- Size: `h-10 w-10` (or `h-9 w-9` for compact). Padding is built into the fixed dimensions
- Focus: `focus-visible:outline-2 focus-visible:outline-offset-2`
- Hover: `hover:bg-gray-100 dark:hover:bg-gray-800` (adjust for theme)
- Icon size within: `h-5 w-5` centred via flex

Ensure the icon uses `currentColor` so it inherits text colour from the button's state → UX Standards §12.3.
