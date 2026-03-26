# Breadcrumbs

> A horizontal trail showing the user's current location within the application hierarchy. Provides quick navigation back to parent pages.

---

## When to use
- Applications with 3+ levels of navigation depth
- When users arrive at pages via multiple paths and need orientation
- E-commerce (category > subcategory > product), admin panels, documentation

## When not to use
- Flat applications with only one level of navigation
- When the hierarchy is already obvious from the sidebar or page structure
- As a replacement for the browser's back button — breadcrumbs show hierarchy, not history

---

## Anatomy

1. **Container** — horizontal row of items. Required
2. **Breadcrumb items** — clickable links representing each level in the hierarchy. Required
3. **Separators** — visual dividers between items (typically "/" or ">"). Required
4. **Current page** — the last item, representing the current page. Not a link. Required

---

## Variants

### Standard
- **Purpose:** simple path display
- **Visual distinction:** text links separated by a divider, last item as plain text

### With overflow
- **Purpose:** deep hierarchies where the full path is too long
- **Visual distinction:** middle items collapse into an ellipsis ("...") or a dropdown. First and last items always visible

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Link item | Styled as a subtle link (underline on hover or muted colour) | |
| Link hover | Underline or colour change | |
| Current page | Plain text, no link, bolder or darker than parent links | |
| Truncated | Ellipsis item with dropdown revealing hidden levels | |

---

## Interaction spec

### Mouse / pointer
- Click any breadcrumb link to navigate to that level
- Click ellipsis to reveal hidden items in a dropdown

### Keyboard
- `Tab` moves through breadcrumb links
- `Enter` activates a link
- Ellipsis dropdown follows Dropdown Menu keyboard patterns

### Touch
- Each breadcrumb link must be a comfortable tap target. Use adequate padding between items

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Show only the parent link and current page (e.g. "← Back to [Parent]"). Full breadcrumbs are too wide |
| `md`+ | Full breadcrumb trail visible. Use overflow/ellipsis pattern for very deep hierarchies |

---

## Accessibility

- **Role / semantics:** wrap in `<nav aria-label="Breadcrumb">` containing an `<ol>`
- **Current page:** last item uses `aria-current="page"` and is not a link
- **Separators:** use CSS pseudo-elements or `aria-hidden="true"` on separator characters so they aren't read by screen readers
- **WCAG:** 2.4.8 (Location), 1.3.1

---

## Motion & animation

- No animation required. Breadcrumbs are static navigation aids
- Overflow dropdown follows standard Dropdown Menu animation patterns

---

## Common pitfalls

1. **Making the current page a link** — the last breadcrumb represents the page the user is on. It should not be clickable
2. **Separators read by screen readers** — if using text characters as separators, hide them from AT with `aria-hidden="true"`. CSS-generated separators are better
3. **No breadcrumbs on mobile** — on small screens, at least show a back link to the parent page. Users on mobile need orientation too
4. **Breadcrumb trail doesn't match the actual hierarchy** — breadcrumbs must reflect the page structure, not the user's navigation history
5. **Missing `<nav>` landmark** — without the nav element and aria-label, screen readers can't identify the breadcrumbs as a navigation region

---

## Tailwind notes

Container:
- `flex items-center gap-2 text-sm`

Link items:
- `text-gray-500 hover:text-gray-700 hover:underline`

Separator:
- Use `before:content-['/'] before:mx-2 before:text-gray-300` on items (except the first), or render separator elements with `aria-hidden="true"`

Current page:
- `text-gray-900 font-medium`

Use `<ol>` for semantic ordering. Each `<li>` contains either a link or the current page text.
