# Alert / Banner

> A persistent inline message used for warnings, errors, informational notices, or success confirmations that the user should see and potentially act on.

→ UX Standards §19.2 for persistent alert requirements.

---

## When to use
- System status warnings: degraded service, maintenance scheduled → UX Standards §2.2
- Persistent errors the user must acknowledge or resolve
- Important informational notices: policy changes, feature announcements
- Success confirmations that need more prominence than a toast

## When not to use
- Brief, auto-dismissing feedback — use Toast → UX Standards §19.1
- Critical decisions requiring user input — use Modal
- Inline field validation errors — use form validation patterns → UX Standards §7.3

---

## Anatomy

1. **Container** — coloured bar or bordered box. Required
2. **Status icon** — icon indicating the alert type (info, warning, error, success). Recommended
3. **Title** — brief heading. Optional
4. **Message** — descriptive text explaining the situation and next steps. Required → UX Standards §2.3
5. **Action** — button or link for the user to resolve the issue. Optional → UX Standards §4.4
6. **Dismiss button** — close icon to remove the alert. Optional (omit for conditions that must be resolved)

---

## Variants

### Info
- **Purpose:** neutral, informational
- **Visual distinction:** blue accent

### Warning
- **Purpose:** non-critical issue requiring awareness → UX Standards §10.1
- **Visual distinction:** amber/yellow accent

### Error
- **Purpose:** critical problem requiring attention → UX Standards §10.1
- **Visual distinction:** red accent

### Success
- **Purpose:** confirmation of a completed action (persistent version)
- **Visual distinction:** green accent

### Banner (page-level)
- **Purpose:** system-wide notices spanning the full width at the top of the page
- **Visual distinction:** full-width bar above the header or content area

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Visible | Alert displayed with appropriate styling | |
| Dismissed | Removed from view | Only if dismissible |

---

## Interaction spec

### Mouse / pointer
- Click action button/link to take the suggested action
- Click dismiss button to close (if dismissible)

### Keyboard
- Action buttons and dismiss buttons are focusable via Tab
- Activated via Enter

### Persistence
- Persistent alerts remain visible until the underlying condition is resolved or the user dismisses them → UX Standards §19.2
- Alerts for ongoing conditions (system degraded, offline) should not be dismissible — they must remain until the condition resolves → UX Standards §4.3

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Full-width. Action button may stack below the message |
| `md`+ | Inline layout: icon + text + action on one row |

Page-level banners are always full-width across all breakpoints.

---

## Accessibility

- **Role / semantics:** `role="alert"` for errors and critical warnings (triggers immediate screen reader announcement). `role="status"` for info and success. Banner-style notices may use `role="banner"` or a semantic `<aside>`
- **Structure:** if a title is present, use a heading element within the alert
- **Colour:** must be paired with icon and text → UX Standards §10.4
- **Recovery:** every error alert must offer a recovery path → UX Standards §4.4
- **WCAG:** 4.1.3 (Status Messages), 1.4.1

---

## Motion & animation

- **Enter:** fade in + height reveal, 200ms ease-out
- **Dismiss:** fade out + height collapse, 150ms ease-in
- **Reduced motion:** instant show/hide → UX Standards §13.9

---

## Common pitfalls

1. **Error alert with no recovery action** — every error must give the user something to do → UX Standards §4.4
2. **Dismissible alerts for unresolved conditions** — if the system is offline, the alert should not be closable
3. **Using an alert where a toast would suffice** — "Changes saved" is a toast, not an alert. Alerts are for persistent or actionable messages
4. **No icon or colour to indicate severity** — distinguish info from warning from error clearly, using icon + colour + text → UX Standards §10.4
5. **Alert buried below the fold** — important alerts should be near the top of the content area where the user will see them
6. **Raw error codes in alert text** — use plain English → UX Standards §2.3

---

## Tailwind notes

Container:
- `flex items-start gap-3 rounded-lg border p-4`
- Info: `bg-blue-50 border-blue-200`
- Warning: `bg-amber-50 border-amber-200`
- Error: `bg-red-50 border-red-200`
- Success: `bg-green-50 border-green-200`

(Replace with semantic tokens in projects: `bg-[var(--color-status-info-bg)]` etc.)

Icon:
- `h-5 w-5 flex-shrink-0 mt-0.5`

Message:
- `text-sm`

Action:
- `text-sm font-medium underline` or a small Button

Banner (full-width):
- `w-full border-b px-4 py-3 flex items-center justify-center gap-3`
