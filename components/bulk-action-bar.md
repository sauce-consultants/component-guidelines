# Bulk action bar

> A contextual toolbar that appears when one or more items are selected in a table or list, showing the selection count and available bulk actions.

→ UX Standards §9.7 for bulk selection requirements.

---

## When to use

- Tables where the user can select multiple rows and perform actions on all of them (delete, archive, export, assign, change status)
- List views (card lists, grid layouts) where multi-select is supported
- Any collection where the user needs to operate on a batch of items rather than one at a time

## When not to use

- Collections that do not support multi-select — if the user can only act on one item at a time, there is no bulk action bar
- Single-action bulk operations where a global "Select all and do X" is sufficient — consider a simpler control rather than a full bar
- Inline row actions — these are per-item, not bulk (→ dropdown-menu.md, → context-menu.md)

---

## Anatomy

1. **Container** — a bar that appears in a fixed position relative to the collection. Required
2. **Selection count** — text showing the number of selected items (e.g. "3 selected"). Required → UX Standards §9.7
3. **Select all / deselect all** — a control to select all items in the dataset (not just the visible page) or clear the selection. Required. When the user has selected all items on the current page, offer "Select all 286 customers" to extend the selection across pages
4. **Action buttons** — the actions available for the selected items. Required. At least one action must be present. Destructive actions use the destructive button variant (→ button.md)
5. **Dismiss / cancel** — a way to clear the selection and close the bar. This may be the deselect all control, or a separate close button. Required

---

## Variants

### Inline

- **Purpose:** appears within the collection layout, typically replacing or overlaying the table header row or sitting between the filter bar and the collection
- **Visual distinction:** same width as the collection. Background colour distinguishes it from the surrounding content (subtle accent or elevated surface)

### Fixed / sticky

- **Purpose:** remains visible as the user scrolls through a long collection. Useful for large datasets where the user may select items across multiple scroll positions
- **Visual distinction:** sticks to the top or bottom of the collection's scroll container. Elevated with a shadow to indicate it is floating above the content

### Floating bar

- **Purpose:** a detached bar, typically centred at the bottom of the viewport, that appears over the content. Common in modern interfaces where the bar should not displace the collection layout
- **Visual distinction:** fixed to the bottom of the viewport or content region. Rounded, elevated with shadow, visually distinct from the page content. Narrower than full width

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Hidden | Not rendered | No items selected |
| Visible | Bar appears with count and actions | One or more items selected |
| Page selected | Count reflects current page selection. "Select all X" link appears | All items on the visible page are checked |
| All selected | Count reflects total dataset size. "Deselect all" replaces "Select all" | Selection extends across all pages |
| Action loading | The triggered action button enters loading state. Other action buttons disabled. Bar remains visible | A bulk action is in progress → UX Standards §3.4 |
| Action complete | Bar remains visible if items are still selected. Bar hides if the action removed the selected items (e.g. bulk delete) | Show toast confirming the action (→ toast.md) |
| Action error | Bar remains visible. Error shown via toast or inline alert | Selection is preserved so the user can retry |

---

## Interaction spec

### Mouse / pointer

- Selecting items in the collection (via checkboxes or click) causes the bar to appear
- Deselecting all items causes the bar to disappear
- Clicking an action button triggers the action on all selected items
- Clicking "Select all [count]" extends the selection beyond the current page
- Clicking "Deselect all" or the dismiss control clears the selection and hides the bar

### Keyboard

- The bar must be reachable via keyboard when it is visible. When the bar appears, it should not steal focus from the collection — the user may still be selecting items
- Action buttons within the bar are focusable via Tab and activated via Enter or Space
- Escape clears the selection and hides the bar (when focus is within the bar)

### Touch

- All action buttons and controls must meet the 44×44px minimum touch target → UX Standards §13.6
- On mobile, the bar must not obscure the collection items the user is trying to select. Position it at the top or bottom of the viewport with enough clearance

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Actions may overflow into a dropdown menu if there is not enough horizontal space. Selection count and primary action remain visible. The floating variant works well here as it avoids displacing the collection layout |
| `md` | Most actions visible inline. Dropdown overflow for 4+ actions |
| `lg`+ | All actions visible inline |

→ UX Standards §16 for breakpoint scale and responsive rules

---

## Accessibility

- **Role / semantics:** `role="toolbar"` with `aria-label="Bulk actions"` (or similar descriptive label). This communicates to screen readers that the element is a group of related action controls
- **Accessible name:** the selection count must be announced when the bar appears. Use an `aria-live="polite"` region for the count so screen readers announce changes as items are selected/deselected
- **Keyboard nav:** within the toolbar, use arrow keys to move between action buttons (toolbar keyboard pattern). Tab moves focus into and out of the toolbar as a single stop
- **Screen reader:** when a bulk action completes, announce the result (e.g. "3 customers archived") via the toast or a live region
- **WCAG criteria:** 4.1.2 (Name, Role, Value), 4.1.3 (Status Messages)

→ UX Standards §13 for baseline accessibility requirements

---

## Motion & animation

- **Enter:** the bar animates in when the first item is selected. Use a slide or fade, 150–200ms ease-out. The floating variant may slide up from below the viewport
- **Exit:** the bar animates out when the selection is cleared. 150ms ease-in
- **Count update:** the selection count updates immediately with no animation. Do not animate number changes — it distracts from the task
- **Reduced motion:** instant show/hide → UX Standards §13.9

→ UX Standards §14 for duration, easing, and reduced motion rules

---

## Common pitfalls

1. **Bar does not appear until a second item is selected** — the bar must appear on the first selection. A single-item selection is valid for bulk actions (the user may select more, or may choose to act on just one via the bar)
2. **Bar steals focus from the collection** — when the bar appears, focus must remain in the collection so the user can continue selecting. Do not auto-focus the bar
3. **Destructive bulk actions have no confirmation** — bulk delete, archive, or other destructive actions must show a confirmation that states exactly what will happen and how many items are affected (→ UX Standards §7.5)
4. **Selection state is lost on page change** — if the user changes the pagination page, their selection on previous pages must persist. Alternatively, clearly communicate that selection is per-page only
5. **No "Select all" across pages** — if the dataset spans multiple pages, selecting all checkboxes on the current page should prompt "Select all 286 customers" to extend the selection. Without this, users cannot act on the full dataset
6. **Action buttons do not enter loading state** — bulk operations may take time. The triggered button must show loading state, and other actions must be disabled to prevent conflicting operations (→ UX Standards §3.4)
7. **Bar overlaps content on mobile** — the bar must not cover the items the user is trying to select. Use a fixed position with enough offset, or use the inline variant that pushes content down

---

## Tailwind notes

Inline variant (replaces or overlays the table header):

- `flex items-center gap-3 px-4 py-2 bg-brand-50 border-b`
- Selection count: `text-sm font-medium`
- Actions: standard button sizes, grouped with `gap-2`

Floating variant:

- `fixed bottom-4 left-1/2 -translate-x-1/2 z-10 flex items-center gap-3 rounded-lg bg-white shadow-lg border px-4 py-3`
- Inside a sidebar shell, centre relative to the content region, not the viewport. Use `calc()` or position within the content region container rather than the body
- Safe area: add `mb-[env(safe-area-inset-bottom)]` on devices with home indicators (if using bottom positioning in a bottom nav shell)

Sticky variant:

- `sticky top-0 z-10 flex items-center gap-3 px-4 py-2 bg-white border-b shadow-sm`

→ UX Standards §10.2 for semantic colour tokens
