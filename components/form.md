# Form (Pattern)

> A composite pattern for collecting and submitting structured user input. This is not a single component but a set of layout, validation, and submission conventions that apply when multiple form controls are composed together.

→ UX Standards §7 covers form rules in detail. This file covers the structural pattern.

---

## When to use
- Any workflow collecting structured input from the user: registration, checkout, settings, data entry, CRUD operations

## When not to use
- Read-only data display — no form needed
- Search — see the Search pattern (search.md)

---

## Anatomy

1. **Form container** — `<form>` element wrapping all fields. Required
2. **Form sections** — logical groups of related fields (e.g. "Personal details", "Address"). Recommended for forms with 6+ fields
3. **Section headings** — titles for each section. Recommended
4. **Fields** — individual form controls (Input, Select, Checkbox, etc.). Required
5. **Field layout** — arrangement of fields (single column, multi-column). Required
6. **Validation summary** — error summary at the bottom of the form, above the submit actions. Optional
7. **Action bar** — submit + cancel buttons, typically at the bottom. Required
8. **Progress indicator** — for multi-step forms, see stepper.md. Conditional

---

## Layout conventions

### Field arrangement
- Default to single-column layout. It's faster to scan and harder to get wrong
- Two-column layout is acceptable for naturally paired fields (first name / last name, city / postcode)
- Never exceed two columns of form fields at any breakpoint

### Field width
- Fields should be proportional to the expected input length: a short postcode field should be narrower than an address field
- Don't stretch all fields to full width uniformly — it wastes space and makes short-input fields feel wrong
- Use `max-w-xs`, `max-w-sm`, `max-w-md`, `max-w-lg` to control field widths semantically

### Spacing
- Consistent vertical spacing between fields. Use `gap-6` or equivalent
- Group-level spacing (between sections) should be larger than field-level spacing

### Labels
- Labels above fields (stacked). Not beside fields (inline), which causes alignment headaches at every breakpoint → UX Standards §7.1
- Label text: sentence case → UX Standards §18.3

### Required / optional
- Mark the minority. If most fields are required, mark the few that are optional → UX Standards §7.4
- Use text ("Required" / "Optional"), not asterisks that need a legend

---

## Validation

### Timing
- Validate on blur (when the user leaves a field) → UX Standards §7.2
- Exception: password strength indicators update in real time
- Do not validate on every keystroke — it interrupts the user

### Inline errors
- Show errors adjacent to the field they relate to → UX Standards §7.3
- Error messages must be specific: what's wrong and what the correct format is
- Use `aria-invalid="true"` and `aria-describedby` on the field → UX Standards §7.3
- An inline error replaces the field's help text in the same slot. Never show both, and never add a line: controls must not move down when an error appears
- A disabled field shows only its hint, not an error

### Summary errors
- For complex forms, a summary at the bottom of the form, above the submit actions, listing all errors with links to the relevant fields is a useful addition
- The summary appears on submission attempt, not during input
- Use `role="alert"` on the summary

### Preserve input
- If submission fails, all user input must be preserved → UX Standards §7.7
- Never reset the form on error

---

## Submission

### Button placement
- Submit button at the bottom of the form, aligned left (following the scan line)
- Primary action on the right in button groups (submit), secondary on the left (cancel)
- Button label describes the action: "Create account", "Save changes", not "Submit" → UX Standards §8.2

### Loading state
- Submit button enters loading state and is disabled during submission → UX Standards §3.4
- Other form controls may be disabled during submission to prevent modification

### Success feedback
- After successful submission, show clear confirmation → UX Standards §7.6
- Navigate to the relevant view, show a toast, or display inline confirmation

### Destructive forms
- Confirmation required for destructive actions → UX Standards §7.5
- Destructive button uses danger styling and specific label → UX Standards §7.5

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | All forms single-column. Full-width fields. Action buttons full-width or stacked |
| `md` | Two-column pairs where appropriate. Fields constrained to readable widths |
| `lg`+ | Same as `md`. Do not let forms stretch to full page width on wide screens. Constrain with `max-w-2xl` or similar |

---

## Accessibility

- **Semantics:** use `<form>`, `<fieldset>`, `<legend>`, `<label>` elements properly
- **Field association:** every field has a `<label>` with matching `for`/`id` → UX Standards §7.1
- **Grouping:** related fields (e.g. radio groups, address sections) wrapped in `<fieldset>` with `<legend>`
- **Error handling:** errors linked via `aria-describedby`, fields marked `aria-invalid` → UX Standards §7.3
- **Tab order:** follows visual order (top to bottom, left to right for multi-column) → UX Standards §13.5
- **Autocomplete:** set `autocomplete` attributes for personal data fields (name, email, address, payment)
- **WCAG:** 1.3.1, 1.3.5 (Identify Input Purpose), 3.3.1, 3.3.2, 3.3.3

---

## Motion & animation

- **Validation message:** fade in, 150ms ease-out. No animation on the error border itself — it should appear immediately
- **Section reveal (progressive forms):** height/opacity transition, 200ms ease-out
- **Reduced motion:** instant transitions → UX Standards §13.9

---

## Common pitfalls

1. **No visible labels** — using placeholders as labels is the most common form anti-pattern → UX Standards §7.1
2. **Validating on every keystroke** — interrupts users mid-input → UX Standards §7.2
3. **Generic button labels** — "Submit" tells the user nothing. Be specific → UX Standards §8.2
4. **Form reset on error** — losing the user's input is unforgivable → UX Standards §7.7
5. **No error summary for long forms** — if the form has many fields and errors are only inline, the user may not scroll down to find them. Add a summary at the bottom of the form, above the submit actions
6. **Three-column field layouts** — never. Two columns maximum, and only for naturally paired fields
7. **Inconsistent field widths** — all fields at `w-full` makes a postcode field feel wrong next to an address field. Size fields to their content
8. **Missing `autocomplete`** — browsers and password managers rely on it. Set it on every relevant field
9. **No confirmation after success** — the user submits and sees... nothing. Always confirm → UX Standards §7.6

---

## Tailwind notes

Form container:
- `max-w-2xl mx-auto` (or constrained by the layout)

Field group:
- `flex flex-col gap-6`

Two-column pair:
- `grid grid-cols-1 sm:grid-cols-2 gap-4`

Section:
- `space-y-6 pt-8 first:pt-0` with a heading: `text-lg font-semibold mb-2`

Action bar:
- `flex items-center justify-end gap-3 pt-6 border-t mt-8`
- Or sticky on mobile: `sticky bottom-0 bg-white border-t px-4 py-3`

Individual fields follow the patterns in their respective component files (input.md, select.md, etc.).

---

## Unsaved changes and leaving a form

> Added from the yard-demo Create booking flow (issue #99).

- **Ask once a multi-step booking is in progress (round 3, issue #99).** A wizard that saves completed steps (Create booking) asks on any exit once anything is entered or a step is completed, even on a step with no edits; a fresh empty first step asks nothing. Copy on an edited step: "Steps you've completed are saved as a pending booking. Changes on this step won't be." On a clean step: "Your booking is saved as a pending booking. You can continue it from Pending bookings." A single-step form still asks only when something would be lost: nothing changed means leaving is silent, and putting a field back makes it clean again
- **Returning user:** the latest unfinished draft is kept in the browser's localStorage (per app, written after the first Next, no expiry, cleared on confirm, discard or Start new). Opening Create on a fresh empty first step with one shows a Dialog "Continue your pending booking?" (reference, type and company, last step completed, when saved) with "Continue" (primary) and "Start new". Not shown mid-booking or when a link lands on a later step. Browser-local only: no cross-device, and cleared storage means no prompt (the booking is still in Pending bookings)
- **Cover every way out.** The exit button, sidebar and menu links, the browser Back button and closing the tab or reloading. In-app routes (including Back) show a Dialog; closing the tab can only show the browser's own prompt (`beforeunload`), which cannot be styled
- **Dialog copy:** title as a question ("Leave this booking?"), one or two sentences saying what is kept and what is lost, then two buttons. "Keep editing" is the primary action and the default focus; the destructive way out is the secondary button, named for what it does ("Leave" in Create booking, where the wording above says what is kept), never "OK" or "Yes"
- **Moving within a multi-step flow** (Next, Back, Edit from the summary) never asks. The guard covers leaving the flow
- **Exit label:** say what happens. "Exit booking", not "Save and exit", when the current step's edits are not saved
- Live Core has no leave guard in the booking flow today (`booking_type.html.heex:10-12`): Cancel is a plain link
