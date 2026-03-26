# Modal / Dialog

> A focused overlay that requires the user's attention or input before they can return to the page. Blocks interaction with the content beneath.

---

## When to use
- Confirmation prompts for destructive actions → UX Standards §7.5
- Short forms that don't warrant a full page (quick edit, add item)
- Critical alerts that require acknowledgement
- Focused tasks that benefit from removing surrounding distractions

## When not to use
- Displaying detail views or long content — use Slide Panel or a dedicated page
- Non-blocking feedback — use Toast
- Content the user didn't request — unsolicited modals are hostile
- Stacking modals on top of modals — redesign the flow instead

---

## Anatomy

1. **Backdrop** — semi-transparent overlay covering the page. Required
2. **Dialog container** — centred panel with the modal content. Required
3. **Header** — title and optional close button. Required
4. **Body** — main content: text, form fields, or information. Required
5. **Footer** — action buttons (confirm, cancel). Required for action modals
6. **Close button** — ✕ icon in the top-right corner. Required

---

## Variants

### Confirmation
- **Purpose:** "Are you sure?" prompts for destructive or significant actions
- **Visual distinction:** short text + two buttons. Destructive button uses danger styling → UX Standards §7.5

### Form
- **Purpose:** quick data entry without leaving the current page
- **Visual distinction:** contains form fields with submit/cancel buttons

### Informational
- **Purpose:** displaying information the user must acknowledge
- **Visual distinction:** text content with a single dismiss button

### Full-screen (mobile)
- **Purpose:** modals on small screens where a centred dialog is too constrained
- **Visual distinction:** takes the full viewport on mobile, standard dialog on desktop

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Closed | Not rendered or hidden | |
| Open | Backdrop + centred dialog visible | |
| Loading | Button loading state within the modal (for async confirm actions) | → UX Standards §3.4 |

---

## Interaction spec

### Mouse / pointer
- Click backdrop to close (for non-destructive modals). Destructive confirmation modals should not close on backdrop click
- Click close button (✕) to close
- Click action buttons to confirm or cancel

### Keyboard
- `Escape` closes the modal (unless it's a required confirmation)
- `Tab` cycles through interactive elements within the modal — focus is trapped
- On open, focus moves to the first interactive element (or the dialog itself if no interactive elements)
- On close, focus returns to the element that triggered the modal

### Touch
- Standard tap interactions. Ensure action buttons meet 44×44px tap targets

### Focus management
- Focus trap is mandatory — the user must not be able to Tab to content behind the modal
- Focus order: close button (if present) → form fields / content → action buttons
- Return focus to trigger element on close

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Full-width with margin (e.g. `mx-4`), or full-screen variant. Bottom-anchored action buttons for thumb reachability |
| `md`+ | Centred dialog, max-width of `sm` (480px) to `lg` (640px) depending on content |

Long modal content should scroll within the body, not cause the entire modal to scroll. Header and footer remain fixed.

---

## Accessibility

- **Role / semantics:** `role="dialog"` with `aria-modal="true"`. Use `<dialog>` element where supported
- **Label:** `aria-labelledby` pointing to the modal title
- **Description:** `aria-describedby` pointing to the body content (optional but helpful for screen readers)
- **Focus trap:** mandatory. Use a JS focus trap utility
- **Close:** Escape key and close button always available
- **Backdrop:** not focusable. Clicking it closes the dialog (except for mandatory confirmations)
- **WCAG:** 2.4.3 (Focus Order), 2.1.2 (No Keyboard Trap — trap is modal-scoped), 4.1.2

---

## Motion & animation

- **Open:** backdrop fades in (200ms). Dialog fades in + scales from 0.95, 200ms ease-out
- **Close:** reverse, 150ms ease-in
- **Reduced motion:** instant show/hide → UX Standards §13.9

---

## Common pitfalls

1. **No focus trap** — without a focus trap, keyboard users can Tab behind the modal and interact with the page, creating confusion
2. **Focus not returned on close** — after closing, focus must go back to the trigger element, not the top of the page
3. **Destructive action as the primary/default button** — in confirmation dialogs, the non-destructive option (Cancel) should be the default. The destructive button must require deliberate action → UX Standards §7.5
4. **Generic button labels** — "OK" / "Cancel" is weak. Use "Delete record" / "Keep record" → UX Standards §8.2
5. **Modal for content that should be a page** — if the modal requires scrolling through extensive content, it should be a page or Slide Panel
6. **Stacked modals** — opening a modal from within a modal is a UX failure. Restructure the flow
7. **Unsolicited modals** — never open a modal the user didn't request (newsletter popups, feature announcements). These are hostile patterns

---

## Tailwind notes

Backdrop:
- `fixed inset-0 z-50 bg-black/50`

Dialog:
- `fixed inset-0 z-50 flex items-center justify-center p-4`
- Panel: `relative w-full max-w-md rounded-lg bg-white shadow-xl`

Header:
- `flex items-center justify-between px-6 pt-6 pb-2`
- Title: `text-lg font-semibold`
- Close: icon button pattern (see icon-button.md)

Body:
- `px-6 py-4 overflow-y-auto max-h-[60vh]`

Footer:
- `flex items-center justify-end gap-3 px-6 pb-6 pt-2`

Destructive confirmation:
- Cancel: secondary button style
- Confirm: `bg-[var(--color-status-error)] text-white` — clearly danger-styled
