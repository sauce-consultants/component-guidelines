# Timeline

> A vertical sequence of events displayed in chronological order. Used for activity logs, history, status tracking, and audit trails.

---

## When to use
- Activity feeds showing what happened and when
- Order or delivery tracking
- Audit logs and change history
- Project milestone tracking

## When not to use
- Scheduling or calendar views — use a Calendar component
- Comparing parallel timelines — use a Gantt chart or specialised visualisation
- Simple lists of items without temporal significance — use a plain list

---

## Anatomy

1. **Container** — vertical wrapper. Required
2. **Timeline line** — vertical line connecting events. Required
3. **Event node** — dot or icon on the line marking each event. Required
4. **Event content** — title, description, timestamp. Required
5. **Timestamp** — when the event occurred. Required
6. **Icon** — category or status icon replacing the default dot. Optional
7. **Actor** — who triggered the event (avatar + name). Optional

---

## Variants

### Simple
- **Purpose:** basic event list with timestamps
- **Visual distinction:** dots on a line, content to the right

### With icons
- **Purpose:** events categorised by type (created, updated, commented, deployed)
- **Visual distinction:** coloured icons replace dots, providing at-a-glance categorisation

### Grouped by date
- **Purpose:** long timelines with date separators
- **Visual distinction:** date headers break the timeline into day groups

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Events listed chronologically (newest first or oldest first, depending on context) | |
| Loading | Skeleton events matching the timeline layout | → UX Standards §3.2 |
| Empty | Empty state message: "No activity yet" | → UX Standards §5 |
| Expanding | "Load more" or infinite scroll for long timelines | |

---

## Interaction spec

### Mouse / pointer
- Timeline is primarily read-only
- Event items may be clickable to navigate to a detail view
- "Load more" button at the end to fetch older events

### Keyboard
- If events are clickable, they should be focusable links
- "Load more" button focusable and activated via Enter

### Touch
- Standard scrolling. Clickable events are tap targets

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| All | Timelines are inherently mobile-friendly as vertical layouts. Content width may narrow at smaller breakpoints |

On wide screens, content should be constrained to a readable width (max-w-2xl or similar) to avoid excessively wide event descriptions.

---

## Accessibility

- **Role / semantics:** use an `<ol>` or `<ul>` with each event as a `<li>`. Ordered list if chronological order is meaningful
- **Timestamps:** use `<time datetime="...">` for machine-readable dates
- **Icons:** decorative icons get `aria-hidden="true"`. If the icon conveys meaning (e.g. "error" event), pair with text or `aria-label`
- **WCAG:** 1.3.1, 1.3.2

---

## Motion & animation

- **New event added:** fade in + slide down from the top, 200ms ease-out
- **Load more:** new events fade in sequentially (staggered 50ms)
- **Reduced motion:** instant render → UX Standards §13.9

---

## Common pitfalls

1. **No timestamps** — events without times lose their temporal context, which is the whole point of a timeline
2. **Unclear chronological order** — state whether newest is first or last and be consistent
3. **Too much content per event** — keep event descriptions concise. Link to detail views for full information
4. **Icon colour as only differentiator** — pair coloured icons with text labels or category names → UX Standards §10.4
5. **No load more / pagination** — timelines can grow indefinitely. Don't load hundreds of events at once

---

## Tailwind notes

Container:
- `relative`

Line:
- `absolute left-4 top-0 bottom-0 w-0.5 bg-gray-200` (positioned behind the nodes)

Event:
- `relative flex gap-4 pb-8`

Node:
- `relative z-10 flex h-8 w-8 items-center justify-center rounded-full bg-white border-2 border-gray-200`
- With icon: `bg-blue-50 border-blue-200` (adjust per event type)

Content:
- `flex-1 pt-0.5`
- Title: `text-sm font-medium text-gray-900`
- Description: `text-sm text-gray-500 mt-1`
- Timestamp: `text-xs text-gray-400 mt-1`
