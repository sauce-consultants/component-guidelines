# Input

> A single-line text entry field. The most common form control, used for short-form text like names, emails, numbers, and search terms.

---

## When to use
- Collecting a single line of text: names, emails, URLs, phone numbers, search queries
- Numeric input where a slider or stepper is not appropriate
- Any field where the expected value is short (one line)

## When not to use
- Multi-line text — use Textarea
- Selecting from a predefined list — use Select or Combobox
- Date entry — use Date Picker (though a text input with masking is an acceptable fallback)
- File selection — use File Upload

---

## Anatomy

1. **Label** — visible text above or beside the field. Required → UX Standards §7.1
2. **Input container** — the bordered area the user types into. Required
3. **Placeholder text** — hint text inside the field. Optional. Must not replace the label → UX Standards §7.1
4. **Leading adornment** — icon or text prefix inside the field (e.g. currency symbol, search icon). Optional
5. **Trailing adornment** — icon or text suffix (e.g. unit label, clear button, visibility toggle). Optional
6. **Helper text** — descriptive text below the field providing guidance. Optional
7. **Validation message** — error or success message below the field, replacing or supplementing helper text. Conditional
8. **Required indicator** — text label marking the field as required or optional → UX Standards §7.4

---

## Variants

### Text
- **Purpose:** general-purpose single-line text entry
- **Visual distinction:** standard input styling

### Email / URL / Tel
- **Purpose:** semantically typed inputs that trigger appropriate mobile keyboards
- **Visual distinction:** visually identical to text; the difference is the `type` attribute

### Password
- **Purpose:** sensitive text entry with masked characters
- **Visual distinction:** masked dots, trailing visibility toggle icon

### Number
- **Purpose:** numeric values. Use `inputmode="numeric"` with a text type rather than `type="number"` for most cases to avoid browser spinner issues
- **Visual distinction:** standard styling; optional trailing unit label

### Search
- **Purpose:** search queries (see also the Search composite pattern)
- **Visual distinction:** leading search icon, optional trailing clear button

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Neutral border, label visible | |
| Hover | Border colour intensifies slightly | |
| Focus | Prominent focus ring or border colour change | → UX Standards §13.5 |
| Filled | Value visible, label remains visible | |
| Disabled | Muted background, reduced opacity, not editable | |
| Read-only | Visually distinct from disabled (no opacity reduction), not editable but selectable | |
| Error | Border and validation message in error colour (red). Icon may accompany message | → UX Standards §10.1 |
| Success | Border and message in success colour (green). Use sparingly | |

---

## Interaction spec

### Mouse / pointer
- Click anywhere in the input container or on the label to focus the field
- Click on adornment buttons (clear, toggle visibility) triggers their action without blurring the input

### Keyboard
- `Tab` to focus, `Shift+Tab` to move back
- Standard text editing keys (select all, copy, paste)
- `Escape` could clear the field or revert to previous value (context-dependent)

### Touch
- Tap to focus. Appropriate virtual keyboard should appear based on input type
- Clear button (if present) must meet 44×44px tap target → UX Standards §13.6

### Validation timing
- Validate on blur, not on every keystroke → UX Standards §7.2
- Exception: password strength indicators update in real time → UX Standards §7.2

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs` (375px) | Full-width fields. Labels stacked above the input. Adornments must not crowd the tap target |
| `sm`–`md` | Fields may sit in multi-column layouts (e.g. first name / last name side-by-side) |
| `lg`+ | Constrain max-width of inputs to maintain readable line length. Very wide inputs feel broken |

Inputs should never stretch beyond ~600px wide without good reason. On wide screens, constrain with `max-w-md` or similar.

---

## Accessibility

- **Role / semantics:** `<input>` element with appropriate `type`
- **Accessible name:** `<label>` element linked via `for`/`id`. Do not rely on placeholder as the label → UX Standards §7.1
- **Required fields:** use `aria-required="true"` and a visible indicator → UX Standards §7.4
- **Error state:** link validation message to the input via `aria-describedby`. Use `aria-invalid="true"` when in error
- **Keyboard nav:** standard, native input behaviour
- **WCAG:** 1.3.1 (Info and Relationships), 1.3.5 (Identify Input Purpose via `autocomplete`), 3.3.1 (Error Identification), 3.3.2 (Labels or Instructions)

---

## Motion & animation

- **Focus transition:** border colour change, 150ms ease-out
- **Validation message appearance:** fade in + slight translate-y, 150ms ease-out. Do not animate the error border — it should appear immediately
- **Reduced motion:** remove translate; fade and colour changes are acceptable

---

## Common pitfalls

1. **Placeholder as label** — placeholders disappear on input and are not reliably read as labels by screen readers. Always use a visible `<label>` → UX Standards §7.1
2. **Using `type="number"` for most numeric fields** — browser implementations vary wildly (scroll-to-change, missing spinner on some platforms). Prefer `type="text" inputmode="numeric" pattern="[0-9]*"` for controlled numeric entry
3. **Validating on every keystroke** — interrupts the user mid-input and produces false errors. Validate on blur → UX Standards §7.2
4. **No `autocomplete` attribute** — browsers and password managers rely on `autocomplete` to fill fields correctly. Set it to the appropriate value (e.g. `name`, `email`, `new-password`)
5. **Form resets on submission error** — if the form fails to submit, all previously entered data must be preserved → UX Standards §7.7
6. **Tiny clear buttons inside the input** — the clear/toggle adornment must meet tap target requirements even though it's inside the field

---

## Tailwind notes

Structural pattern:
- Container: `relative flex items-center`
- Input: `w-full rounded-md border px-3 py-2 text-base transition-colors`
- Focus: `focus:outline-none focus:ring-2 focus:ring-[var(--color-action-primary)] focus:border-[var(--color-action-primary)]`
- Error: `border-[var(--color-status-error)]` with `text-[var(--color-status-error)]` on the validation message
- Label: `block text-sm font-medium mb-1.5`
- Helper/validation text: `text-sm mt-1.5`
- Adornments: `absolute inset-y-0` with `left-0 pl-3` or `right-0 pr-3`, centred with `flex items-center`

Use `peer` utilities to connect validation states from the input to sibling elements if needed. Use semantic colour tokens for status borders and text → UX Standards §10.2.
