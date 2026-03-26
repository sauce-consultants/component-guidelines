# Badge / Tag

> A small labelling element used to display status, categories, counts, or metadata. Badges annotate other elements; tags categorise or label content.

---

## When to use
- Status indicators in tables, cards, and lists (e.g. "Active", "Pending", "Failed")
- Category labels on content items
- Notification counts on navigation items or icons
- Filtering tags that the user can add or remove

## When not to use
- Long text — badges are for short labels (1–3 words)
- Primary actions — badges are informational, not interactive (unless they are removable tags)
- Detailed status explanations — use an Alert or inline text

---

## Anatomy

1. **Container** — small, rounded surface with background colour. Required
2. **Label** — short text. Required
3. **Dot / icon** — leading status dot or small icon. Optional
4. **Remove button** — close icon for removable tags. Conditional

---

## Variants

### Status badge
- **Purpose:** communicate state using semantic colours
- **Visual distinction:** coloured background matching the status (red/error, amber/warning, green/success, blue/info, grey/neutral) → UX Standards §10.1

### Category tag
- **Purpose:** label content with a category or type
- **Visual distinction:** neutral or theme-coloured background. May be removable

### Count badge
- **Purpose:** notification count overlaid on an icon or nav item
- **Visual distinction:** small circular badge with a number, typically positioned top-right of the parent element

### Dot badge
- **Purpose:** minimal presence indicator (e.g. unread, online)
- **Visual distinction:** small coloured dot with no text

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Coloured background with contrasting text | |
| Hover (removable) | Background darkens slightly, remove icon appears or becomes more prominent | Only for interactive tags |
| Focus (removable) | Focus ring on the tag or the remove button | |

---

## Interaction spec

### Removable tags
- Click the remove button to remove the tag
- The remove button is the interactive element, not the whole tag
- Removing a tag may update a filter or a form value

### Count badges
- Not interactive themselves — the parent element (e.g. bell icon) is the interactive target

### Keyboard
- Removable tags: remove button focusable via Tab, activated via Enter or Backspace
- Non-interactive badges are not focusable

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| All | Badges are small and responsive by nature. No breakpoint-specific changes |

In constrained spaces (table cells), badges may truncate with ellipsis if the label is too long.

---

## Accessibility

- **Status badges:** colour must be paired with the text label → UX Standards §10.4, §9.5. A green badge that says "Active" is accessible; a green dot alone is not
- **Count badges:** if overlaid on an icon, the count must be announced. Use `aria-label` on the parent: "Notifications (3)" rather than relying on the visual badge alone
- **Removable tags:** remove button needs `aria-label`: "Remove [tag name]"
- **Dot badges:** pair with a text label or aria-label on the parent element
- **WCAG:** 1.4.1 (Use of Colour), 1.1.1

---

## Motion & animation

- **Tag add:** fade in with slight scale, 100ms ease-out
- **Tag remove:** fade out, 100ms ease-in
- **Count change:** brief scale pulse (1.0 → 1.2 → 1.0), 200ms
- **Reduced motion:** instant add/remove, no pulse

---

## Common pitfalls

1. **Colour-only status** — a red badge without text is meaningless to colour-blind users. Always include a text label → UX Standards §10.4
2. **Inconsistent status colours** — red must mean the same thing everywhere → UX Standards §10.1
3. **Badges that are too large** — badges should be compact. If the label needs more than 2–3 words, it's not a badge — it's inline text
4. **Count badge without accessible announcement** — the number "3" overlaid on a bell icon is invisible to screen readers without an `aria-label`
5. **Using brand colour for status** — brand colours must not overlap with status colours → UX Standards §10.1

---

## Tailwind notes

Status badge:
- `inline-flex items-center gap-1.5 rounded-full px-2.5 py-0.5 text-xs font-medium`
- Success: `bg-[var(--color-status-success)]/10 text-[var(--color-status-success)]`
- Error: `bg-[var(--color-status-error)]/10 text-[var(--color-status-error)]`
- Warning: `bg-[var(--color-status-warning)]/10 text-[var(--color-status-warning)]`

Count badge:
- `absolute -top-1 -right-1 h-5 min-w-5 rounded-full bg-red-500 text-white text-xs flex items-center justify-center px-1`

Removable tag:
- `inline-flex items-center gap-1 rounded-full bg-gray-100 pl-2.5 pr-1 py-0.5 text-sm`
- Remove: `h-4 w-4 rounded-full hover:bg-gray-200 flex items-center justify-center`

Dot:
- `h-2.5 w-2.5 rounded-full` with semantic colour
