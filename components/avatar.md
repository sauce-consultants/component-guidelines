# Avatar

> A visual representation of a user or entity. Displays a profile image, initials, or a generic placeholder icon.

---

## When to use
- Identifying users in lists, comments, conversations, and headers
- Representing teams, organisations, or entities alongside their name
- User menu triggers in the header

## When not to use
- Decorative images unrelated to identity — use a standard image
- Logo display — use a dedicated logo component or image

---

## Anatomy

1. **Container** — circular (or rounded square) frame. Required
2. **Image** — user's profile photo. Primary content when available
3. **Initials** — first letter(s) of the user's name. Fallback when no image is available
4. **Placeholder icon** — generic person icon. Fallback when neither image nor initials are available
5. **Status indicator** — small dot overlaid on the avatar (online, away, busy). Optional
6. **Badge** — notification count overlaid on the avatar. Optional

---

## Variants

### Image
- **Purpose:** user has uploaded a profile photo
- **Visual distinction:** photo fills the circular container

### Initials
- **Purpose:** no photo available but name is known
- **Visual distinction:** coloured background with initials centred. Background colour may be deterministic from the user's name for consistency

### Placeholder
- **Purpose:** no photo or name available
- **Visual distinction:** generic icon (person silhouette) on neutral background

### Group / stack
- **Purpose:** showing multiple users (e.g. assignees, participants)
- **Visual distinction:** overlapping avatars with a "+N" overflow indicator

---

## Sizes

| Size | Dimension | Use case |
|---|---|---|
| `xs` | 24px | Inline mentions, compact lists |
| `sm` | 32px | Table cells, comment threads |
| `md` | 40px | Standard lists, cards |
| `lg` | 48–56px | Profile headers, user detail |
| `xl` | 64–80px | Profile pages, settings |

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Image, initials, or placeholder | |
| Loading | Skeleton circle matching avatar size | |
| Error (image failed) | Falls back to initials, then placeholder | Handle image load failure gracefully |
| With status | Status dot overlaid bottom-right | |

---

## Interaction spec

Avatars are typically non-interactive display elements. When interactive:
- **As a link:** wrapping in an `<a>` to navigate to a user profile. Hover shows pointer cursor
- **As a menu trigger:** clicking opens user menu (header use case)
- **In a group:** clicking the "+N" overflow may open a full list

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| All | Avatar sizes are fixed and do not change across breakpoints. Choose the appropriate size for the context |

Avatar groups may show fewer avatars on mobile (e.g. 3 + overflow instead of 5 + overflow).

---

## Accessibility

- **Image avatar:** `alt` text with the user's name. If the name is visible adjacent to the avatar, use `alt=""` (decorative) → UX Standards §13.4
- **Initials / placeholder:** use `aria-label` with the user's name, or `aria-hidden="true"` if the name is displayed as adjacent text
- **Status indicator:** pair with text or `aria-label` — "Online", "Away". Colour alone is not sufficient → UX Standards §10.4
- **Interactive avatars:** follow Button or Link accessibility patterns
- **WCAG:** 1.1.1, 1.4.1

---

## Motion & animation

- **Image load:** fade in from skeleton, 150ms ease-out
- **Status change:** dot colour transition, 150ms
- **Reduced motion:** instant transitions

---

## Common pitfalls

1. **No fallback for missing images** — if the image URL fails or is absent, the avatar must degrade to initials or a placeholder. A broken image icon is unacceptable
2. **Inconsistent sizes** — pick from the defined size scale. Arbitrary sizes (37px, 43px) create visual inconsistency
3. **Initials background colours that change** — if using name-derived colours, the algorithm must be deterministic. The same user must always get the same colour
4. **Status dot colour without text** — a green dot means nothing without context. Pair with a label or tooltip → UX Standards §10.4
5. **Non-circular crop on rectangular images** — use `object-cover` and `overflow-hidden` to ensure photos fill the circle without distortion

---

## Tailwind notes

Container:
- `relative inline-flex items-center justify-center rounded-full overflow-hidden bg-gray-200`
- Sizes: `h-6 w-6` (xs), `h-8 w-8` (sm), `h-10 w-10` (md), `h-12 w-12` (lg), `h-16 w-16` (xl)

Image:
- `h-full w-full object-cover`

Initials:
- `text-sm font-medium text-white` (size adjusts per avatar size)

Status dot:
- `absolute bottom-0 right-0 h-3 w-3 rounded-full border-2 border-white`
- Online: `bg-[var(--color-status-success)]`

Group stack:
- `flex -space-x-2` with `ring-2 ring-white` on each avatar for the overlap border
- Overflow: `h-10 w-10 rounded-full bg-gray-100 text-sm font-medium flex items-center justify-center`
