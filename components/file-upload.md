# File Upload

> A control for selecting and uploading files via click or drag-and-drop. Handles file validation, preview, and upload progress.

---

## When to use
- Any form or workflow where the user needs to attach files (images, documents, data files)
- Profile photo upload, document submission, CSV import, media attachment

## When not to use
- Capturing images or video live — use a camera/media capture component
- Pasting content — handle paste events separately, though a file upload may complement paste

---

## Anatomy

1. **Drop zone** — a bordered area that accepts drag-and-drop. Required
2. **Upload prompt** — text and/or icon explaining what to do ("Drag files here or click to browse"). Required
3. **File input** — hidden native `<input type="file">` triggered by the drop zone or a button. Required
4. **Accepted formats hint** — text indicating allowed file types and size limits. Recommended
5. **File list** — list of selected/uploaded files with name, size, status. Conditional
6. **File preview** — thumbnail for images, icon for documents. Optional
7. **Progress indicator** — upload progress per file. Conditional
8. **Remove button** — removes a file from the queue. Required when files are listed
9. **Validation message** — error when a file fails validation (wrong type, too large). Conditional

---

## Variants

### Single file
- **Purpose:** upload exactly one file (e.g. profile photo)
- **Visual distinction:** replacing the current file clears the previous one. May show a preview directly in the drop zone

### Multi-file
- **Purpose:** upload multiple files at once
- **Visual distinction:** file list accumulates below or beside the drop zone. Each file has its own status

### Compact / button-only
- **Purpose:** tight layouts where a drop zone is impractical
- **Visual distinction:** a button that opens the file picker. No drag-and-drop area

---

## States

| State | Visual treatment | Notes |
|---|---|---|
| Empty | Drop zone with prompt text and icon | |
| Drag over | Border highlights (dashed border changes colour), background tints | Visual feedback that drop is accepted |
| Drag over (invalid) | Error-coloured border if the dragged file type is not accepted | |
| Files selected | File list visible with names, sizes, previews | |
| Uploading | Progress bar per file, upload percentage | → UX Standards §3.1 |
| Upload complete | Success indicator per file | |
| Upload failed | Error indicator per file with retry option | → UX Standards §4.4 |
| Disabled | Muted drop zone, not interactive | |

---

## Interaction spec

### Mouse / pointer
- Click the drop zone or prompt to open native file picker
- Drag files from the OS file manager into the drop zone
- Click remove button on individual files to dequeue

### Keyboard
- `Tab` to focus the drop zone/button
- `Enter` or `Space` opens the file picker
- File list items should be navigable; remove buttons focusable

### Touch
- Tap the drop zone to open file picker (OS-level file selection)
- Drag-and-drop is not available on most mobile devices — the click/tap path must be the primary interaction

### Validation
- Validate file type, size, and count client-side before upload begins
- Show specific error messages: "File type not supported. Accepted formats: PDF, PNG, JPG" or "File exceeds the 10MB limit"
- Do not silently reject files → UX Standards §2.3

---

## Responsive behaviour

| Breakpoint | Behaviour |
|---|---|
| `xs`–`sm` | Drop zone full-width. File list stacks vertically. Compact/button variant may be preferred |
| `md`+ | Drop zone can be wider. File list may use a side-by-side layout for preview + details |

On mobile, prioritise the tap-to-browse flow since drag-and-drop isn't available. The drop zone acts as a large tap target.

---

## Accessibility

- **Role / semantics:** the drop zone should contain a visually hidden `<input type="file">` with appropriate `accept` attribute
- **Accessible name:** `<label>` for the file input, or `aria-label` on the drop zone describing the action
- **File list:** use a `<ul>` with each file as a `<li>`. Remove buttons need `aria-label`: "Remove [filename]"
- **Upload progress:** announce progress via `aria-live="polite"` or `role="progressbar"` with `aria-valuenow`
- **Drag-and-drop:** announce drop zone state changes if possible, but do not rely on drag-and-drop as the only method
- **WCAG:** 1.3.1, 4.1.2, 2.1.1

---

## Motion & animation

- **Drag over:** border and background transition, 150ms ease-out
- **File added to list:** fade in + slide down, 150ms ease-out
- **File removed:** fade out, 100ms ease-in. Remaining items reflow smoothly
- **Progress bar:** smooth fill animation, no easing (linear is appropriate for progress)
- **Reduced motion:** remove slide; fade and progress fill are acceptable

---

## Common pitfalls

1. **No feedback on drag over** — when a user drags a file over the drop zone, nothing changes visually. Always provide a drag-over state
2. **Silent rejection of invalid files** — if a file is rejected for type or size, show an explicit error message explaining why → UX Standards §2.3
3. **No progress indicator** — uploading without visual feedback makes users think the action failed. Show per-file progress for uploads that take more than a moment
4. **Missing retry on failure** — if an upload fails, the user needs a retry action per file, not just a generic error → UX Standards §4.4
5. **Drop zone steals page-level drop events** — ensure drag-and-drop handling is scoped to the drop zone element, not the entire page
6. **File size limits not communicated upfront** — show accepted types and max size in the drop zone prompt, not only after rejection

---

## Tailwind notes

Drop zone:
- `flex flex-col items-center justify-center rounded-lg border-2 border-dashed p-8 text-center transition-colors cursor-pointer`
- Default: `border-gray-300 bg-gray-50`
- Drag over: `border-[var(--color-action-primary)] bg-[var(--color-action-primary)]/5`
- Focus: `focus-within:ring-2 focus-within:ring-[var(--color-action-primary)]`

File list:
- `flex flex-col gap-2 mt-4`
- File row: `flex items-center gap-3 rounded-md border p-3`
- Progress bar: `h-1.5 rounded-full bg-gray-200` with `bg-[var(--color-action-primary)]` fill
- Remove: icon button pattern (see icon-button.md)

Hide the native input: `sr-only` (Tailwind's screen-reader-only class).
