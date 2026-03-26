# Command Palette

> A keyboard-driven search and action launcher that overlays the interface. Allows power users to find content, navigate, and execute actions without leaving the keyboard.

---

## When to use
- Applications with many pages, features, or actions where keyboard shortcuts add significant value
- Developer tools, dashboards, admin panels — any power-user-oriented product → UX Standards §1.7
- As a universal search + action hub

## When not to use
- Simple applications with few pages — the overhead isn't justified
- Mobile-primary products where keyboard isn't the primary input method
- As a replacement for visible navigation — it supplements navigation, doesn't replace it

---

## Anatomy

1. **Backdrop** — semi-transparent overlay. Required
2. **Input field** — search/filter input at the top. Required
3. **Results list** — filtered list of commands, pages, actions, and content. Required
4. **Result item** — individual result with icon, label, and optional metadata (shortcut key, category). Required
5. **Group headers** — category dividers in the results list. Optional
6. **Footer** — keyboard hints (↑↓ to navigate, Enter to select, Esc to close). Optional but recommended

---

## Variants

### Search-focused
- **Purpose:** find and navigate to content
- **Visual distinction:** results are pages, records, or content items

### Action-focused
- **Purpose:** execute commands (toggle dark mode, create new item, open settings)
- **Visual distinction:** results are actions with associated keyboard shortcuts

### Nested / multi-level
- **Purpose:** drilling into categories (e.g. type ">" to show actions, "#" to search tags)
- **Visual distinction:** prefix characters change the result set. Breadcrumb trail may show current depth

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Closed | Not rendered | |
| Open (empty) | Input focused, recent items or suggested actions shown | |
| Filtering | Results update as user types | |
| No results | "No results for..." message | |
| Loading | Spinner in the results area for async search | |

---

## Interaction spec

### Keyboard (primary interaction method)
- `Cmd+K` / `Ctrl+K` to open (industry standard)
- `ArrowUp` / `ArrowDown` to navigate results
- `Enter` to execute the highlighted result
- `Escape` to close
- Typing filters the list immediately
- `Backspace` on empty input may go back to parent level (nested variant)

### Mouse / pointer
- Click an item to execute it
- Click backdrop to close

### Touch
- Tap to open (usually via a search icon in the header)
- Tap items to execute
- Less relevant on touch-primary devices — the value is primarily keyboard-driven

### Filtering
- Filter as the user types, with no debounce for local results
- Debounce 200–300ms for async results
- Support fuzzy matching for better discoverability

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Full-width, positioned near the top of the screen. May not be as useful on mobile |
| `md`+ | Centred dialog, ~560px wide, top-third of the screen |

---

## Accessibility

- **Role / semantics:** `role="combobox"` on the input, `role="listbox"` on the results list, `role="option"` on items
- **Label:** `aria-label="Command palette"` or similar on the dialog
- **Active item:** `aria-activedescendant` on the input tracking the highlighted result
- **Focus trap:** focus stays within the palette while open
- **Live region:** announce result count changes: "5 results" / "No results found"
- **WCAG:** 4.1.2, 2.1.1, 2.4.3

---

## Motion & animation

- **Open:** backdrop fades in. Dialog fades in + scales from 0.95, 150ms ease-out
- **Close:** reverse, 100ms ease-in
- **Result filtering:** instant — do not animate list changes during typing
- **Reduced motion:** instant show/hide → UX Standards §13.9

---

## Common pitfalls

1. **No keyboard shortcut to open** — the whole point is keyboard speed. `Cmd+K` / `Ctrl+K` is expected
2. **Slow filtering** — filtering must be instant for local results. Any perceptible lag kills the value proposition
3. **No recent items or suggestions** — an empty palette on open forces the user to know what they're looking for. Show recent items
4. **Missing keyboard navigation hints** — new users need to know ↑↓ navigates and Enter selects. Show hints in the footer
5. **Conflicting with browser shortcuts** — `Cmd+K` may focus the browser URL bar. Test in your deployment environment
6. **No fuzzy matching** — exact matching only is too strict. "settngs" should still find "Settings"

---

## Tailwind notes

Backdrop:
- `fixed inset-0 z-50 bg-black/50`

Dialog:
- `fixed top-[15%] left-1/2 -translate-x-1/2 z-50 w-full max-w-lg rounded-xl bg-white shadow-2xl overflow-hidden`

Input:
- `w-full border-b px-4 py-3 text-base outline-none`
- Leading search icon: `absolute left-4 text-gray-400`

Results list:
- `max-h-80 overflow-y-auto py-2`

Result item:
- `flex items-center gap-3 px-4 py-2.5 cursor-pointer`
- Highlighted: `bg-gray-100`
- Shortcut badge: `ml-auto text-xs text-gray-400 bg-gray-100 rounded px-1.5 py-0.5 font-mono`

Group header:
- `px-4 py-1.5 text-xs font-medium text-gray-400 uppercase tracking-wider`

Footer:
- `border-t px-4 py-2 flex items-center gap-4 text-xs text-gray-400`
