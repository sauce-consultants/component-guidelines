# Slider

> A control for selecting a numeric value (or range) along a continuous or stepped track. Best for values where approximate selection is acceptable or where the user benefits from a spatial sense of the value's position within a range.

---

## When to use
- Setting a value within a bounded range (volume, brightness, price range, quantity)
- When the user benefits from seeing where their value sits relative to min/max
- Filtering by range (e.g. price between £50 and £200)

## When not to use
- Precise numeric entry — use Input with `inputmode="numeric"`
- Unbounded values — sliders need a defined min and max
- Very wide ranges where fine control matters (e.g. 0–100,000) — the slider becomes impractical for precision

---

## Anatomy

1. **Track** — the horizontal bar representing the full range. Required
2. **Filled track** — the portion of the track between min and the current value (or between two values for range). Required
3. **Thumb** — the draggable handle. Required (one for single, two for range)
4. **Label** — describes what the slider controls. Required → UX Standards §7.1
5. **Value display** — shows the current numeric value. Recommended. May be a tooltip above the thumb or a text readout beside the slider
6. **Min/Max labels** — text or numbers at each end of the track. Optional
7. **Step markers** — tick marks for stepped sliders. Optional

---

## Variants

### Single value
- **Purpose:** select one value within a range
- **Visual distinction:** one thumb, filled track from min to thumb

### Range
- **Purpose:** select a lower and upper bound
- **Visual distinction:** two thumbs, filled track between them

### Stepped
- **Purpose:** discrete values only (e.g. quantities of 1, 5, 10, 25)
- **Visual distinction:** thumb snaps to defined positions. Tick marks may indicate valid stops

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Track and thumb at current value | |
| Hover | Thumb enlarges slightly or shows shadow | |
| Focus | Focus ring on thumb | → UX Standards §13.5 |
| Active / Dragging | Thumb may elevate (shadow) or enlarge. Value tooltip becomes visible | |
| Disabled | Muted track and thumb, not interactive | |

---

## Interaction spec

### Mouse / pointer
- Click anywhere on the track to jump the thumb to that position
- Click and drag the thumb to adjust
- For range sliders, clicking the track moves the nearest thumb

### Keyboard
- `ArrowRight` / `ArrowUp` increases value by one step
- `ArrowLeft` / `ArrowDown` decreases by one step
- `Home` sets to minimum, `End` sets to maximum
- `PageUp` / `PageDown` for larger increments (10× step)

### Touch
- Drag the thumb. Thumb must have a 44×44px minimum hit area → UX Standards §13.6
- Consider expanding the thumb's touch area beyond its visual size
- Tap on track to jump to position

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Full-width track. Value display may move from inline to above the slider. Ensure thumb is easy to grab |
| `md`+ | Standard inline layout. May sit alongside a numeric input for precise entry |

Pair a slider with a numeric input for best of both worlds: spatial feedback from the slider, precision from the input. Keep them synchronised.

---

## Accessibility

- **Role / semantics:** `<input type="range">` or custom with `role="slider"`
- **ARIA:** `aria-valuemin`, `aria-valuemax`, `aria-valuenow`, `aria-valuetext` (for human-readable value, e.g. "£50")
- **Accessible name:** `<label>` linked to the slider
- **Range slider:** each thumb needs its own accessible name (e.g. "Minimum price", "Maximum price")
- **Keyboard:** full arrow key support as specified above
- **WCAG:** 4.1.2, 1.3.1, 2.1.1

---

## Motion & animation

- **Thumb drag:** immediate, no easing on the drag itself — must track the pointer 1:1
- **Snap to step:** brief 100ms ease-out when releasing on a stepped slider
- **Value tooltip:** fade in on drag start, fade out on release, 100ms
- **Reduced motion:** remove tooltip fade; snap can remain as it is functional

---

## Common pitfalls

1. **No visible current value** — sliders are imprecise by nature. Always show the current numeric value somewhere (tooltip, inline readout, or paired input)
2. **Thumb too small on touch** — the visual thumb can be small, but the touch target must be large. Add invisible padding
3. **Range thumbs that can't cross** — in range sliders, decide whether the lower thumb can exceed the upper. Usually it shouldn't — clamp values to prevent crossing
4. **No keyboard support on custom sliders** — if building a custom slider, implement the full arrow key, Home/End, PageUp/PageDown pattern
5. **Missing `aria-valuetext`** — a slider at position 50 should announce "£50" not just "50". Provide meaningful value text

---

## Tailwind notes

Track:
- `relative w-full h-2 rounded-full bg-gray-200`
- Filled: `absolute h-full rounded-full bg-[var(--color-action-primary)]`

Thumb:
- `absolute h-5 w-5 rounded-full bg-white border-2 border-[var(--color-action-primary)] shadow cursor-pointer`
- Position via `left` percentage calculated from value
- Expand touch target: wrap in a transparent `h-11 w-11` container centred on the thumb

For native `<input type="range">`, use `appearance-none` and style via `::-webkit-slider-thumb` and `::-moz-range-thumb` pseudo-elements. This approach is simpler but less customisable for range sliders.
