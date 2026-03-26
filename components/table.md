# Table / Data Grid

> A structured data display using rows and columns. The primary component for displaying, sorting, filtering, and acting on collections of records.

---

## When to use
- Displaying structured data with multiple attributes per record
- When users need to compare values across rows
- Operations dashboards, admin panels, CRM records, inventory, any list of entities

## When not to use
- Fewer than 3 columns of data — use a simple list or Card layout
- Content that is primarily visual (products, images) — use Cards in a grid
- Mobile-first interfaces where tables will always be cramped — use Cards with key-value pairs

---

## Anatomy

1. **Table container** — wrapper handling overflow and scroll. Required
2. **Column headers** — labels for each column, with sort affordance where applicable. Required → UX Standards §9.1
3. **Rows** — individual data records. Required
4. **Cells** — data values within each row/column intersection. Required
5. **Row actions** — icon buttons or a three-dot menu per row. Optional → UX Standards §9.4
6. **Selection checkboxes** — for bulk selection. Optional → UX Standards §9.7
7. **Bulk action bar** — appears when rows are selected, showing bulk actions and selection count. Conditional
8. **Sort indicators** — arrow icons on sortable columns. Conditional → UX Standards §9.2
9. **Filter bar** — active filter chips above the table. Conditional → UX Standards §9.8
10. **Pagination** — below the table. Conditional (see pagination.md)
11. **Empty state** — when the table has no data or filters produce no results. Conditional → UX Standards §5

---

## Variants

### Basic table
- **Purpose:** read-only data display
- **Visual distinction:** clean rows and columns, no interactivity beyond sorting

### Interactive table
- **Purpose:** sortable, filterable, with row actions and selection
- **Visual distinction:** sort indicators, action menus, selection checkboxes, bulk bar

### Inline editable
- **Purpose:** spreadsheet-like direct editing within cells → UX Standards §9.6
- **Visual distinction:** cells show edit affordance on hover. Edit mode has a clear visual change

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Alternating row shading or borders for row separation | |
| Hover | Row background highlight | |
| Selected | Checkbox checked, row background tinted | → UX Standards §9.7 |
| Sorted | Active column header bold with direction arrow | → UX Standards §9.2 |
| Filtered | Filter chips visible, result count updated | → UX Standards §9.8 |
| Loading | Skeleton rows in place of data | → UX Standards §3.2 |
| Empty | Empty state message with guidance | → UX Standards §5 |
| No results (filtered) | Distinct from empty — mention active filters, offer clear-all | → UX Standards §6.3 |
| Inline editing | Cell in edit mode with input, save/cancel affordance | → UX Standards §9.6 |

---

## Interaction spec

### Mouse / pointer
- Click column headers to sort (toggle ascending/descending/none)
- Click row to navigate to detail view (if applicable) or select
- Click row action button/menu to access per-row actions
- Click checkbox to select/deselect. Click header checkbox for select all

### Keyboard
- `Tab` moves through interactive elements (sort headers, checkboxes, action buttons)
- `Enter` on a row may navigate to detail view
- `Space` toggles checkbox selection
- For inline editing: `Enter` to commit, `Escape` to cancel → UX Standards §9.6

### Touch
- Row actions via a revealed menu (swipe or tap three-dot icon)
- Selection via checkboxes. Long-press to select is an optional enhancement

### Sorting
- Default sort must be the most operationally useful order → UX Standards §9.2
- Allow multi-column sort only if the use case demands it. Keep it simple

### Filtering
- Active filters shown as chips above the table → UX Standards §9.8
- Clear individual filters and clear all must both be available → UX Standards §6.2
- Filtered empty state must differ from true empty state → UX Standards §6.3

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Options: (1) horizontal scroll in a bounded container, (2) card-based row layout, (3) column prioritisation — hide low-priority columns. Choose based on use case → UX Standards §16.5 |
| `md` | Show primary columns, hide secondary. Action menus accessible |
| `lg`+ | Full table visible. All columns shown |

Never force a full desktop table into a mobile viewport unchanged → UX Standards §16.5.

---

## Accessibility

- **Role / semantics:** use `<table>`, `<thead>`, `<tbody>`, `<tr>`, `<th>`, `<td>`. Do not build tables from `<div>` unless using ARIA grid roles
- **Column headers:** `<th scope="col">`. Units in headers, not cells → UX Standards §9.1
- **Sort:** `aria-sort="ascending"` / `"descending"` / `"none"` on sortable `<th>` elements
- **Selection:** select-all checkbox labelled "Select all rows". Row checkboxes labelled with the record identifier
- **Status indicators:** use colour and text label together → UX Standards §9.5
- **Inline editing:** announce edit mode entry/exit via live region
- **WCAG:** 1.3.1, 1.3.2, 4.1.2, 2.1.1

---

## Motion & animation

- **Sort:** rows reorder instantly. Optionally fade-crossfade, 150ms
- **Row add/remove:** fade in/out, 150ms. Avoid complex animations that delay operations
- **Bulk action bar:** slide in from top or bottom, 150ms ease-out
- **Reduced motion:** instant changes → UX Standards §13.9

---

## Common pitfalls

1. **Table from `<div>` elements without ARIA** — if you can't use `<table>`, you must implement `role="grid"`, `role="row"`, `role="columnheader"`, `role="gridcell"`. Prefer native `<table>`
2. **Units repeated in every cell** — put "£", "kg", "%" in the column header, not in each cell → UX Standards §9.1
3. **No empty state** — a table with zero rows and no message looks broken → UX Standards §5
4. **Colour-only status** — status columns must use text + colour, never colour alone → UX Standards §9.5
5. **No sort indication** — if a column is sorted, the user must see which one and in what direction → UX Standards §9.2
6. **Bulk actions visible with no selection** — bulk actions should only appear when ≥1 row is selected → UX Standards §9.7
7. **Horizontal scroll on the whole page** — if the table overflows, scroll within a bounded container, not the entire page → UX Standards §16.3
8. **Numeric columns left-aligned** — right-align numbers for easy comparison → UX Standards §9.1

---

## Tailwind notes

Container:
- `overflow-x-auto rounded-lg border`

Table:
- `min-w-full divide-y`

Header:
- `<th>`: `px-4 py-3 text-left text-xs font-medium uppercase tracking-wider text-gray-500`
- Sortable: `cursor-pointer select-none hover:text-gray-700`

Cells:
- `px-4 py-3 text-sm whitespace-nowrap`
- Numeric: `text-right tabular-nums`

Row hover:
- `hover:bg-gray-50`

Selected row:
- `bg-[var(--color-action-primary)]/5`

Bulk bar:
- `sticky top-0 z-10 flex items-center gap-4 bg-gray-50 px-4 py-2 border-b`

Responsive card layout (mobile):
- Replace `<table>` with a card list: `flex flex-col gap-3` with each row as a card showing key-value pairs
