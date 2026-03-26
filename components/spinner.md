# Spinner

> A small, indeterminate loading indicator for localised areas. Communicates that an operation is in progress when a skeleton layout is not applicable.

---

## When to use
- Button loading states (alongside or replacing the label) → UX Standards §3.4
- Small content regions loading independently
- Inline actions where a skeleton doesn't make sense (e.g. loading a single value)

## When not to use
- Full-page or large-section loading — use Skeleton Loader
- Measurable progress — use Progress Bar
- The sole indicator that an app is loading — pair with context (text, skeleton) → UX Standards §3.1

---

## Anatomy

1. **Spinner graphic** — a circular animation. Required
2. **Label** — optional text like "Loading..." adjacent to the spinner. Optional but recommended when the spinner stands alone

---

## Variants

### Default
- **Purpose:** standard small spinner for inline use
- **Visual distinction:** circular spin, matches text colour

### Button spinner
- **Purpose:** replaces or accompanies button label during async action
- **Visual distinction:** sized to match button text height. White or contrasting with button background

### Page spinner
- **Purpose:** centred spinner for a content area while loading (when skeleton isn't suitable)
- **Visual distinction:** larger, centred in the content area with optional "Loading" text

---

## States

One state: spinning. The spinner is rendered or not. It has no interactive states.

---

## Interaction spec

Non-interactive. The spinner should not receive focus or block user interaction unless the parent control is disabled (e.g. a disabled button in loading state).

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| All | Spinner size is fixed. No breakpoint changes |

---

## Accessibility

- **Screen reader:** the spinner itself is `aria-hidden="true"`. Accompany it with a visually hidden `<span>` that says "Loading" using `role="status"` or `aria-live="polite"` on the container
- **Button context:** the button's `aria-busy="true"` communicates loading to screen readers
- **WCAG:** 4.1.3

---

## Motion & animation

- **Spin:** continuous rotation, 0.6–0.8s per cycle. Use a partial-circle graphic (e.g. 270° arc) so the spin is visible
- **Reduced motion:** replace spin with a pulsing opacity animation → UX Standards §13.9

---

## Common pitfalls

1. **Spinner as the only indicator** — a spinner in the middle of a blank screen tells the user nothing. Add context or use skeletons
2. **Spinner too small or too large** — match the context. Button spinners match text size. Page spinners are larger but not enormous
3. **No screen reader announcement** — without `aria-live` or `role="status"`, the spinner is invisible to assistive technology
4. **Spinner left spinning after load** — ensure the spinner is removed when data arrives. This sounds obvious but is a common async bug

---

## Tailwind notes

SVG spinner:
- `animate-spin h-5 w-5` (default size, adjust per context)
- `h-4 w-4` for button spinners
- `h-8 w-8` for page-level spinners

Use an SVG circle with a partial stroke:
```html
<svg class="animate-spin h-5 w-5" viewBox="0 0 24 24" fill="none">
  <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4" />
  <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z" />
</svg>
```

Colour inherits via `currentColor`. In buttons, the spinner takes the button's text colour. Standalone spinners use `text-gray-400` or the action primary colour.
