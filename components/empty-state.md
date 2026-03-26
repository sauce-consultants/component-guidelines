# Empty State

> Placeholder content displayed when a view has no data to show. Explains why the view is empty and guides the user toward populating it.

---

## When to use
- First-time use of a feature before data exists
- Search or filter results returning nothing
- Lists, tables, or dashboards with no records
- After deletion has cleared a view

## When not to use
- Error states — use an Error screen or inline error → UX Standards §4
- Loading states — use Skeleton Loader → UX Standards §3.2

→ UX Standards §5 for empty state requirements. §6.3 for distinguishing "no results" from "empty dataset".

---

## Anatomy

1. **Container** — centred within the content area. Required
2. **Illustration or icon** — a relevant visual. Optional but recommended
3. **Title** — brief statement explaining the empty state. Required
4. **Description** — 1–2 sentences of context and guidance. Required
5. **Action button** — primary action to populate the view (e.g. "Create your first project"). Optional but recommended where applicable

---

## Variants

### First use
- **Purpose:** the user hasn't created any data yet
- **Messaging:** encouraging, instructional. "No projects yet. Create your first one to get started."

### No search results
- **Purpose:** a search or filter returned nothing → UX Standards §6.3
- **Messaging:** acknowledge the search, suggest adjustments. "No results for 'xyz'. Try different keywords or clear your filters."
- **Actions:** "Clear filters" button

### Cleared / deleted
- **Purpose:** content was removed
- **Messaging:** neutral confirmation. "All items archived. Create new items or view the archive."

---

## States

Empty states are themselves a state — they do not have sub-states. They replace the content area entirely when triggered.

---

## Interaction spec

### Action buttons
- Follow standard Button interaction patterns
- "Create first [item]" should use primary button styling
- "Clear filters" should use secondary or link-style

### Keyboard
- Action buttons focusable and activatable via Enter/Space

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Illustration may shrink or hide. Text and action stack vertically, centred |
| `md`+ | Full illustration visible. Content centred in the available space |

---

## Accessibility

- **Role / semantics:** standard content — no special ARIA needed. The heading provides structure
- **Illustration:** decorative illustrations use `alt=""` or `aria-hidden="true"`. Meaningful illustrations need descriptive alt text → UX Standards §13.4
- **Live region:** if the empty state appears after a filter action, announce it via `aria-live="polite"`: "No results found"
- **WCAG:** 1.1.1, 1.3.1

---

## Motion & animation

- **Entrance:** fade in, 200ms ease-out. Illustration may have a subtle entrance animation
- **Reduced motion:** instant render → UX Standards §13.9

---

## Common pitfalls

1. **Generic "Nothing here" message** — every empty state must explain the specific reason in context → UX Standards §5
2. **No action** — if the user can do something to populate the view, give them a button. Don't leave them stuck
3. **Confusing "no results" with "empty dataset"** — these need different messages and different recovery actions → UX Standards §6.3
4. **Apologetic tone** — empty states are not errors. Keep the tone neutral or constructive, not apologetic → UX Standards §5
5. **Empty state not tested** — empty states are often forgotten in development. They're part of the design and must be built

---

## Tailwind notes

Container:
- `flex flex-col items-center justify-center text-center py-16 px-4`

Illustration:
- `h-32 w-32 mb-6 text-gray-300` (or an SVG illustration)

Title:
- `text-lg font-semibold text-gray-900 mb-2`

Description:
- `text-sm text-gray-500 max-w-md mb-6`

Action:
- Standard primary Button pattern (see button.md)
