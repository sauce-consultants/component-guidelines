# Pagination

> Page-based navigation for datasets. Shows the user where they are in a set of results and lets them navigate between pages.

---

## When to use
- Data tables and lists where the user needs to track position in a large dataset → UX Standards §9.3
- Search results where total count and position matter
- Any paginated API response

## When not to use
- Feeds or browsable content where position tracking isn't important — use infinite/continuous scroll → UX Standards §9.3
- Fewer than one page of results — hide pagination entirely
- When the total result count is unknown and position is irrelevant

---

## Anatomy

1. **Previous button** — navigates to the previous page. Required
2. **Next button** — navigates to the next page. Required
3. **Page numbers** — clickable numbered links. Required (though can be simplified on mobile)
4. **Current page indicator** — highlighted page number. Required
5. **Ellipsis** — indicates skipped page numbers in long sequences. Conditional
6. **Result count** — "Showing 51–100 of 340". Recommended → UX Standards §9.3
7. **Page size selector** — dropdown to change results per page. Optional

---

## Variants

### Full
- **Purpose:** data tables and search results where precision matters
- **Visual distinction:** prev/next + numbered pages + result count

### Simple (prev/next only)
- **Purpose:** content where page numbers aren't meaningful (blog posts, articles)
- **Visual distinction:** just previous and next buttons with optional result count

### Compact
- **Purpose:** tight spaces, mobile
- **Visual distinction:** prev/next arrows + "Page 3 of 12" text. No individual page numbers

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Page numbers visible, current highlighted | |
| Hover | Background on hovered page number | |
| Current page | Strong background fill or bold text | Must not be a link |
| Disabled prev/next | Muted when on first/last page | Visible but non-interactive |

---

## Interaction spec

### Mouse / pointer
- Click a page number to navigate. Click prev/next to step through

### Keyboard
- `Tab` moves through pagination controls
- `Enter` activates the focused page link
- Previous and next buttons are focusable and activated via Enter

### Touch
- Page number buttons need adequate spacing for tap targets. May need to simplify to compact variant on small screens

### Behaviour
- Navigating to a new page should scroll the user to the top of the results area
- If results load asynchronously, show a loading state in the data area (skeleton or spinner), not on the pagination itself

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Compact variant: prev/next + "Page X of Y". Individual page numbers take too much space |
| `md`+ | Full numbered pagination. Show 5–7 page numbers with ellipsis for gaps |

---

## Accessibility

- **Role / semantics:** wrap in `<nav aria-label="Pagination">`
- **Current page:** `aria-current="page"` on the active page
- **Disabled:** `aria-disabled="true"` on prev/next when at the boundaries
- **Page links:** use `aria-label="Page 3"` (or `aria-label="Go to page 3"`) for clarity
- **WCAG:** 2.4.8, 4.1.2, 2.1.1

---

## Motion & animation

- No animation on the pagination control itself
- Results area may crossfade or show a loading skeleton when changing pages

---

## Common pitfalls

1. **Showing pagination when there's only one page** — hide the control entirely if total pages ≤ 1
2. **Current page is clickable** — the current page should not be a link; clicking it does nothing and confuses users
3. **No result count** — users need to know "Showing 51–100 of 340" to understand the dataset scope → UX Standards §9.3
4. **Page change doesn't scroll to top** — after navigating, the user should see the top of the new results, not be stuck at the bottom
5. **Missing ellipsis logic** — showing all 40 page numbers is unusable. Truncate with ellipsis and show first, last, and a window around the current page

---

## Tailwind notes

Container:
- `flex items-center justify-between` or `flex items-center justify-center gap-1`

Page numbers:
- `h-10 w-10 flex items-center justify-center rounded-md text-sm`
- Current: `bg-[var(--color-action-primary)] text-white font-medium`
- Others: `text-gray-600 hover:bg-gray-100`

Prev/Next:
- `flex items-center gap-1 px-3 py-2 text-sm rounded-md`
- Disabled: `opacity-50 pointer-events-none`

Result count:
- `text-sm text-gray-500`
