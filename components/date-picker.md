# Date Picker

> A calendar-based control for selecting a date or date range. Provides visual context for day-of-week, month navigation, and relative date positioning.

---

## When to use
- Selecting a specific date where the day of the week matters (bookings, deadlines, scheduling)
- Date range selection (check-in/check-out, reporting periods)
- When the user needs calendar context to make their choice

## When not to use
- Known, easily typed dates (date of birth) — a formatted text Input is often faster
- Far-future or far-past dates (year of birth) — a calendar requires too much navigation. Use separate month/year selects or a text input
- Time selection only — use a dedicated time picker

---

## Anatomy

1. **Label** — describes what date is being selected. Required → UX Standards §7.1
2. **Trigger input** — text field showing the selected date, with a calendar icon. Required
3. **Calendar panel** — the popover containing the calendar grid. Required
4. **Month/year header** — navigation controls to move between months/years. Required
5. **Day grid** — the 7-column grid of days. Required
6. **Day cell** — individual date. Required
7. **Today indicator** — visual highlight on the current date. Recommended
8. **Selected date indicator** — highlight on the chosen date(s). Required
9. **Range highlight** — background fill between two selected dates in range mode. Conditional
10. **Helper / validation text** — below the trigger input. Optional / conditional

---

## Variants

### Single date
- **Purpose:** select one date
- **Visual distinction:** single selected highlight

### Date range
- **Purpose:** select a start and end date
- **Visual distinction:** two highlighted dates with a shaded range between them. May use two trigger inputs

### With time
- **Purpose:** select date and time together
- **Visual distinction:** time selection controls below or beside the calendar grid

### Inline
- **Purpose:** calendar always visible, not in a popover (e.g. sidebar calendar)
- **Visual distinction:** no trigger input; calendar renders directly in the page

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Empty | Trigger shows placeholder (e.g. "DD/MM/YYYY") | |
| Open | Calendar panel visible | |
| Day hover | Background highlight on hovered day | In range mode, preview the range on hover |
| Day selected | Strong background fill (brand colour) | |
| Today | Subtle ring or dot distinguishing today from other days | |
| Disabled dates | Muted text, not selectable | Use for dates outside valid range |
| Error | Error border on trigger + validation message | |

---

## Interaction spec

### Mouse / pointer
- Click trigger or calendar icon to open panel
- Click a day to select it (and close panel for single date)
- Click prev/next arrows to navigate months
- Click month/year header to access month or year selection view
- Click outside to close

### Keyboard
- `Enter` or `Space` or `ArrowDown` on trigger opens the panel
- Arrow keys navigate the day grid (left/right by day, up/down by week)
- `Home` / `End` jump to start/end of the week
- `PageUp` / `PageDown` navigate by month
- `Enter` selects the focused day
- `Escape` closes without selecting

### Touch
- Tap to open. Day cells must be at least 44×44px → UX Standards §13.6
- Swipe left/right on the calendar to navigate months (optional enhancement)

### Manual input
- Allow the user to type a date directly into the trigger input in the expected format
- Parse the typed value and validate on blur → UX Standards §7.2
- Show the expected format as placeholder or helper text

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Calendar panel full-width or uses a bottom sheet. Day cells must be comfortably tappable |
| `md`+ | Calendar popover anchored to the trigger. Standard sizing |

For date range on mobile, consider a sequential flow: select start date, then show calendar for end date, rather than requiring the user to interact with a small dual-calendar.

---

## Accessibility

- **Role / semantics:** trigger uses `role="combobox"` or a button that opens the dialog. Calendar panel uses `role="dialog"` with `role="grid"` for the day grid
- **Accessible name:** `<label>` linked to the trigger input
- **Grid navigation:** `aria-selected` on selected day, `aria-current="date"` on today
- **Disabled dates:** `aria-disabled="true"` on dates outside the valid range
- **Keyboard:** full arrow key grid navigation. Focus must be trapped within the calendar dialog while open
- **Live region:** announce month changes when navigating
- **WCAG:** 1.3.1, 4.1.2, 2.1.1, 2.4.3 (Focus Order)

---

## Motion & animation

- **Panel open:** fade + scale from trigger, 150ms ease-out
- **Month navigation:** subtle slide or crossfade between month views, 200ms ease-in-out
- **Reduced motion:** instant show/hide, no slide between months → UX Standards §14

---

## Common pitfalls

1. **No manual input fallback** — some users prefer to type dates. A calendar-only picker slows them down. Always allow direct text entry in the trigger field
2. **Day cells too small on mobile** — a 7-column grid on a 375px screen leaves ~50px per cell. Ensure tappability
3. **No today indicator** — users lose context without knowing where today is in the grid
4. **Year navigation buried** — if users need to select dates years away, clicking month-by-month is painful. Provide a year/month jump control
5. **Locale-unaware formatting** — date formats vary by locale (DD/MM/YYYY vs MM/DD/YYYY). Use the project's locale settings for display and parsing
6. **Calendar doesn't handle disabled ranges** — if only future dates are valid, past dates must be clearly disabled, not just un-highlighted

---

## Tailwind notes

Trigger:
- Same pattern as Input: `rounded-md border px-3 py-2` with a trailing calendar icon

Calendar panel:
- `absolute z-50 mt-1 rounded-lg border bg-white shadow-lg p-4`
- Header: `flex items-center justify-between mb-4`
- Day grid: `grid grid-cols-7 gap-1`
- Day cell: `h-10 w-10 flex items-center justify-center rounded-full text-sm cursor-pointer hover:bg-gray-100`
- Selected: `bg-[var(--color-action-primary)] text-white`
- Today: `ring-1 ring-[var(--color-action-primary)]`
- Disabled: `text-gray-300 cursor-not-allowed`
- Range fill: `bg-[var(--color-action-primary)]/10` on days between start and end
