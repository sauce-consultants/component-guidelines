# Toggle / Switch

> An on/off control for binary settings that take effect immediately. Visually resembles a physical switch.

---

## When to use
- Settings that take effect immediately on toggle (e.g. "Enable notifications")
- Binary preferences where the result is visible right away
- Any on/off control where the user expects instant feedback

## When not to use
- Form fields that submit with other data — use Checkbox (toggles imply immediate effect)
- Choosing between two non-boolean options — use Radio Group
- Triggering an action (like save or send) — use Button

---

## Anatomy

1. **Track** — the pill-shaped background. Required
2. **Thumb** — the sliding circular indicator. Required
3. **Label** — text describing what the toggle controls. Required → UX Standards §7.1
4. **Status text** — optional "On"/"Off" text reinforcing the state. Optional but recommended → UX Standards §10.4

---

## Variants

### Default
- **Purpose:** standard on/off toggle
- **Visual distinction:** track changes colour between on (brand/success) and off (neutral grey). Thumb slides left/right

### With status text
- **Purpose:** makes the current state explicit for users who may not distinguish on/off by colour alone
- **Visual distinction:** "On" / "Off" label beside or inside the track

### Compact
- **Purpose:** tighter layouts, toolbars, table rows
- **Visual distinction:** smaller track and thumb. Label may be positioned differently

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Off | Grey/neutral track, thumb on left (LTR) | |
| On | Brand/success coloured track, thumb on right | |
| Hover | Slight track colour shift or thumb shadow | |
| Focus | Focus ring around the track | → UX Standards §13.5 |
| Disabled | Muted colours, not interactive | Can be disabled in either on or off state |
| Loading | Optional spinner on thumb while server confirms | For toggles that call an API |

---

## Interaction spec

### Mouse / pointer
- Click anywhere on the track or thumb to toggle
- Click on the label to toggle (same as clicking the control)

### Keyboard
- `Space` toggles the switch when focused
- `Enter` may also toggle (be consistent with your implementation)
- `Tab` to navigate to/from the control

### Touch
- Tap to toggle. Swipe across the track to toggle (optional enhancement)
- Entire track + thumb is the tap target. Must meet 44×44px → UX Standards §13.6

### Optimistic UI
- Toggle should apply visually immediately. Confirm with the server in the background. If the server rejects, revert and show an error → UX Standards §3.5

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Toggle + label in a single row. Align toggle to the right edge in settings lists |
| `md`+ | Same pattern. Toggles in settings pages often appear in a list with labels on the left, toggles on the right |

The toggle itself does not change size across breakpoints, but its containing layout may adapt.

---

## Accessibility

- **Role / semantics:** `role="switch"` with `aria-checked="true/false"`. Alternatively, use `<input type="checkbox" role="switch">`
- **Accessible name:** linked `<label>` or `aria-label`
- **State communication:** `aria-checked` must update on toggle. Screen readers will announce "on" or "off"
- **Do not use colour alone** — always pair the colour change with thumb position and optionally status text → UX Standards §10.4
- **WCAG:** 4.1.2, 1.4.1 (Use of Colour), 2.1.1

---

## Motion & animation

- **Toggle:** thumb slides from one side to the other, 150–200ms ease-out. Track colour transitions simultaneously
- **Loading:** spinner on thumb, faded track
- **Reduced motion:** instant position change, no sliding. Colour change is acceptable

---

## Common pitfalls

1. **Using a toggle in a form that requires submission** — toggles imply immediate effect. If the change only applies when the user clicks "Save", use a Checkbox instead
2. **Colour as the only state indicator** — a grey-to-green colour change fails colour-blind users. The thumb position is the primary indicator; status text adds further clarity → UX Standards §10.4
3. **Track too small for touch** — the track must be large enough to tap comfortably. Expand hit area if needed
4. **No optimistic update** — if the toggle calls an API, show the toggled state immediately and revert on failure. A delay between tap and visual change feels broken → UX Standards §3.5
5. **Missing `role="switch"`** — without this role, screen readers treat it as a checkbox, which misleads users about the expected behaviour

---

## Tailwind notes

Track:
- `relative inline-flex h-6 w-11 rounded-full transition-colors` 
- Off: `bg-gray-200`
- On: `bg-[var(--color-action-primary)]`

Thumb:
- `absolute h-5 w-5 rounded-full bg-white shadow transition-transform`
- Off: `translate-x-0.5`
- On: `translate-x-[22px]`

Use a visually hidden `<input type="checkbox" role="switch">` for native form integration, with the styled track/thumb as siblings controlled via CSS.
