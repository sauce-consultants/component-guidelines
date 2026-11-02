# Slot Grid

> A single-choice picker for one fixed time slot from a day, laid out in period columns, with optional site traffic.

---

## When to use
- Choosing one arrival or departure slot from a day with fixed one-hour (or similar) slots, such as a gate or dock booking
- When the shape of the day matters: which periods are open, which slots are full, which are quiet

## When not to use
- Choosing a date: use Date Picker
- A short list of options with no time structure: use Radio Group
- A long list or free time entry: use Select, or a time input
- Capturing a time range: this picks one slot, not a start and an end

---

## Anatomy

1. **Group heading** — names the group (for example "Arrival time"). Required, outside the component, and the group's accessible name
2. **Period columns** — Overnight (00:00 to 05:59), Morning, Afternoon, Evening, each with a visible period label. Required
3. **Slot** — the time range, for example "08:00-09:00". Required
4. **Status line** — on an unavailable slot, the reason ("Fully booked", "Site closed", "Time has passed"). On an available slot, the site traffic label with a bar glyph, when site traffic is supplied. Optional
5. **Selected indicator** — yellow fill and a check icon
6. **Legend** — explains the bar glyphs. Shown only when at least one slot has site traffic

---

## Variants

### With site traffic
- **Purpose:** the site can say how many bookings each slot already holds against its cap
- **Visual distinction:** available slots show Light, Medium or Heavy with one, two or three bars

### Availability only (default data)
- **Purpose:** only open or closed is known
- **Visual distinction:** no status line on available slots, no legend. This is the same component with `traffic` left off every slot. Never invent site traffic

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Available | gray-800 fill, gray-500 outline, white text | The outline is at least 3:1 against the card |
| Hover | gray-700 fill | |
| Focus | 2px yellow-400 ring with a 2px offset | Visible on keyboard focus only |
| Selected | yellow-500 fill, black bold text, check icon | Not colour alone: the check icon and bold text carry it |
| Unavailable | dashed gray-500 border, gray-400 text, reason shown | `aria-disabled`, skipped by the keyboard, `not-allowed` cursor |

→ UX Standards §13 for state and contrast rules

---

## Interaction spec

### Mouse / pointer
- Click an available slot to select it. Clicking an unavailable slot does nothing
- Selection is single-choice. Changing the date clears the selection, because slots belong to a day

### Keyboard
- The grid is one tab stop: the selected slot, otherwise the first available slot
- Arrow keys move to the next or previous available slot and select it, wrapping at the ends. Unavailable slots are skipped
- Home and End move to the first and last available slot
- Space or Enter selects the focused slot

### Touch
- Every slot is at least 56px high and fills its column width

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs` (375px) | Two columns: Overnight and Morning, then Afternoon and Evening |
| `sm` (640px) | Two columns |
| `md` (768px) | Four columns, one per period |
| `lg` (1024px+) | Four columns |

→ UX Standards §16 for breakpoint scale and responsive rules

---

## Accessibility

- **Role / semantics:** `role="radiogroup"` containing buttons with `role="radio"` and `aria-checked`
- **Accessible name:** the group is named by its heading (`aria-labelledby`). Each slot is named by its time range
- **Description:** the status line (reason or site traffic) is the slot's `aria-describedby`. A group-level hint or error goes on the group's `aria-describedby`
- **Keyboard nav:** roving tabindex as above. Unavailable slots use `aria-disabled` with visible reason text, not `disabled`, so the reason is never hidden
- **Screen reader:** selecting a slot announces it as checked. An error message under the grid sits in a `role="status"` region
- **Non-colour cues:** unavailable = dashed border and reason text. Site traffic = text label and bar glyph. Selected = check icon
- **Contrast:** 4.5:1 for text and 3:1 for the slot outline and focus ring, in every state that is not disabled. Disabled slots are exempt but still meet 4.5:1 for their reason text
- **WCAG criteria:** 1.4.1 Use of Colour, 1.4.3 Contrast (Minimum), 1.4.11 Non-text Contrast, 2.1.1 Keyboard, 4.1.2 Name, Role, Value

→ UX Standards §13 for baseline accessibility requirements

---

## Motion & animation

- Colour transition on hover and selection only (the standard `transition-colors`)
- No movement. Nothing to reduce for `prefers-reduced-motion`

→ UX Standards §14 for duration, easing, and reduced motion rules

---

## Common pitfalls

1. **Showing availability by colour alone** — an unavailable slot needs a reason in text and a different outline, not only a dimmer fill
2. **Using `disabled` on unavailable slots** — it removes the slot from the tab order and the reason cannot be reached. Use `aria-disabled` with visible text
3. **Inventing site traffic** — show site traffic only when the source supplies a booked count and a cap per slot. Otherwise leave it off and show availability only
4. **Leaving a stale selection after the date changes** — clear it and revalidate
5. **No group name** — the grid is a radiogroup and needs a label
6. **Four narrow columns on a phone** — two across at 375px keeps each slot readable

---

## Tailwind notes

- Grid: `grid grid-cols-2 gap-x-3 gap-y-5 md:grid-cols-4`, each period a `flex flex-col gap-2` column
- Slot: `min-h-14 rounded-xl px-3 py-2`. Available `bg-gray-800 ring-1 ring-inset ring-gray-500`. Unavailable `border border-dashed border-gray-500 bg-gray-900 text-gray-400`. Selected `bg-yellow-500 text-black`
- Focus: `focus-visible:ring-2 focus-visible:ring-yellow-400 focus-visible:ring-offset-2`

→ UX Standards §10.2 for semantic colour tokens

---

## Implementation reference

yard-demo `src/components/ui/SlotGrid.tsx`, with a docs page at `/docs/slot-grid`.
