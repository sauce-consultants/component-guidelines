# Stat / KPI Card

> A focused display for a single key metric: a label, a prominent value, and optional trend or context. Used in dashboards and summary views.

---

## When to use
- Dashboard headers showing 3–6 key metrics at a glance
- Summary panels above data tables
- Report sections highlighting important numbers

## When not to use
- Displaying many metrics with relationships between them — use a chart
- Detailed breakdowns — use a Table or data visualisation
- Single inline values — just use text

---

## Anatomy

1. **Container** — card surface. Required
2. **Label** — what the metric represents (e.g. "Revenue", "Open tickets"). Required
3. **Value** — the primary number, formatted appropriately. Required
4. **Trend indicator** — arrow or icon showing direction (up/down) with percentage or absolute change. Optional
5. **Sparkline** — small inline chart showing the metric over time. Optional
6. **Comparison period** — text clarifying the trend (e.g. "vs. last month"). Optional
7. **Icon** — visual reinforcement of the metric's category. Optional

---

## Variants

### Simple
- **Purpose:** label and value only
- **Visual distinction:** minimal — just the number with context

### With trend
- **Purpose:** value with directional change indicator
- **Visual distinction:** up/down arrow with change amount, colour-coded (green for positive, red for negative — or reverse if "up" is bad, e.g. churn)

### With sparkline
- **Purpose:** value with a small time-series visualisation
- **Visual distinction:** tiny line chart beside or below the value

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Default | Label, value, optional trend | |
| Loading | Skeleton matching the card layout | → UX Standards §3.2 |
| Error | Error message replacing the value, with retry | → UX Standards §4.2 |
| No data | Dash or "—" with explanatory text | |

---

## Interaction spec

Stat cards are typically non-interactive display elements.
- **Clickable variant:** the card links to a detail view or filtered table. Hover state on the whole card
- Trend colours must be paired with icons (arrows) and text labels → UX Standards §10.4

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs` | Single column stack or 2-column grid |
| `sm`–`md` | 2–3 columns |
| `lg`+ | 3–6 columns in a row |

Cards should maintain equal height. Values should not wrap — use abbreviations (1.2k, 3.4M) if needed at smaller sizes.

---

## Accessibility

- **Structure:** label and value should read naturally as a pair. Use `aria-label` on the card: "Revenue: £142,000, up 12% vs. last month"
- **Trend colours:** always paired with arrow icon and text → UX Standards §10.4
- **Sparklines:** `aria-hidden="true"` if the trend is communicated via text. Otherwise, provide an `aria-label` summarising the trend
- **WCAG:** 1.4.1, 1.1.1

---

## Motion & animation

- **Value count-up:** optional animated number transition on load, 300ms. Use sparingly — it's satisfying but can feel gimmicky if overused
- **Trend arrow:** fade in, 150ms
- **Reduced motion:** instant value display, no count-up

---

## Common pitfalls

1. **Trend colour without icon or text** — a green number is meaningless without an up arrow and "+12%" label → UX Standards §10.4
2. **Ambiguous trend direction** — "up" is not always good (e.g. churn rate, error count). Make sure colour and language align with the metric's meaning
3. **No loading state** — skeleton cards while data loads. Empty cards with no explanation look broken → UX Standards §3.2
4. **Unformatted numbers** — large numbers need formatting: thousands separators, currency symbols, appropriate decimal places
5. **Too many stat cards** — more than 6 creates dashboard fatigue. Curate the most important metrics

---

## Tailwind notes

Container:
- `rounded-lg border bg-white p-4 sm:p-6`

Layout:
- Grid: `grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4`

Label:
- `text-sm font-medium text-gray-500`

Value:
- `text-2xl font-semibold text-gray-900 tabular-nums`

Trend:
- `inline-flex items-center gap-1 text-sm font-medium`
- Up (positive): `text-[var(--color-status-success)]`
- Down (negative): `text-[var(--color-status-error)]`
