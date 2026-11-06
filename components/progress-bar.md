# Progress Bar

> A horizontal bar that visually communicates progress toward completion. Can be determinate (known total) or indeterminate (unknown duration).

---

## When to use
- File uploads showing percentage complete
- Multi-step processes showing overall completion
- Long-running operations where progress can be measured
- Data import/export progress

## When not to use
- Unknown-duration operations with no progress measurement — use Spinner
- Brief operations (under 1 second) — show nothing, or use an optimistic update → UX Standards §3.5
- Loading content into a layout — use Skeleton Loader

---

## Anatomy

1. **Track** — the full-width background bar representing 100%. Required
2. **Fill** — the coloured portion indicating current progress. Required
3. **Label** — text indicating what is in progress. Optional
4. **Percentage / count** — numeric progress value. Recommended for determinate bars
5. **Status text** — "Uploading 3 of 7 files" or similar. Optional

---

## Variants

### Determinate
- **Purpose:** progress can be measured (e.g. 45% complete)
- **Visual distinction:** fill width maps to a percentage

### Determinate against a limit (yard-demo `ProgressBar`)
- **Purpose:** how much of a limit is used, for example time on site against an SLA threshold. A tick marks the limit, and the track runs past it so an overage shows
- **Scale:** the limit tick sits at 75% of the track, so the track is 133.3% of the limit. The bar never rescales, so the tick is in the same place on every bar. If the value goes past the end of the track, the fill stops at the end, a small "+" shows there, and the caption carries the exact overage ("Breached by 1h 52m")
- **Fill colour:** the whole fill is the colour of the result: green within the limit (and when met), yellow near the limit (the last 15%), orange when breached (from the start of the bar, including the part before the tick). Cyan and purple are not used for status: they mean inbound and outbound in this app. The empty track stays grey
- **Cues:** met, near and breached put words in the caption, with no icon: "Met", "Near the limit: 14 mins left", "Breached by 12 mins". The limit tick is a shape. Colour is never the only signal
- **Props:** `value` (where the fill ends, percent of the track, over 100 shows "+"), `label` (the accessible name, required), `limit` (where the tick sits, percent of the track), `tone` (`default`, `met`, `near`, `breached`), `valueText`, `caption` (visible text under the bar)
- **Accessibility:** `aria-valuenow` is how far along the track the fill is (0 to 100, held at 100 past the end). `aria-valuetext` says the value in words and where the limit is marked: "1h 32m against a limit of 2h, the limit is marked at 75% of the bar. 28 mins left"
- **No threshold, no bar:** when a measure has no limit, show the measured value as text only. Do not draw an empty or full bar
- **Finished:** the bar freezes at its final value and the caption says Met or Breached by N
- **Contrast** (dark card gray-950, track gray-700 with a gray-500 outline): fill against the track green-400 5.95:1, yellow-500 7.5:1, orange-500 3.8:1. The tick is white with a 1px gray-950 outline, so against orange-500 the outline is 7.3:1, against green-400 11.4:1, and above and below the bar the white tick is 19.8:1 against the card. Caption text at least 4.5:1 (green-400 11.4:1, yellow-400 15:1, orange-400 8.8:1)
- **Motion:** none. The fill jumps to the new value, so there is nothing to turn off for reduced motion

### Indeterminate
- **Purpose:** operation is running but progress is unknown
- **Visual distinction:** animated fill that cycles or pulses across the track

### Segmented
- **Purpose:** multi-phase processes (e.g. upload → process → complete)
- **Visual distinction:** track divided into segments, each filling independently

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| In progress | Fill at current percentage, animated for indeterminate | |
| Complete | Fill at 100%, may turn green/success colour | |
| Error | Fill stops, bar turns red/error colour, error message shown | |
| Paused | Fill at current position, no animation | Rare |

---

## Interaction spec

Progress bars are non-interactive display elements.
- Cancel buttons for long operations should be adjacent to (not inside) the progress bar
- Error state should offer a retry action

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| All | Full-width of its container. Height remains constant |

---

## Accessibility

- **Role / semantics:** `role="progressbar"` with `aria-valuenow`, `aria-valuemin="0"`, `aria-valuemax="100"` for determinate. For indeterminate, omit `aria-valuenow`
- **Label:** `aria-label` describing the operation: "Uploading files" or `aria-labelledby` pointing to visible label text
- **Updates:** announce significant milestones (25%, 50%, 100%) via live region or periodic `aria-valuenow` updates. Do not announce every percentage increment
- **WCAG:** 4.1.2, 1.3.1

---

## Motion & animation

- **Determinate fill:** smooth width transition, linear easing (appropriate for progress)
- **Indeterminate:** repeating animation — a highlight bar sliding across the track, 1.5–2s duration
- **Completion:** brief colour transition to success, 200ms
- **Reduced motion:** determinate fill can still animate (it's functional). Indeterminate animation may be simplified to a pulsing opacity → UX Standards §13.9

---

## Common pitfalls

1. **No text progress alongside the bar** — a bar at 60% is ambiguous without "60%" or "3 of 5 files" text
2. **Indeterminate bar for measurable progress** — if you know the progress, show it. Users prefer determinate feedback
3. **Progress bar for fast operations** — operations under 1 second don't need a progress bar. They finish before the user processes it
4. **No error state** — if the operation can fail, the progress bar needs an error treatment and retry action
5. **Jerky fill updates** — smooth the transition between progress values. Don't jump from 10% to 40% in a single frame

---

## Tailwind notes

Track:
- `w-full h-2 rounded-full bg-gray-200 overflow-hidden`

Fill (determinate):
- `h-full rounded-full bg-[var(--color-action-primary)] transition-[width] duration-300`
- Width set via inline style: `style="width: 45%"`

Fill (indeterminate):
- `h-full w-1/3 rounded-full bg-[var(--color-action-primary)] animate-[indeterminate_1.5s_ease-in-out_infinite]`
- Define keyframe: `@keyframes indeterminate { 0% { transform: translateX(-100%); } 100% { transform: translateX(400%); } }`

Label and percentage:
- `flex items-center justify-between text-sm mb-1.5`
