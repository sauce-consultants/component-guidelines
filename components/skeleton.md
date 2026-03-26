# Skeleton Loader

> Placeholder shapes rendered in the approximate position and size of real content while data is loading. Reduces perceived load time and prevents layout shift.

→ UX Standards §3.2 for skeleton screen requirements.

---

## When to use
- Any screen where the layout is known but data hasn't arrived yet
- Cards, tables, lists, profiles, dashboards — any structured content
- When you want to avoid a blank screen or generic spinner → UX Standards §3.1

## When not to use
- Small, isolated loading regions (a single button action) — use a Spinner or button loading state
- Content with unpredictable layout — a skeleton that doesn't match the real content is worse than a spinner
- Operations that complete in under 200ms — show nothing

---

## Anatomy

1. **Skeleton block** — rectangular placeholder for text lines, images, or content areas. Required
2. **Skeleton circle** — circular placeholder for avatars. Conditional
3. **Shimmer animation** — a subtle left-to-right shine effect indicating loading. Recommended

---

## Variants

### Text skeleton
- **Purpose:** placeholder for text paragraphs or labels
- **Visual distinction:** horizontal bars of varying widths mimicking line lengths

### Card skeleton
- **Purpose:** placeholder for a full card component
- **Visual distinction:** rectangle for image area + text bars for title/description

### Table skeleton
- **Purpose:** placeholder for table rows
- **Visual distinction:** row-shaped bars matching the table's column layout

### Custom / composite
- **Purpose:** any layout-specific skeleton built from primitive shapes
- **Visual distinction:** matches the real component's layout as closely as possible

---

## States

Skeletons have one state: loading. They are replaced by real content when data arrives.

---

## Interaction spec

Skeletons are non-interactive. They should not be focusable or announce themselves individually to screen readers.

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| All | Skeletons must match the responsive layout of the real content. A 3-column card grid skeleton at `lg` should show 3 skeleton cards. At `xs`, it should show 1 |

---

## Accessibility

- **Role / semantics:** use `aria-hidden="true"` on skeleton elements since they are purely visual placeholders
- **Loading announcement:** the container holding the skeleton should have `aria-busy="true"` while loading and `aria-live="polite"` to announce when content has loaded
- **WCAG:** 4.1.3 (Status Messages)

---

## Motion & animation

- **Shimmer:** a gradient highlight sweeps left to right across the skeleton, 1.5–2s duration, infinite loop. This communicates activity
- **Content replacement:** skeletons fade out as real content fades in, 150ms. Or real content renders immediately, replacing the skeleton with no transition
- **Reduced motion:** remove shimmer animation. Static grey blocks are acceptable → UX Standards §13.9

---

## Common pitfalls

1. **Skeleton doesn't match the real layout** — the whole point is to pre-render the expected shape. If the skeleton is a generic rectangle and the real content is a card with image + text + badge, the transition will cause layout shift → UX Standards §17.4
2. **Using brand or accent colours for skeletons** — skeleton placeholders must use neutral, low-contrast grey tones → UX Standards §3.2
3. **Skeleton for content that loads instantly** — if data arrives in under 200ms, the skeleton flashes and disappears. Only show skeletons for loads expected to take longer
4. **No skeleton at all** — a blank screen while loading makes users assume something is broken → UX Standards §3.1
5. **Skeleton persists after data loads** — ensure skeletons are replaced as soon as real data is available. Don't add artificial minimum display times → UX Standards §3.4

---

## Tailwind notes

Block:
- `rounded-md bg-gray-200 animate-pulse`
- Text line: `h-4 rounded-md bg-gray-200` with varying widths (`w-3/4`, `w-1/2`, `w-full`)

Circle (avatar):
- `h-10 w-10 rounded-full bg-gray-200 animate-pulse`

Shimmer alternative to `animate-pulse`:
```css
@keyframes shimmer {
  0% { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}
.skeleton-shimmer {
  background: linear-gradient(90deg, theme('colors.gray.200') 25%, theme('colors.gray.100') 50%, theme('colors.gray.200') 75%);
  background-size: 200% 100%;
  animation: shimmer 1.5s infinite;
}
```

Compose skeletons that mirror the target component's layout. Use the same spacing, sizing, and grid structure as the real component.
