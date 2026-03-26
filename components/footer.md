# Footer

> A page-level area at the bottom of the screen containing secondary navigation, legal links, branding, and metadata.

---

## When to use
- Public-facing websites and applications that need copyright notices, legal links, or secondary navigation
- Applications where support links, version info, or social links belong outside the main nav

## When not to use
- Full-screen application dashboards where every pixel is working space — footer adds unnecessary chrome
- Single-page tools or embedded widgets

---

## Anatomy

1. **Container** — full-width bar at the bottom of the page content. Required
2. **Brand / logo** — smaller version of the logo. Optional
3. **Navigation columns** — grouped links organised by category. Optional
4. **Legal text** — copyright notice, company name, year. Recommended
5. **Legal links** — privacy policy, terms of service, cookie policy. Recommended
6. **Social links** — icon links to social media profiles. Optional
7. **Language / region selector** — if the product is multilingual. Optional

---

## Variants

### Simple
- **Purpose:** minimal footer for applications
- **Visual distinction:** single row with copyright text and a few links

### Multi-column
- **Purpose:** marketing sites or products with extensive secondary navigation
- **Visual distinction:** columns of grouped links above a bottom bar with legal text

---

## States

Footers are largely static. Link hover states follow standard link conventions.

---

## Interaction spec

### Mouse / pointer
- Click links to navigate. Standard link behaviour

### Keyboard
- All links focusable via Tab. Standard link activation

### Touch
- Links must have adequate spacing for comfortable tapping

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Multi-column layouts stack to single column. Links stack vertically. Legal text at the bottom |
| `md`+ | Columns side-by-side. Legal bar spans full width below |

---

## Accessibility

- **Role / semantics:** `<footer>` element. If it contains navigation, wrap links in `<nav aria-label="Footer navigation">`
- **Social links:** icon-only social links need `aria-label` (e.g. "Twitter", "LinkedIn") → UX Standards §12.4
- **WCAG:** 2.4.1, 4.1.2

---

## Motion & animation

- None required. Footers are static

---

## Common pitfalls

1. **Footer overlapping content** — if using a sticky or fixed footer, ensure it doesn't cover page content. Most footers should be in the document flow, not fixed
2. **Social icons without labels** — icon-only links need `aria-label` for screen readers
3. **Outdated copyright year** — dynamically generate the year, don't hardcode it
4. **Duplicating primary navigation** — the footer is for secondary links. Don't repeat the entire header nav

---

## Tailwind notes

Container:
- `border-t bg-gray-50 px-4 py-8 lg:px-6 lg:py-12`

Multi-column:
- `grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8`

Legal bar:
- `border-t mt-8 pt-6 flex flex-col sm:flex-row items-center justify-between gap-4 text-sm text-gray-500`
