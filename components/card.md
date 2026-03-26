# Card

> A contained content block that groups related information into a single visual unit. Used for previews, summaries, and browsable collections.

---

## When to use
- Displaying items in a browsable collection (products, articles, users, projects)
- Summarising an entity with a mix of text, images, and actions
- Dashboard widgets or content modules
- As the mobile-responsive alternative to table rows

## When not to use
- Structured tabular data where comparison across rows matters — use Table
- Single-value display — use a Badge or inline text
- Full-page content — cards are for contained, scannable units

---

## Anatomy

1. **Container** — bordered or elevated surface. Required
2. **Header** — title, optional subtitle, optional icon or avatar. Optional
3. **Media** — image, illustration, or video at the top. Optional
4. **Body** — main content: text, key-value pairs, description. Required
5. **Footer** — actions, metadata, or secondary information. Optional
6. **Badge / status** — indicator overlaid or positioned within the card. Optional

---

## Variants

### Basic
- **Purpose:** simple content container
- **Visual distinction:** border or subtle shadow, no header/footer separation

### With media
- **Purpose:** content that leads with a visual (product images, article thumbnails)
- **Visual distinction:** image spans the top of the card, content below

### Interactive / clickable
- **Purpose:** the entire card is a link to a detail view
- **Visual distinction:** hover state on the whole card (elevation change, border highlight). Cursor pointer

### Horizontal
- **Purpose:** list-like layout with media on the side
- **Visual distinction:** image on the left, content on the right. Useful in narrower viewports

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Resting appearance with border or shadow | |
| Hover (clickable) | Slight elevation or border colour change | Only for interactive cards |
| Focus (clickable) | Focus ring around the card | → UX Standards §13.5 |
| Selected | Highlighted border or checkmark overlay | For selectable card grids |
| Loading | Skeleton card matching the card's layout | → UX Standards §3.2 |

---

## Interaction spec

### Mouse / pointer
- If the card is clickable, the entire surface is the click target
- Action buttons within the card (e.g. bookmark, share) should stop propagation so they don't trigger the card's link

### Keyboard
- Clickable cards: focusable via Tab, activated via Enter
- Action buttons within cards: independently focusable
- Use a single `<a>` wrapping the primary content for clickable cards, not an onclick on a `<div>`

### Touch
- Clickable cards: the entire card surface is the tap target
- Ensure action buttons within cards have adequate spacing to avoid accidental card navigation

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs` | Single-column card stack. Horizontal cards may revert to vertical |
| `sm` | Two-column card grid |
| `md`–`lg` | Three or four-column grid depending on content width needs |
| `xl`+ | Constrain max columns or card width to avoid overly wide cards |

Cards in a grid should be equal height within each row. Use CSS grid with `auto-rows` or flexbox with stretch alignment.

---

## Accessibility

- **Clickable cards:** use an `<a>` or `<button>` as the primary interactive element. Avoid wrapping the entire card in an anchor if it contains other interactive elements — instead, use a pseudo-element stretch technique to make the primary link cover the card area
- **Heading:** card titles should use an appropriate heading level to maintain document outline
- **Images:** media images need descriptive `alt` text, or `alt=""` if purely decorative → UX Standards §13.4
- **WCAG:** 4.1.2, 2.4.4 (Link Purpose), 1.1.1

---

## Motion & animation

- **Hover (clickable):** elevation or shadow transition, 150ms ease-out
- **Card grid loading:** staggered skeleton fade-in (optional)
- **Reduced motion:** no hover elevation change → UX Standards §13.9

---

## Common pitfalls

1. **Entire card wrapped in `<a>` with buttons inside** — nested interactive elements are invalid HTML and break accessibility. Use the pseudo-element stretch technique on the primary link instead
2. **Inconsistent card heights in a grid** — cards in a row must be the same height. Use CSS grid or flex stretch
3. **No loading skeleton** — a grid of empty rectangles while cards load looks broken. Use skeleton cards → UX Standards §3.2
4. **Cards without clear hierarchy** — if everything is a card, nothing stands out. Vary card sizes or highlight key cards
5. **Click target ambiguity** — if a card is clickable and has action buttons, the user must know what clicking the card body does vs. clicking a button

---

## Tailwind notes

Container:
- `rounded-lg border bg-white shadow-sm overflow-hidden`
- Clickable: add `hover:shadow-md transition-shadow cursor-pointer`

Layout:
- Grid: `grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4`
- Equal heights: CSS grid handles this automatically

Internal:
- Header: `px-4 pt-4 pb-2`
- Body: `px-4 py-2`
- Footer: `px-4 pt-2 pb-4 flex items-center justify-between`
- Media: `w-full h-48 object-cover` (or aspect ratio utilities)
