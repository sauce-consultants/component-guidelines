# Combobox

> A searchable select control that combines text input with a filterable dropdown list. Used when the option set is large or the user may benefit from typing to narrow results.

---

## When to use
- Selecting from a long list (15+ options) where search improves speed
- When options need to be loaded asynchronously (e.g. searching users, products, addresses)
- Tagging or multi-select scenarios where the user types to find and add values

## When not to use
- Short option lists (under 10) — use Select or Radio Group
- Free-form text with no predefined options — use Input
- Date selection — use Date Picker

---

## Anatomy

1. **Label** — visible text above the control. Required → UX Standards §7.1
2. **Input field** — text input the user types into to filter options. Required
3. **Clear button** — clears the current input/selection. Optional
4. **Dropdown panel** — filtered list of matching options. Required
5. **Option** — individual selectable item. Required
6. **No results message** — shown when the filter matches nothing. Required when filtering is possible
7. **Loading indicator** — shown while async options are being fetched. Conditional
8. **Selected value chips** — for multi-select variants, tags representing each selected value. Conditional
9. **Helper / validation text** — below the input. Optional / conditional

---

## Variants

### Single select
- **Purpose:** user picks exactly one value from a searchable list
- **Visual distinction:** selected value displayed in the input field after selection

### Multi-select
- **Purpose:** user picks one or more values, displayed as removable chips/tags
- **Visual distinction:** chips stack inside or below the input area. Input remains active for additional searches

### Async / remote
- **Purpose:** options are fetched from an API based on user input
- **Visual distinction:** loading spinner in the dropdown while results are being fetched

### Creatable
- **Purpose:** user can type a new value that isn't in the list and add it
- **Visual distinction:** a "Create [value]" option appears at the end of the filtered list

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Input with placeholder, dropdown closed | |
| Focused / typing | Dropdown open, options filtering in real time | |
| Loading | Spinner in dropdown, options area shows loading state | → UX Standards §3.1 |
| No results | Dropdown shows "No results" message | Distinguish from empty dataset → UX Standards §6.3 |
| Selected (single) | Value in input, dropdown closed | |
| Selected (multi) | Chips visible, input ready for more | |
| Disabled | Muted, not interactive | |
| Error | Error border + validation message | |

---

## Interaction spec

### Mouse / pointer
- Click input to focus and open dropdown
- Click option to select. In multi-select, dropdown stays open
- Click chip remove icon to deselect (multi-select)
- Click outside to close

### Keyboard
- `ArrowDown` opens dropdown from focused input
- `ArrowUp` / `ArrowDown` navigates options
- `Enter` selects highlighted option
- `Escape` closes dropdown
- `Backspace` in empty input removes the last chip (multi-select)
- Typing filters the list in real time

### Touch
- Tap to focus and open. Dropdown should not obstruct the input
- Chip remove buttons must meet 44×44px tap target → UX Standards §13.6

### Debouncing
- For async variants, debounce input by 200–300ms to avoid excessive API calls
- Show a brief loading state before results appear → UX Standards §3.1

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Full-width. Multi-select chips may need to wrap. Consider a bottom sheet for the dropdown on mobile |
| `md`+ | Standard dropdown. Width matches or exceeds input width |

For multi-select, if chip count is high, allow chip area to wrap with a max visible height and scroll, or show "+N more" overflow indicator.

---

## Accessibility

- **Role / semantics:** input with `role="combobox"`, `aria-expanded`, `aria-autocomplete="list"`. Dropdown with `role="listbox"`, options with `role="option"`
- **Accessible name:** `<label>` linked to the input
- **Active option:** `aria-activedescendant` on the input
- **Selected state (multi):** each chip should be removable and announced. Use `aria-label` on remove buttons: "Remove [value]"
- **Live region:** announce result count when filtering: "5 results available" via `aria-live="polite"`
- **WCAG:** 1.3.1, 4.1.2, 2.1.1

---

## Motion & animation

- **Dropdown open/close:** same as Select — fade + translate, 150ms ease-out
- **Chip add/remove:** fade in/out with slight scale, 100ms
- **Reduced motion:** instant show/hide, no translate or scale → UX Standards §14

---

## Common pitfalls

1. **No debounce on async search** — fires a request on every keystroke, causing performance issues and rate limiting
2. **No "no results" state** — user types, sees an empty dropdown, assumes it's broken → UX Standards §5
3. **Chips without remove buttons** — in multi-select, every chip must be individually removable
4. **Dropdown doesn't handle overflow** — long option text must truncate with ellipsis, not break the dropdown width
5. **Focus lost after selection** — focus should return to the input after selecting, especially in multi-select
6. **No loading state for async** — user types and sees nothing for 500ms, assumes no results. Show a spinner immediately

---

## Tailwind notes

Input area:
- `flex flex-wrap items-center gap-1.5 rounded-md border px-3 py-2 min-h-[42px]`
- Inner input: `flex-1 min-w-[80px] outline-none bg-transparent`

Chips:
- `inline-flex items-center gap-1 rounded-full bg-gray-100 px-2.5 py-0.5 text-sm`
- Remove: `h-4 w-4 rounded-full hover:bg-gray-200`

Dropdown and option styling follows the same patterns as Select. Use portals to avoid overflow clipping.
