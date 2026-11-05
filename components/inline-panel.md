# Inline Panel

> A settings or filter panel that opens in the page flow, between the page header (or filter bar) and the content, and pushes the content down. Used for View settings on the yard board and the Site filter on Core bookings.

---

## How it differs from a Slide Panel / Drawer

→ slide-panel.md is an overlay anchored to a screen edge. An Inline Panel is not.

| | Inline Panel | Slide Panel / Drawer |
|---|---|---|
| Position | In the page flow, under the header | Overlays the page from an edge |
| Content below | Pushed down, still visible and usable | Covered (modal) or beside (persistent) |
| Backdrop / focus trap | None | Modal variants have both |
| Typical content | A few settings or one filter, grouped in columns | A detail view or a form |
| Close | The same button that opened it | Close button, Escape, backdrop |

Use an Inline Panel when the user changes settings and watches the content respond. Use a Slide Panel when the user is working on a separate record or form.

---

## When to use
- A handful of view settings or filters (switches, a single-select list) that change what the page below shows
- The user needs to see the effect straight away, so the content must stay visible

## When not to use
- Detail views, forms or anything long: use a Slide Panel or a page
- Destructive or blocking choices: use a Modal
- A single option: use a Select or Dropdown in the filter bar

---

## Anatomy

1. **Toggle button** — owned by the page. Required. A secondary Button with `aria-expanded` and `aria-controls` set to the panel id
2. **Panel** — a Card in the page flow, the same background as the cards around and beneath it (gray-900 on the yard board's gray-950 page; gray-950, the default Card, above the bookings cards). Required. The wrapper's gutters match what is underneath it: `px-4 pt-4` on the page background (yard board), or the layout's own gutters and background (`bg-gray-800 px-2 pt-2 lg:px-6 lg:pt-6` above a Main + aside layout) so the panel's edges line up with the cards below and no darker band shows behind it
3. **Column** — a title, a description and the controls. One or more. Required
4. **Controls** — switch rows or a radio group, with a Clear button at the bottom right of the panel (an end-aligned row across all columns). Clear calls one reset for the whole form, so it clears every control the panel holds, not just one. It is a standard secondary Button at the default size, like Cancel in a form. Rows are a Field with a Radio or Switch and a Label, centred on the same row (items-center), never the RadioOption block, whose label sits high against the dot

---

## States

| State | Visual treatment |
|---|---|
| Closed | Panel not rendered. Toggle is the secondary Button |
| Open | Panel rendered, content pushed down. The toggle does not change look; `aria-expanded` says it is open |
| Filter active | The toggle is unchanged: the standard secondary Button, text "Filter", in every state (it is not a primary action). The filter is shown as a dismissible info Flag under the card title of the content it filters, in a `role="status"` region so applying and removing it is announced. It stays while the panel is closed and across tab and date changes → UX Standards §6.1, §9.8. An empty result reminds the user the filter is on and offers Clear |
| Unavailable | Toggle is a disabled Button. Do not hide it |

---

## Props that fit the panel to its surroundings

- `className` on the wrapper: gutters and background of whatever sits beneath the panel. Default `px-4 pt-4` on the page background, which is what the yard board uses. Bookings passes `bg-gray-800 px-2 pt-2 lg:px-6 lg:pt-6` so the panel lines up with the Main + aside cards.
- `cardClassName` on the card: the card colour. Default gray-900, which the yard board keeps. Bookings passes `bg-gray-950` to match the list and menu cards.
- `FilterBar`'s `filter.onClear`: one reset for every control in the panel. The Clear button calls it; use it whenever the panel holds more than one control.

Only pass these when the panel sits above content with its own surface. Elsewhere leave the defaults.

---

## Responsive behaviour

The columns sit in a grid: 1 column on phones, 2 from `md`, 4 from `xl`. The panel works the same on every width.

---

## Accessibility

- The toggle has `aria-expanded` and `aria-controls` pointing at the panel id
- Focus stays on the toggle. There is no trap and no auto-move of focus
- A single-select filter is a radio group with an accessible name, and a Clear button
- Each column title is a heading so the panel can be navigated by headings
- WCAG: 4.1.2, 2.4.3, 1.4.3

---

## Common pitfalls
- Using it for long content: it pushes the page down, so keep it to a few short columns
- Hiding the active state when the panel is closed: a filter that is applied but not visible hides rows from the user. Show the info Flag in the content card
- Adding a backdrop or focus trap: that makes it a Slide Panel
