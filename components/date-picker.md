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

---

## Header date variant

> Added from the yard-demo bookings list (issue #82).

- **Purpose:** the page's current day, shown as the subtitle under the page title, with previous and next day arrows either side of the title block. Used where the day is the main way of navigating a list (Bookings, Pending bookings)
- **Trigger:** a button styled as header text, not a form field. Its accessible name is the date ("Thursday 23 October 2026"), and it has a calendar icon after the text. Use a button, not a text input, because there is nothing to type
- **Panel:** `role="dialog"` named "Choose date" holding a `role="grid"` month. Choosing a day closes the panel and returns focus to the trigger. Escape closes without changing the date
- **Arrows:** icon-only buttons named for what they move ("Previous day", "Next day"), never just "Previous"
- **Do not** add the picker to lists that have no date (Completed, No show): show the title only

---

## Field variant

> Added from the yard-demo Create booking flow (issue #99). `variant="field"` on the same DatePicker.

- **Purpose:** a date inside a form (Create booking, Schedule: date of arrival). The calendar still helps because the day of the week matters, and the date can also be typed
- **Trigger:** an Input-styled text field (same height, radius, ring and focus ring as Input) with a trailing calendar icon button. The text input is the field; the icon button opens the calendar. Placeholder `DD/MM/YYYY`
- **Label:** a visible `<label htmlFor>` on the text input, with the hint under the field linked by `aria-describedby`. The calendar button is named "Choose date, currently" plus the full date, and has a 44px tap area
- **Typing:** DD/MM/YYYY (D/M/YYYY, and `.` or `-` between the parts also read). Parsed on blur and on Enter. The date must be a real calendar day (31/02/2026 is rejected, not rolled over)
- **Error:** an inline message under the field in orange (`aria-invalid` on the input, message first in `aria-describedby`, announced through a status region): "Enter a real date as DD/MM/YYYY, for example 29/10/2026", or "Enter a date as DD/MM/YYYY" when empty. The last good date stays selected and the typed text is left for the user to correct
- **`onInvalid(message | null)`:** called when the typed text is checked (blur or Enter) and is empty, not a date or not a real day, with the same message as the inline error. Called with `null` once the text is valid or a day is chosen in the calendar. The `value` still holds the last good date, so a form must use this to block submit (Next stays unavailable with the reason on screen) and to list the message in its error summary, rather than silently continuing with the old date. An empty field counts as invalid because the date is required
- **`maxDate` (field only):** the last day that can be chosen, compared by day. Days after it are disabled in the calendar: `aria-disabled="true"` (not `disabled`, so the arrow keys can still land and keep focus), struck through as well as muted (not colour alone), choosing one does nothing, the arrow keys stop at the last day, and "Next month" is `aria-disabled` once the next month starts after it. A typed date after it is invalid: inline error "Choose a date on or before DD/MM/YYYY" and `onInvalid` is called with the same message, like any other invalid text. Use it where a rule caps the date (Create booking, Schedule: the earliest purchase order due date); the form explains why in its own hint or note. Omit it for no limit. Added in issue #99 round 3
- **`maxDateMessage` (field only):** the message for a date after `maxDate`, in place of the generic "Choose a date on or before DD/MM/YYYY". It belongs to `maxDate`: it is used only when `maxDate` is set (ignored without it), for every date after that day, and it is the inline error, the `onInvalid` message and so the text in the form's error summary. Build both from the same source (one helper, one due date) so the limit and its words cannot disagree, and have the form's own validation of the same rule say the same words. Say why the limit exists, e.g. "Choose a date on or before 02/11/2026, the purchase order due date". Omit it for the generic wording. Added in issue #99 round 4, tied to `maxDate` in round 6
- **`minDate` (field only):** the first day that can be chosen, compared by day. The mirror of `maxDate`: days before it are disabled in the calendar (`aria-disabled="true"`, struck through as well as muted), choosing one does nothing, the arrow keys stop at the first day, and "Previous month" is `aria-disabled` once the previous month ends before it. A typed date before it is invalid: inline error "Choose a date on or after DD/MM/YYYY" and `onInvalid` is called with the same message. Use it where a rule sets an earliest date (Create booking, Schedule: the earliest day with a bookable slot, after the client's booking cutoff, a per-client setting of hours before arrival). Omit it for no limit. Added in issue #99 round 8
- **`minDateMessage` (field only):** the message for a date before `minDate`, in place of the generic "Choose a date on or after DD/MM/YYYY". It belongs to `minDate`: used only when `minDate` is set, for every date before that day, and it is the inline error, the `onInvalid` message and the text in the form's error summary. Build both from the same source and have the form's own validation say the same words. Say why, e.g. "Bookings close 24 hours before arrival. Choose 03/11/2026 or later". Added in issue #99 round 8
- **Panel and keyboard:** the same calendar panel and keys as the header variant. The panel anchors to the end of the field. Choosing a day closes it, replaces the typed text and clears any error
- **Header variant:** unchanged
