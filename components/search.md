# Search

> A composite pattern combining a text input with a results display, filtering, and keyboard navigation. Enables users to find content across the application.

---

## When to use
- Global search across the application's content
- Local search within a specific section (table, list, directory)
- Search-as-you-type with instant results
- Search pages with full results, filtering, and pagination

## When not to use
- Filtering a visible list with predefined options — use Filter chips or Select
- Navigating and executing commands — use Command Palette (though they can overlap)
- Selecting from a known set — use Combobox

---

## Anatomy

1. **Search input** — text field with search icon and optional clear button. Required
2. **Results panel** — dropdown or inline area showing matching results. Required
3. **Result item** — individual result with title, optional description, category, or highlight. Required
4. **Result grouping** — results organised by type or category. Optional
5. **No results message** — shown when the search returns nothing. Required → UX Standards §6.3
6. **Loading indicator** — spinner or skeleton while results are being fetched. Conditional
7. **Recent searches** — previously searched terms shown before typing. Optional
8. **Filters** — refinement controls (category, date, type) alongside results. Optional
9. **Pagination** — for full search pages with many results. Conditional

---

## Variants

### Inline / autocomplete
- **Purpose:** quick search with a dropdown of results appearing as the user types
- **Visual distinction:** results appear in a floating panel below the input. Selecting a result navigates or fills the input

### Full-page search
- **Purpose:** dedicated search experience with filters, sorting, and paginated results
- **Visual distinction:** input at the top, results below in a structured list or grid. Filter sidebar optional

### Scoped search
- **Purpose:** search within a specific context (e.g. within a table, within a project)
- **Visual distinction:** may appear as an input above a table that filters rows, or within a specific panel

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Empty (idle) | Input with placeholder. May show recent searches on focus | |
| Typing | Results filter in real time (debounced for async) | |
| Loading | Spinner in the results area | → UX Standards §3.1 |
| Results | Matching items displayed | |
| No results | Clear message explaining no matches, with suggestions | → UX Standards §6.3, §5 |
| Error | Error message with retry | → UX Standards §4.4 |

---

## Interaction spec

### Mouse / pointer
- Click input to focus. Recent searches or suggestions may appear
- Click a result to navigate to it or select it
- Click clear button to reset the search

### Keyboard
- `ArrowDown` moves focus from input into the results list
- `ArrowUp` / `ArrowDown` navigates results
- `Enter` on a result navigates or selects it
- `Enter` on the input may submit to a full search results page
- `Escape` clears the input or closes the results panel
- Global shortcut: `/` or `Cmd+K` to focus the search input → UX Standards §1.7

### Touch
- Tap to focus and show keyboard
- Results must be tappable with adequate spacing
- Clear button meets 44×44px target → UX Standards §13.6

### Debouncing
- Local filtering: no debounce needed (instant)
- Async search: debounce 200–300ms to avoid excessive API calls
- Show the previous result set while new results are loading → UX Standards §3.3

### Query highlighting
- Highlight the matching portion of each result to show why it matched. Use bold or a background highlight on the matched text

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Search input may be hidden behind a search icon in the header. Tapping opens a full-width search view. Inline results take full width |
| `md`+ | Inline search in the header or content area. Results panel anchored below the input |

For full-page search:
| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Filters hidden behind a "Filters" button or bottom sheet. Results single-column |
| `md`+ | Filter sidebar visible alongside results. Results may be multi-column if using cards |

---

## Accessibility

- **Role / semantics:** input: `role="combobox"` with `aria-expanded`, `aria-autocomplete="list"`. Results: `role="listbox"` with `role="option"` items. Same pattern as Combobox
- **Accessible name:** `aria-label="Search"` or a visible `<label>`
- **Active result:** `aria-activedescendant` on the input
- **Result count:** announce via `aria-live="polite"`: "8 results found" or "No results"
- **Clear button:** `aria-label="Clear search"`
- **WCAG:** 4.1.2, 2.1.1, 1.3.1, 4.1.3

---

## Motion & animation

- **Results panel open:** fade in + translate, 150ms ease-out (same as Combobox dropdown)
- **Result updates during typing:** instant — do not animate individual results in and out during filtering
- **Clear transition:** input clears immediately, results panel may fade to recent/suggestions
- **Reduced motion:** instant show/hide → UX Standards §14

---

## Common pitfalls

1. **No debounce on async search** — fires a request per keystroke, causing performance problems and flicker
2. **Blank results area while loading** — keep the previous results visible until new ones arrive → UX Standards §3.3
3. **No "no results" state** — an empty results area with no message looks broken → UX Standards §6.3
4. **No clear button** — the user needs a one-click way to reset the search
5. **Search icon that doesn't do anything** — the leading search icon should not be an interactive button competing with the input. It's decoration. The input itself is the interactive element
6. **No keyboard shortcut to focus search** — power users expect `/` or `Cmd+K` to jump to search
7. **Results disappear between keystrokes** — flashing a loading spinner on every character change is jarring. Debounce and keep previous results visible
8. **No query highlighting in results** — without highlighting, the user can't see why a result matched
9. **Full-page search with no filters** — for large datasets, search alone is insufficient. Provide refinement options

---

## Tailwind notes

Inline search input:
- `relative flex items-center`
- Input: `w-full rounded-md border pl-10 pr-10 py-2 text-base`
- Search icon: `absolute left-3 h-5 w-5 text-gray-400 pointer-events-none`
- Clear button: `absolute right-3` (icon button pattern)

Results panel:
- `absolute z-50 mt-1 w-full rounded-md border bg-white shadow-lg max-h-80 overflow-y-auto`

Result item:
- `flex items-center gap-3 px-4 py-2.5 cursor-pointer hover:bg-gray-50`
- Highlighted: `bg-gray-100`
- Match highlight: `<mark class="bg-yellow-100 rounded px-0.5">` or `font-semibold` on matched text

Recent searches:
- `px-4 py-2 text-sm text-gray-500` with clock icon

Full-page layout:
- Container: `max-w-5xl mx-auto`
- With filters: `grid grid-cols-1 md:grid-cols-[240px_1fr] gap-6`
- Results list: `flex flex-col divide-y`
