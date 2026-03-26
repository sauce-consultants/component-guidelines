# Button

> A clickable control that triggers an action. The most fundamental interactive element in any interface.

---

## When to use
- To trigger an action: submit, save, delete, navigate, open
- For any user-initiated operation that changes state or starts a process

## When not to use
- To navigate to a new page or URL — use a link instead
- For toggling a boolean setting — use a Toggle / Switch
- For selecting from options — use Radio Group or Select
- When the action is contextual to a single row in a table — consider Icon Button or a row action menu

---

## Anatomy

1. **Container** — the clickable hit area. Required
2. **Label** — text describing the action. Required for standard buttons
3. **Leading icon** — optional icon before the label for visual reinforcement
4. **Trailing icon** — optional icon after the label (e.g. chevron for menus, external link indicator)
5. **Loading indicator** — spinner or animation replacing or accompanying the label during async actions. Conditional

---

## Variants

### Primary
- **Purpose:** the single most important action on the screen
- **Visual distinction:** filled background using the brand/action colour, high contrast text
- **Constraint:** maximum one per screen → UX Standards §8.1

### Secondary
- **Purpose:** supporting actions alongside the primary
- **Visual distinction:** outlined or muted fill, visually subordinate to primary

### Ghost / Tertiary
- **Purpose:** low-emphasis actions, often used in toolbars or alongside content
- **Visual distinction:** no border or fill in default state; background appears on hover/focus

### Destructive
- **Purpose:** actions that delete, remove, or permanently alter data
- **Visual distinction:** red/danger colour treatment, clearly separated from primary/secondary → UX Standards §8.1, §10.1

### Link-style
- **Purpose:** actions that look like inline links but behave as buttons (no navigation)
- **Visual distinction:** underlined text, no container. Use sparingly to avoid confusion with actual links

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Variant-appropriate fill, border, text colour | Resting state |
| Hover | Slight background shift (darken filled, add fill to ghost) | Pointer cursor |
| Focus | Visible focus ring, high contrast | Must not be suppressed → UX Standards §13.5 |
| Active / Pressed | Further darkened or scaled slightly (scale: 0.98) | Brief, tactile feedback |
| Disabled | Reduced opacity (0.5) or muted colours | Must explain why if possible → UX Standards §8.3 |
| Loading | Spinner replaces or sits beside label; button disabled | Button must not change size → UX Standards §3.4 |

---

## Interaction spec

### Mouse / pointer
- Single click triggers the action
- No double-click behaviour — prevent duplicate submissions during loading state
- Hover shows pointer cursor and hover state

### Keyboard
- `Enter` or `Space` triggers the action
- Button must be focusable via `Tab`
- Focus order follows DOM order → UX Standards §13.5

### Touch
- Minimum tap target: 44×44px → UX Standards §13.6
- If the visual button is smaller, expand the hit area with padding
- Active state must trigger on touch start for responsiveness

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs` (375px) | Full-width buttons in stacked forms. Icon-only variants acceptable where space is constrained |
| `sm` (640px) | Buttons may sit side-by-side in action groups |
| `md` (768px+) | Standard inline layout. Button groups align to content flow |

Action groups (e.g. "Save" + "Cancel") should stack vertically on `xs` with the primary action on top. On `sm` and above, arrange horizontally with the primary action on the trailing side (right in LTR layouts).

---

## Accessibility

- **Role / semantics:** use `<button>` element. Never use `<div>` or `<span>` with click handlers
- **Accessible name:** derived from the visible label text. For icon-only buttons, use `aria-label`
- **Keyboard nav:** focusable via Tab, activated via Enter or Space
- **Screen reader:** label is announced. Loading state should be communicated via `aria-busy="true"` or a live region announcing "Loading" / "Please wait"
- **Disabled state:** use the `disabled` attribute, not `aria-disabled`, unless the button needs to remain in tab order for discoverability
- **WCAG:** 2.1 — 4.1.2 (Name, Role, Value), 2.4.7 (Focus Visible), 1.4.3 (Contrast)

---

## Motion & animation

- **Hover transition:** background colour shift, 150ms ease-out
- **Active/pressed:** subtle scale (0.98), 100ms ease-in
- **Loading spinner:** continuous rotation, no fixed duration
- **Reduced motion:** disable scale transition; spinner may continue as it is functional, not decorative → UX Standards §14, §13.9

---

## Common pitfalls

1. **Using a `<div>` or `<a>` instead of `<button>`** — loses keyboard support, focus management, and screen reader semantics. If it triggers an action and doesn't navigate, it must be a `<button>`
2. **Multiple primary buttons on one screen** — violates the one-primary-action rule. Audit the screen and demote secondary actions
3. **Generic labels like "Submit" or "OK"** — labels must describe the action: "Save changes", "Delete record" → UX Standards §8.2
4. **Button changes size when entering loading state** — set a minimum width or use fixed dimensions so the spinner doesn't collapse the button → UX Standards §3.4
5. **No loading state on async actions** — clicking a button that calls an API must show loading and disable the button to prevent double submission
6. **Disabled button with no explanation** — if a user can see a button but can't use it, tell them why via tooltip or hint text → UX Standards §8.3
7. **Destructive buttons styled the same as primary** — delete/archive actions must be visually distinct (danger colour) and never the default/primary action in a confirmation dialog → UX Standards §7.5

---

## Tailwind notes

Use semantic colour tokens rather than raw Tailwind palette classes for variant colours → UX Standards §10.2.

Structural patterns:
- Base: `inline-flex items-center justify-center font-medium rounded-md transition-colors`
- Sizing: use consistent height classes (`h-9`, `h-10`, `h-11`) and horizontal padding (`px-4`, `px-6`) per size variant
- Focus: `focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-[var(--color-action-primary)]`
- Disabled: `disabled:opacity-50 disabled:pointer-events-none`
- Full-width on mobile: `w-full sm:w-auto`
- Loading state: position the spinner absolutely or use flex gap to avoid layout shift
