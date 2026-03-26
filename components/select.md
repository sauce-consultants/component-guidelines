# Select

> A dropdown control that allows the user to choose a single value from a predefined list of options.

---

## When to use
- Choosing one option from a list of 5–15 items
- When the full list of options doesn't need to be visible at all times
- Space-constrained forms where radio buttons would take too much room

## When not to use
- Fewer than 5 options and space available — use Radio Group (all options visible = faster decision)
- Long lists (15+) or user needs to search — use Combobox
- Multiple selections — use Checkbox group or multi-select Combobox
- Binary choice — use Toggle or a pair of Radio buttons

---

## Anatomy

1. **Label** — visible text above the control. Required → UX Standards §7.1
2. **Trigger** — the button-like element showing the current selection and a chevron. Required
3. **Placeholder** — text shown when no value is selected (e.g. "Select an option"). Optional
4. **Dropdown panel** — the listbox that opens on interaction. Required
5. **Option** — individual selectable item within the panel. Required
6. **Option group label** — divider/header for grouped options. Optional
7. **Helper text** — guidance below the trigger. Optional
8. **Validation message** — error text below the trigger. Conditional

---

## Variants

### Native select
- **Purpose:** simple forms, maximum accessibility with minimal effort
- **Visual distinction:** uses the browser's native `<select>` element. Styling is limited but behaviour is rock-solid across devices

### Custom select
- **Purpose:** richer visual design, grouped options, icons in options, custom positioning
- **Visual distinction:** fully styled dropdown panel using ARIA listbox pattern. More control, more responsibility

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Trigger shows placeholder or selected value | Chevron indicates expandability |
| Hover | Border colour intensifies | |
| Focus | Focus ring on trigger | |
| Open | Dropdown visible, trigger may change border/background | Active option highlighted |
| Disabled | Muted, not interactive | |
| Error | Error-coloured border + validation message | |

---

## Interaction spec

### Mouse / pointer
- Click trigger to open dropdown. Click option to select and close
- Click outside the dropdown to close without selecting

### Keyboard
- `Enter` or `Space` or `ArrowDown` opens the dropdown when trigger is focused
- `ArrowUp` / `ArrowDown` navigates options
- `Enter` or `Space` selects the highlighted option and closes
- `Escape` closes without selecting
- Type-ahead: typing a letter jumps to the first matching option

### Touch
- Tap trigger to open. Tap option to select
- Dropdown should be positioned to avoid going off-screen, especially on mobile
- Consider using native `<select>` on mobile for better OS-level interaction

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Full-width trigger. Consider native `<select>` for better mobile UX. Dropdown may use a bottom sheet pattern |
| `md`+ | Standard dropdown below the trigger. Width matches trigger or content, whichever is wider |

On mobile, native selects give the user their OS's built-in picker (bottom wheel on iOS, full-screen list on Android), which is often faster and more accessible than a custom dropdown.

---

## Accessibility

- **Role / semantics:** native: `<select>` + `<option>`. Custom: trigger with `role="combobox"` or `role="listbox"` pattern, options with `role="option"`
- **Accessible name:** `<label>` linked to the trigger
- **Open state:** `aria-expanded="true/false"` on the trigger
- **Active option:** `aria-activedescendant` on the trigger pointing to the highlighted option
- **Keyboard nav:** full arrow key, type-ahead, and escape support
- **WCAG:** 1.3.1, 4.1.2, 2.1.1 (Keyboard)

---

## Motion & animation

- **Open:** dropdown fades in + translates from 4–8px above its final position, 150ms ease-out
- **Close:** fade out, 100ms ease-in
- **Reduced motion:** remove translate, keep fade or use instant show/hide → UX Standards §14

---

## Common pitfalls

1. **Custom select with no keyboard support** — if you build a custom dropdown, you must implement the full ARIA listbox keyboard pattern. If you can't, use native `<select>`
2. **Dropdown clipped by overflow containers** — custom dropdowns must use a portal or `position: fixed` strategy to avoid being clipped by parent `overflow: hidden`
3. **No placeholder or initial state** — if no default is selected, show clear placeholder text, not an empty trigger
4. **Too many options without search** — selects with more than ~15 options become hard to navigate. Switch to Combobox
5. **Using a select for 2–3 options** — radio buttons are almost always better when the option count is low and space exists
6. **Ignoring mobile UX** — custom selects often perform poorly on mobile. Test on real devices or default to native

---

## Tailwind notes

Trigger:
- `relative w-full rounded-md border px-3 py-2 text-left text-base flex items-center justify-between`
- Chevron: `h-5 w-5 text-gray-400 transition-transform` with `rotate-180` when open

Dropdown panel:
- `absolute z-50 mt-1 w-full rounded-md border bg-white shadow-lg max-h-60 overflow-auto`
- Option: `px-3 py-2 cursor-pointer` with `bg-gray-100` for highlighted and `font-medium` for selected
- Use `focus-visible` on the trigger, not on individual options (focus is managed via `aria-activedescendant`)

Semantic tokens for status borders and colours → UX Standards §10.2.
