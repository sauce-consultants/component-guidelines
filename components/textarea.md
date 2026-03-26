# Textarea

> A multi-line text entry field for longer-form content such as descriptions, comments, notes, and messages.

---

## When to use
- Collecting text that is expected to span multiple lines
- Comments, descriptions, notes, feedback, messages
- Any field where the user benefits from seeing more of their input at once

## When not to use
- Short, single-line values — use Input
- Rich text editing with formatting — use a dedicated rich text editor component
- Code input — use a code editor component with syntax highlighting

---

## Anatomy

1. **Label** — visible text above the field. Required → UX Standards §7.1
2. **Text area container** — the bordered, multi-line input region. Required
3. **Placeholder text** — hint text. Optional. Must not replace the label
4. **Helper text** — guidance below the field. Optional
5. **Character count** — live count showing characters used vs. limit. Optional but recommended when a limit exists
6. **Validation message** — error message below the field. Conditional
7. **Resize handle** — browser-native or custom drag handle. Optional

---

## Variants

### Default
- **Purpose:** standard multi-line text entry
- **Visual distinction:** bordered area, taller than a single-line input

### Auto-resizing
- **Purpose:** starts small, grows with content to avoid premature scrolling
- **Visual distinction:** visually identical; the textarea grows in height as the user types, up to a defined maximum

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Neutral border, label visible | Set a sensible initial height (3–5 rows) |
| Hover | Border colour intensifies | |
| Focus | Focus ring or border highlight | |
| Filled | Content visible, label remains | |
| Disabled | Muted background, not editable | |
| Read-only | Distinct from disabled, selectable | |
| Error | Error-coloured border + validation message | |

---

## Interaction spec

### Mouse / pointer
- Click to focus. Resize handle (if present) allows vertical resizing
- Consider restricting resize to vertical only (`resize-y`) to prevent layout breakage

### Keyboard
- `Tab` to focus, `Shift+Tab` to move back
- `Enter` inserts a new line (does not submit)
- Submission is handled by the parent form, not the textarea itself

### Touch
- Tap to focus. Virtual keyboard appears
- Scroll within the textarea must not conflict with page scroll

### Validation
- Validate on blur → UX Standards §7.2
- Character count updates in real time (this is feedback, not validation)

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Full-width. May need a shorter initial height to avoid dominating the screen |
| `md`+ | Width constrained by form layout. Auto-resize is especially helpful at larger sizes |

---

## Accessibility

- **Role / semantics:** `<textarea>` element
- **Accessible name:** `<label>` linked via `for`/`id`
- **Character count:** associate with the textarea via `aria-describedby`. Announce dynamically if approaching the limit (e.g. "10 characters remaining") via a live region
- **Error state:** `aria-invalid="true"` + `aria-describedby` linked to validation message
- **WCAG:** 1.3.1, 3.3.1, 3.3.2

---

## Motion & animation

- **Auto-resize:** smooth height transition, 150ms ease-out. Avoid jarring jumps
- **Focus:** border transition, 150ms ease-out
- **Reduced motion:** height transitions can be removed; immediate resize is acceptable

---

## Common pitfalls

1. **No initial height set** — a textarea with `rows="1"` looks like a regular input. Set at least 3 rows by default
2. **Horizontal resize enabled** — allows users to break the layout. Restrict to vertical resize or disable resize entirely for auto-resizing variants
3. **Character limit enforced without warning** — if there is a max length, show a live character count well before the limit is hit. Do not silently truncate
4. **Auto-resize with no max height** — the textarea can grow indefinitely and push everything off screen. Set a `max-h` and allow scrolling beyond that point
5. **Placeholder used as the label** — same rule as Input → UX Standards §7.1

---

## Tailwind notes

Structural pattern:
- Textarea: `w-full rounded-md border px-3 py-2 text-base transition-colors resize-y`
- Auto-resize: set `overflow-hidden` and adjust height via JS. Cap with `max-h-60` or similar
- Focus: same ring pattern as Input
- Character count: `text-sm text-gray-500 mt-1 text-right`
- Error: same pattern as Input

Use `min-h-[calc(theme(lineHeight.normal)*3+theme(spacing.4))]` or a simple `min-h-24` to set a sensible initial height.
