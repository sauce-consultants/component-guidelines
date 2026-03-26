# Radio Group

> A set of mutually exclusive options where the user selects exactly one. All options are visible at once, enabling quick comparison.

---

## When to use
- Selecting one option from 2–7 mutually exclusive choices
- When the user benefits from seeing all options at once to compare
- Settings or forms where the choice is meaningful and deserves visibility

## When not to use
- More than 7 options — use Select or Combobox
- Multiple selections allowed — use Checkbox group
- Binary toggle that takes immediate effect — use Toggle / Switch
- Space is very constrained — use Select

---

## Anatomy

1. **Group label** — describes what the set of options represents. Required (as `<legend>`)
2. **Radio control** — the circular indicator. Required per option
3. **Option label** — descriptive text beside each control. Required
4. **Option description** — secondary text for complex options. Optional
5. **Helper text** — guidance below the group. Optional
6. **Validation message** — error text when no selection is made on a required group. Conditional

---

## Variants

### Stacked (default)
- **Purpose:** standard vertical layout with one option per row
- **Visual distinction:** radio + label pairs stacked vertically

### Inline
- **Purpose:** short option labels in a horizontal row
- **Visual distinction:** options arranged horizontally. Only suitable for 2–4 short labels

### Card-style
- **Purpose:** rich option selection where each choice has a title, description, or icon
- **Visual distinction:** each option is a bordered card. Selected card has a highlighted border or background

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Unselected | Empty circle | |
| Selected | Filled circle with inner dot | |
| Hover | Background highlight on the option row/card | |
| Focus | Focus ring on the radio control | → UX Standards §13.5 |
| Disabled | Muted, not interactive | Can be disabled per-option or for the whole group |
| Error | Error border/message on the group level | Shown when a required group has no selection |

---

## Interaction spec

### Mouse / pointer
- Click the control or the label to select that option
- Selecting one option deselects any previously selected option

### Keyboard
- `Tab` enters the radio group (focuses the selected option, or the first option if none selected)
- `ArrowUp` / `ArrowDown` (or `ArrowLeft` / `ArrowRight` for inline) moves selection between options
- Arrow keys both move focus and select — this is the native radio group pattern
- `Tab` again exits the group

### Touch
- Tap target covers the control + label area. 44×44px minimum → UX Standards §13.6

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Inline variants revert to stacked. Card variants stack single-column |
| `md`+ | Inline is acceptable for short labels. Card variants may use 2-column grid |

---

## Accessibility

- **Role / semantics:** `<fieldset>` with `<legend>` wrapping `<input type="radio">` elements sharing the same `name`
- **Group label:** the `<legend>` provides the group label for screen readers
- **Keyboard pattern:** native radio group arrow-key navigation. Do not override this
- **Required:** `aria-required` on the group. Show validation error if submitted without selection
- **WCAG:** 1.3.1, 4.1.2, 2.1.1

---

## Motion & animation

- **Selection:** fill and inner dot scale in, 100–150ms ease-out
- **Card variant:** border colour transition, 150ms ease-out
- **Reduced motion:** instant state change

---

## Common pitfalls

1. **No default selection on required groups** — radio buttons cannot be deselected once chosen. If no default is appropriate, accept that the user might submit without selecting and handle validation. Do not pre-select an option the user didn't choose
2. **Overriding native keyboard behaviour** — radio groups have a specific arrow-key pattern. Do not make each radio tab-focusable independently; this breaks the expected pattern
3. **Missing `<fieldset>` and `<legend>`** — screen readers need these to understand the group. A `<div>` with a heading is not equivalent
4. **Too many options** — more than 7 radio buttons clutters the form. Switch to Select or Combobox
5. **Card variant without clear selected state** — when options are cards, the selected state must be visually obvious. A subtle border change is not enough; use a combination of border, background, and a check icon

---

## Tailwind notes

Stacked layout:
- Group: `fieldset flex flex-col gap-3`
- Option row: `flex items-start gap-3`
- Control: `h-5 w-5 rounded-full border-2 transition-colors`
- Selected: `border-[var(--color-action-primary)]` with inner dot via pseudo-element or nested element

Card layout:
- Option card: `relative rounded-lg border-2 p-4 cursor-pointer transition-colors`
- Selected card: `border-[var(--color-action-primary)] bg-[var(--color-action-primary)]/5`
- Include a hidden radio input inside each card for native form behaviour

Inline layout:
- Group: `flex flex-wrap gap-4` at `md`+, `flex flex-col gap-3` at `xs`–`sm`
