# Dropdown Menu

> An action list triggered from a button. Displays a list of commands or navigation options in a floating panel.

---

## When to use
- Grouping multiple actions behind a single trigger button (e.g. three-dot menu, "More" button)
- User menu with account actions (profile, settings, logout)
- Action menus on table rows → UX Standards §9.4

## When not to use
- Selecting a value for a form field — use Select or Combobox
- Showing content or details — use a Popover or Slide Panel
- Navigation between major sections — use Header or Sidebar nav

---

## Anatomy

1. **Trigger** — button that opens the menu. Required
2. **Menu panel** — floating list of items. Required
3. **Menu item** — individual action with label and optional icon/shortcut. Required
4. **Separator** — divider between groups of items. Optional
5. **Group label** — text header for a group of related items. Optional
6. **Submenu indicator** — chevron on items that open a nested submenu. Conditional
7. **Destructive item** — visually distinct for dangerous actions. Conditional

---

## Variants

### Standard
- **Purpose:** general action menu
- **Visual distinction:** text items, optional leading icons

### With icons
- **Purpose:** visual reinforcement of actions
- **Visual distinction:** icon + label per item

### With submenus
- **Purpose:** nested action groups
- **Visual distinction:** chevron on parent items, submenu appears on hover/focus

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Closed | Menu hidden, only trigger visible | |
| Open | Menu panel visible below/beside the trigger | |
| Item hover | Background highlight on hovered item | |
| Item focus | Background highlight matching hover | |
| Item disabled | Muted text, not clickable | |
| Destructive item | Red/danger text colour | |

---

## Interaction spec

### Mouse / pointer
- Click trigger to toggle menu open/close
- Click a menu item to execute the action and close the menu
- Click outside the menu to close
- Hover on a submenu item reveals the submenu

### Keyboard
- `Enter`, `Space`, or `ArrowDown` on the trigger opens the menu and focuses the first item
- `ArrowDown` / `ArrowUp` navigates items
- `Enter` or `Space` executes the focused item
- `Escape` closes the menu and returns focus to the trigger
- `ArrowRight` opens a submenu, `ArrowLeft` closes it
- Type-ahead: typing a letter jumps to the first matching item

### Touch
- Tap trigger to open. Tap item to execute
- Tap outside to close
- Items must have adequate spacing for tap targets

### Positioning
- Default: below and aligned to the trigger's start edge
- Auto-flip if the menu would overflow the viewport

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Menu may take full-width at the bottom of the screen (action sheet pattern) for easier thumb reach |
| `md`+ | Standard floating menu anchored to the trigger |

---

## Accessibility

- **Role / semantics:** trigger: `aria-haspopup="menu"`, `aria-expanded`. Menu: `role="menu"`. Items: `role="menuitem"`. Separators: `role="separator"`
- **Focus management:** on open, focus moves to the first item. On close, focus returns to the trigger
- **Disabled items:** `aria-disabled="true"`, not removed from the list (keeps spatial consistency)
- **Keyboard:** full arrow key navigation, type-ahead
- **WCAG:** 4.1.2, 2.1.1, 2.4.3

---

## Motion & animation

- **Open:** fade in + scale from trigger edge, 100–150ms ease-out
- **Close:** fade out, 75ms ease-in
- **Reduced motion:** instant show/hide → UX Standards §14

---

## Common pitfalls

1. **Menu items that don't close the menu** — executing an action should close the menu. Exception: toggle items (like checkboxes in a filter menu)
2. **No keyboard support** — dropdown menus must support full arrow key navigation and type-ahead
3. **Using a dropdown menu for form selection** — if the result populates a form field, use Select. Menus are for actions
4. **Menu clipped by overflow** — use a portal or fixed positioning to avoid parent container clipping
5. **Focus not returned to trigger on close** — after closing the menu via Escape or item click, focus must return to the trigger
6. **Destructive items with no visual distinction** — "Delete" should be in red/danger colour, separated from other items → UX Standards §10.1

---

## Tailwind notes

Trigger:
- Standard Button or Icon Button pattern

Menu panel:
- `absolute z-50 mt-1 min-w-[180px] rounded-md border bg-white shadow-lg py-1`

Menu item:
- `flex items-center gap-2 px-3 py-2 text-sm cursor-pointer transition-colors`
- Hover/focus: `bg-gray-100`
- Destructive: `text-[var(--color-status-error)] hover:bg-red-50`
- Disabled: `opacity-50 pointer-events-none`

Separator:
- `my-1 border-t`

Group label:
- `px-3 py-1.5 text-xs font-medium text-gray-400`
