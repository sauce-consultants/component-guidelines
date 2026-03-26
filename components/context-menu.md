# Context Menu

> An action list triggered by right-click (desktop) or long-press (mobile). Provides contextual actions relevant to the element the user interacted with.

---

## When to use
- Providing shortcut actions on specific elements (files, rows, cards, canvas objects)
- Power-user interfaces where right-click access speeds up workflows
- Applications mimicking desktop software conventions (design tools, file managers, code editors)

## When not to use
- As the only way to access actions — always provide a visible alternative (row action menu, toolbar) for discoverability
- Simple web pages or content sites — users don't expect custom context menus outside application contexts
- Mobile-primary products — long-press is not discoverable and conflicts with OS-level actions

---

## Anatomy

1. **Trigger zone** — the element or region where right-click/long-press activates the menu. Required
2. **Menu panel** — floating list of actions. Required
3. **Menu item** — individual action with label, optional icon, optional shortcut. Required
4. **Separator** — divider between groups. Optional
5. **Submenu** — nested menu for grouped actions. Optional
6. **Destructive item** — danger-styled for irreversible actions. Conditional

---

## Variants

### Standard
- **Purpose:** contextual actions on a specific element
- **Visual distinction:** appears at the pointer position

### With submenus
- **Purpose:** grouped nested actions
- **Visual distinction:** chevron on parent items, submenu appears on hover

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Hidden | No menu visible | Default state |
| Open | Menu visible at pointer position | |
| Item hover/focus | Background highlight | |
| Item disabled | Muted, not interactive | |

---

## Interaction spec

### Mouse / pointer
- Right-click on the trigger zone opens the menu at the pointer position
- Suppress the browser's native context menu within the trigger zone
- Left-click on an item executes the action and closes the menu
- Click outside closes the menu

### Keyboard
- `Shift+F10` or the context menu key opens the menu (positioned near the focused element)
- Arrow keys navigate items
- Enter executes, Escape closes
- Full keyboard pattern matches Dropdown Menu

### Touch
- Long-press (500ms+) to trigger. Optional — not all products need this
- Haptic feedback on trigger (if the platform supports it)
- Items must be adequately spaced for touch

### Positioning
- Anchored to the pointer position (or the focused element for keyboard invocation)
- Flip to stay within viewport bounds

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Context menus are generally not appropriate on mobile. Provide visible action buttons or swipe actions instead. If long-press is supported, present as a bottom sheet |
| `md`+ | Standard floating menu at pointer position |

---

## Accessibility

- **Role / semantics:** same as Dropdown Menu: `role="menu"` on the panel, `role="menuitem"` on items
- **Keyboard invocation:** must support `Shift+F10` or the context menu key — right-click alone excludes keyboard users
- **Focus management:** on open, focus moves to the first item. On close, focus returns to the trigger element
- **Discoverability:** context menus are hidden by default. Every action available via context menu must also be available through a visible control
- **WCAG:** 4.1.2, 2.1.1

---

## Motion & animation

- **Open:** fade in + slight scale from the pointer origin, 100ms ease-out
- **Close:** fade out, 75ms ease-in
- **Reduced motion:** instant show/hide → UX Standards §14

---

## Common pitfalls

1. **Context menu as the only access to actions** — users who don't right-click (touch users, keyboard users, users unfamiliar with the convention) will never find these actions. Always provide a visible alternative
2. **Suppressing the native context menu globally** — only override right-click on elements where your custom menu is relevant. Don't override it everywhere
3. **No keyboard invocation** — if right-click opens a menu, `Shift+F10` must also work
4. **Menu appears off-screen** — always calculate position relative to viewport bounds
5. **Long-press conflicts on mobile** — long-press may conflict with text selection, OS-level actions, or drag behaviours. Test thoroughly if implementing
6. **Not providing the same items as the visible action menu** — the context menu and the row/card action menu (three-dot) should offer the same or overlapping actions

---

## Tailwind notes

Menu panel and item styling follows the same patterns as Dropdown Menu (see dropdown-menu.md).

Positioning:
- Use `position: fixed` with `top` and `left` set to the pointer coordinates via JS
- Flip calculation: check if `left + menuWidth > window.innerWidth`, if so anchor to the other side

The menu panel is rendered via a portal to avoid overflow clipping.
