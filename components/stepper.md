# Stepper / Wizard

> A multi-step process indicator that shows progress through a linear sequence of stages. Guides the user through complex tasks broken into manageable steps.

---

## When to use
- Multi-step forms (registration, checkout, onboarding)
- Workflows with a defined sequence where later steps depend on earlier ones
- When breaking a complex task into stages reduces cognitive load → UX Standards §1.6

## When not to use
- Non-linear processes where the user can complete steps in any order — use Tabs or a checklist
- Only 2 steps — just show step 1 and step 2 without a formal stepper
- Very long processes (10+ steps) — reconsider the workflow design

---

## Anatomy

1. **Step indicators** — numbered or icon-based markers for each step. Required
2. **Step labels** — text describing each step. Required at desktop, may be hidden on mobile
3. **Connector lines** — lines between step indicators showing sequence. Required
4. **Step content area** — the form or content for the current step. Required
5. **Navigation buttons** — "Back" and "Next" / "Continue" buttons. Required
6. **Completion indicator** — check mark or filled state on completed steps. Required

---

## Variants

### Horizontal
- **Purpose:** standard layout for 3–6 steps
- **Visual distinction:** steps arranged left to right with connectors between them

### Vertical
- **Purpose:** longer step sequences or when step labels need more space
- **Visual distinction:** steps stacked vertically, content beside or below each step

### Compact / mobile
- **Purpose:** small screens where a full stepper is too wide
- **Visual distinction:** "Step 2 of 5" text + progress bar, or dots indicator

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Incomplete | Neutral/grey indicator, muted label | |
| Current | Highlighted indicator (brand colour), bold label | |
| Completed | Check icon or filled indicator, connected line fills | |
| Error | Error colour on the step indicator, error icon | When the step has validation errors |
| Disabled / Locked | Muted, not clickable | For steps that can't be reached yet |

---

## Interaction spec

### Mouse / pointer
- Click "Next" to advance, "Back" to return
- Clicking a completed step to jump back is optional — allow if the process supports non-destructive revisiting. Do not allow clicking forward to uncompleted steps

### Keyboard
- `Tab` moves through the navigation buttons and any interactive elements in the current step
- Step indicators may be focusable links (if click-to-navigate is supported)

### Touch
- Navigation buttons are the primary controls. Full-width on mobile
- Step indicators should be tappable if click-to-navigate is supported

### Validation
- Validate the current step before allowing advancement to the next
- Show inline errors on the current step, not a generic error → UX Standards §7.2
- Preserve user input if they navigate back and forward → UX Standards §7.7

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Compact variant: "Step 2 of 5" with a progress bar or dots. Full horizontal stepper doesn't fit |
| `md` | Horizontal stepper may show icons/numbers without labels, or use a condensed layout |
| `lg`+ | Full horizontal stepper with labels visible |

Navigation buttons on mobile should stack vertically with "Next" above "Back", or sit side-by-side with "Next" as the primary action on the trailing side.

---

## Accessibility

- **Role / semantics:** step list as `<ol>` for sequential meaning. Current step indicated via `aria-current="step"`
- **Step navigation:** if steps are clickable, use links or buttons with appropriate labels ("Go to step 2: Address details")
- **Progress:** announce step changes: "Step 2 of 5: Address details" via a live region or page title update
- **Validation errors:** announce errors when the user attempts to advance → UX Standards §4.2
- **WCAG:** 1.3.1, 2.4.8, 4.1.2

---

## Motion & animation

- **Step transition:** content area crossfades or slides horizontally, 200ms ease-out
- **Connector fill:** line fills from left to right on completion, 300ms ease-out
- **Completion check:** check icon scales in, 150ms ease-out
- **Reduced motion:** instant transitions → UX Standards §13.9

---

## Common pitfalls

1. **No validation before advancing** — letting users skip ahead without completing required fields creates errors downstream. Validate on "Next"
2. **Losing user input on back navigation** — going back and forward must preserve everything the user entered → UX Standards §7.7
3. **Stepper is too wide on mobile** — 5 steps with labels won't fit at 375px. Use the compact variant
4. **No indication of total steps** — the user must know how many steps remain. Always show total count
5. **"Submit" on a non-final step** — navigation buttons must use descriptive labels. "Next: Review details", not "Submit" until the final step → UX Standards §8.2
6. **Allowing forward jumps to incomplete steps** — clicking step 4 when step 2 isn't done creates confusion. Only allow jumping back to completed steps

---

## Tailwind notes

Horizontal stepper:
- Container: `flex items-center justify-between`
- Step: `flex flex-col items-center`
- Indicator: `h-10 w-10 rounded-full flex items-center justify-center text-sm font-medium`
- Current: `bg-[var(--color-action-primary)] text-white`
- Completed: `bg-[var(--color-status-success)] text-white` with check icon
- Incomplete: `bg-gray-200 text-gray-500`
- Connector: `flex-1 h-0.5 bg-gray-200 mx-2` with completed fill via `bg-[var(--color-action-primary)]`

Compact:
- `flex items-center gap-3`
- Progress bar: `flex-1 h-1.5 rounded-full bg-gray-200` with proportional fill
- Text: `text-sm text-gray-600 whitespace-nowrap`
