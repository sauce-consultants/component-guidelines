# Checkbox

> A toggle control for binary choices or multi-select scenarios. Used when the user can select zero, one, or many options from a group.

---

## When to use
- Toggling a single boolean value (e.g. "I agree to the terms")
- Selecting multiple items from a list
- Bulk selection in tables (select all / select row)

## When not to use
- Choosing one option from a mutually exclusive set — use Radio Group
- Toggling a setting that takes effect immediately — use Toggle / Switch
- Choosing from a long list — use multi-select Combobox

---

## Anatomy

1. **Checkbox control** — the visual indicator (box). Required
2. **Label** — descriptive text beside the control. Required → UX Standards §7.1
3. **Check mark** — visible when checked. Required
4. **Indeterminate indicator** — horizontal line/dash shown when a parent checkbox has mixed child states. Conditional
5. **Helper text** — additional guidance below the label. Optional

---

## Variants

### Single checkbox
- **Purpose:** standalone boolean toggle
- **Visual distinction:** single control + label pair

### Checkbox group
- **Purpose:** multi-select from a defined set
- **Visual distinction:** vertically stacked checkboxes under a group label

### With indeterminate state
- **Purpose:** parent/child relationships (e.g. "Select all" when some children are checked)
- **Visual distinction:** dash icon instead of check mark

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Unchecked | Empty bordered box | |
| Checked | Filled box with check mark icon | |
| Indeterminate | Filled box with dash/minus icon | Only for parent checkboxes with mixed children |
| Hover | Border or background intensifies | Apply to both control and label area |
| Focus | Focus ring on the control | → UX Standards §13.5 |
| Disabled | Muted, not interactive | Can be disabled in any check state |
| Error | Error-coloured border/outline + validation message | Rare for individual checkboxes, more common for required groups |

---

## Interaction spec

### Mouse / pointer
- Click the control or the label to toggle state
- The entire label + control row should be clickable

### Keyboard
- `Space` toggles the checkbox when focused
- `Tab` moves between checkboxes (or to the next form element)
- In a group, each checkbox is individually focusable

### Touch
- Tap target covers the control + label. Minimum 44×44px hit area → UX Standards §13.6

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Checkbox groups stack vertically. Ensure generous spacing between items for tap targets |
| `md`+ | Groups may use 2-column layout if there are many options and screen width allows |

---

## Accessibility

- **Role / semantics:** `<input type="checkbox">` with `<label>`
- **Group:** wrap in `<fieldset>` with `<legend>` for checkbox groups
- **Indeterminate:** set via JS (`checkbox.indeterminate = true`). Announced as "mixed" by screen readers
- **Required group:** if at least one must be selected, use `aria-required` on the group and show a validation message if none selected
- **WCAG:** 1.3.1, 4.1.2, 2.1.1

---

## Motion & animation

- **Check/uncheck:** check mark and fill scale in from centre, 100–150ms ease-out
- **Reduced motion:** instant state change, no scale animation

---

## Common pitfalls

1. **Using checkboxes for mutually exclusive options** — if only one can be selected, use Radio Group
2. **Clickable area limited to the box** — the label must also toggle the checkbox. Use proper `<label>` linkage or wrap both in a clickable container
3. **Indeterminate state not handled** — when implementing select-all patterns, forgetting the indeterminate visual state confuses users about what's selected
4. **No fieldset/legend for groups** — screen readers need group context. A heading above a list of checkboxes is not sufficient; use `<fieldset>` and `<legend>`
5. **Custom checkbox that loses native behaviour** — if building a styled checkbox, ensure it still works with form submission, keyboard, and assistive tech. Test thoroughly

---

## Tailwind notes

Control:
- `h-5 w-5 rounded border-2 transition-colors flex items-center justify-center`
- Checked: `bg-[var(--color-action-primary)] border-[var(--color-action-primary)]`
- Unchecked: `border-gray-300`
- Focus: `focus-visible:ring-2 focus-visible:ring-offset-2`

Layout:
- Single: `flex items-start gap-3` (items-start keeps alignment with first line of multi-line labels)
- Group: `flex flex-col gap-3` or `grid grid-cols-2 gap-3` for two-column at wider breakpoints
- Label: `text-base cursor-pointer select-none`

Use a visually hidden native `<input type="checkbox">` with a styled sibling for full native behaviour with custom appearance.
