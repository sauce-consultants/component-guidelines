# Toast / Notification

> A brief, non-blocking feedback message that appears temporarily to confirm an action or communicate a status change. Auto-dismisses after a short duration.

→ UX Standards §19.1 for toast requirements.

---

## When to use
- Confirming completed actions: "Changes saved", "Record deleted", "Email sent"
- Brief status updates that don't require user action
- Non-critical information the user should see but doesn't need to act on

## When not to use
- Information the user must act on — use Alert / Banner → UX Standards §19.2
- Error messages requiring resolution — use inline errors or Alert
- Critical warnings — use Modal or persistent Alert
- Actions the user needs to take — toasts should not contain critical workflows → UX Standards §19.1

---

## Anatomy

1. **Container** — small floating bar. Required
2. **Icon** — status icon (check, warning, error, info). Optional but recommended
3. **Message** — brief text. Required. One sentence max
4. **Action link** — a single optional action (e.g. "Undo"). Optional
5. **Dismiss button** — close icon to dismiss early. Optional

---

## Variants

### Success
- **Purpose:** confirming a successful action
- **Visual distinction:** green accent or check icon

### Error
- **Purpose:** brief error notification (non-critical)
- **Visual distinction:** red accent or error icon

### Warning
- **Purpose:** non-critical warning
- **Visual distinction:** amber accent or warning icon

### Info
- **Purpose:** neutral informational message
- **Visual distinction:** blue accent or info icon

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Entering | Animates in from its position | |
| Visible | Fully rendered with auto-dismiss timer running | |
| Hovering | Timer pauses while the user hovers | User may be reading or about to click "Undo" |
| Exiting | Animates out | |

---

## Interaction spec

### Mouse / pointer
- Hover pauses the auto-dismiss timer
- Click the action link to trigger the action (e.g. Undo)
- Click dismiss button to close immediately

### Keyboard
- Toasts should be announced to screen readers but should not steal focus
- If a toast has an action, it should be reachable via keyboard (see accessibility)

### Touch
- Swipe to dismiss (optional enhancement)
- Action link and dismiss button meet tap target requirements

### Timing
- Auto-dismiss after 3–5 seconds → UX Standards §19.1
- If the toast has an action (like "Undo"), extend to 5–8 seconds to give the user time to act
- No artificial minimum display time for simple confirmations

### Stacking
- Multiple toasts stack in a consistent direction (e.g. newest at the top)
- Limit visible toasts to 3–5; queue additional ones
- Toasts must not overlap interactive content → UX Standards §19.3

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Full-width or nearly full-width, positioned at the bottom of the screen |
| `md`+ | Fixed-width (max ~400px), positioned top-right or bottom-centre → UX Standards §19.3 |

---

## Accessibility

- **Role / semantics:** `role="status"` or use `aria-live="polite"` on the toast container. This announces the message without stealing focus
- **Focus:** toasts should not take focus. If a toast has an action (Undo), consider adding the action to the page's focus order as a separate element or using `role="alertdialog"` for critical actions
- **Timing:** users with disabilities may need more time. Pausable on hover helps, but critical actions should not rely on timed toasts
- **WCAG:** 4.1.3 (Status Messages), 2.2.1 (Timing Adjustable — toast actions must not be the only way to perform the action)

---

## Motion & animation

- **Enter:** slide in + fade from the edge (e.g. slide down from top-right), 200ms ease-out
- **Exit:** fade out + slide, 150ms ease-in
- **Stack shift:** existing toasts shift smoothly when a new one enters, 200ms ease-out
- **Reduced motion:** instant show/hide, no slide → UX Standards §13.9

---

## Common pitfalls

1. **Critical actions only available via toast** — "Undo" in a toast that disappears after 5 seconds must not be the only way to undo. Provide alternative recovery paths → UX Standards §19.1
2. **Toasts that don't auto-dismiss** — if it needs to persist, it's not a toast — it's an Alert
3. **Toasts covering interactive content** — position toasts away from primary action areas → UX Standards §19.3
4. **No `aria-live` announcement** — without it, screen reader users never know the toast appeared
5. **Too many simultaneous toasts** — rapid actions can flood the screen. Queue and limit visible count
6. **Long text in toasts** — keep messages to one short sentence. If more explanation is needed, use a different pattern

---

## Tailwind notes

Container (toast region):
- `fixed z-50 flex flex-col gap-2`
- Top-right: `top-4 right-4`
- Bottom-centre: `bottom-4 left-1/2 -translate-x-1/2`
- Mobile: `bottom-4 left-4 right-4`

Individual toast:
- `flex items-center gap-3 rounded-lg border bg-white px-4 py-3 shadow-lg`

Status icon:
- Success: `text-[var(--color-status-success)]`
- Error: `text-[var(--color-status-error)]`

Message:
- `text-sm text-gray-900`

Action:
- `text-sm font-medium text-[var(--color-action-primary)] hover:underline`

Dismiss:
- Icon button pattern, small size
