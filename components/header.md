# Header / Navbar

> The top-level navigation bar that persists across pages. Provides access to primary navigation, branding, search, and user-level actions.

---

## When to use
- Every application needs a primary navigation structure. The header is the most common pattern for this
- Use when primary navigation is flat or shallow (up to ~7 top-level items)

## When not to use
- Deep navigation with many nested levels — combine with a Sidebar
- If the app is a single-screen tool with no navigation — omit entirely

---

## Anatomy

1. **Container** — full-width bar at the top of the viewport. Required
2. **Logo / brand mark** — application identity, typically linking to the home page. Required
3. **Primary nav links** — top-level navigation items. Required (unless navigation is handled elsewhere)
4. **Search** — global search trigger or inline input. Optional
5. **Utility actions** — notifications, settings, help. Optional
6. **User menu** — avatar/name with dropdown for account actions, logout. Optional
7. **Mobile menu trigger** — hamburger icon that opens the mobile navigation. Required at mobile breakpoints → UX Standards §16.4

---

## Variants

### Horizontal nav
- **Purpose:** standard top bar with links visible at desktop sizes
- **Visual distinction:** logo left, nav links centre or left-aligned, utility/user right

### Minimal
- **Purpose:** apps where navigation is sparse or handled by a sidebar
- **Visual distinction:** logo and user menu only. May include a sidebar toggle

### With mega menu
- **Purpose:** complex sites with many sections and sub-sections
- **Visual distinction:** hovering/clicking a nav item opens a wide dropdown panel with grouped links

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Fixed or sticky at top | |
| Active page | Current nav link highlighted (underline, background, bold) | Must be visible → UX Standards §1.4 |
| Scrolled | May gain a shadow or reduce in height for a compact look | Optional enhancement |
| Mobile | Nav links hidden; hamburger icon visible | → UX Standards §16.4 |
| Mobile open | Slide-out drawer or full-screen overlay showing nav links | |

---

## Interaction spec

### Mouse / pointer
- Click nav links to navigate. Active link is visually indicated
- Hover on items with dropdowns reveals the dropdown (or click to toggle)

### Keyboard
- `Tab` moves through nav links, utility actions, and user menu
- `Enter` activates links. `Escape` closes open dropdowns
- Skip link ("Skip to main content") must be the first focusable element on the page

### Touch
- Mobile menu trigger taps to open/close
- Nav links in mobile menu are full-width tap targets

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Logo + hamburger trigger. Nav links in a drawer or overlay |
| `md` | May show abbreviated nav (fewer items, overflow into a "More" dropdown) |
| `lg`+ | Full horizontal nav visible. All primary links shown |

→ UX Standards §16.4 for navigation pattern adaptation across breakpoints.

---

## Accessibility

- **Role / semantics:** wrap in `<header>` and use `<nav aria-label="Main navigation">` for the link group
- **Skip link:** provide a "Skip to main content" link as the first focusable element. Visually hidden until focused
- **Current page:** use `aria-current="page"` on the active nav link
- **Mobile menu:** the hamburger button needs `aria-expanded`, `aria-controls`, and `aria-label="Open menu"`
- **Keyboard:** all interactive elements reachable via Tab. Dropdowns support arrow key navigation
- **WCAG:** 2.4.1 (Bypass Blocks via skip link), 2.4.8 (Location), 4.1.2

---

## Motion & animation

- **Mobile menu:** slide in from the side or fade from top, 200–300ms ease-out → UX Standards §14.2
- **Dropdown menus:** fade + translate, 150ms ease-out
- **Scrolled state transition:** height and shadow change, 200ms ease-in-out
- **Reduced motion:** instant state changes, no slide → UX Standards §13.9

---

## Common pitfalls

1. **No skip link** — keyboard and screen reader users must tab through every nav item on every page load. Always provide "Skip to main content"
2. **Mobile menu not focusable** — when the mobile menu opens, focus must move into it. When it closes, focus returns to the trigger
3. **Active page not indicated** — users must know where they are. Highlight the current nav item with more than just colour → UX Standards §10.4
4. **Hamburger icon without label** — the hamburger needs `aria-label="Open menu"` and ideally a visible "Menu" text label on mobile
5. **Fixed header blocking content** — if using `position: fixed` or `sticky`, add appropriate top padding/margin to the page content to prevent the header from covering it
6. **Too many nav items** — more than 7 top-level items creates cognitive overload. Group into dropdowns or use a sidebar

---

## Tailwind notes

Container:
- `fixed top-0 left-0 right-0 z-40 h-16 bg-white border-b`
- Or `sticky top-0` if fixed positioning creates layout complications
- Body offset: `pt-16` on the main content area

Layout:
- `flex items-center justify-between px-4 lg:px-6 h-full max-w-screen-2xl mx-auto`
- Nav links: `hidden lg:flex items-center gap-1`
- Mobile trigger: `lg:hidden`

Active link: `text-[var(--color-action-primary)] font-medium` or an underline/bottom-border treatment.
