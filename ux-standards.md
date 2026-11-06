# UX Standards

**Version:** 2.0  
**Date:** March 2025  
**Author:** Sauce  
**Status:** Active

---

## How to read this document

Three levels of requirement are used throughout:

- **must** — a standard. Apply to all software built at Sauce, no exceptions.
- **should** — a best practice. Apply unless client budget or requirements explicitly prevent it.
- **could** — an enhancement. Apply when there is time and it adds clear value.

This document is written for use by both human contributors and AI coding agents. Rules are stated as directives. Where rationale is given, it is brief.

---

## 1. Core principles

These principles underpin every decision in this document. When a situation is not covered by a specific rule, use these to reason toward the right answer.

**1.1** The interface must communicate system state at all times. A user must never be left wondering whether something is loading, broken, or empty.

**1.2** Interfaces must use language and concepts familiar to the target user. Avoid technical terminology in user-facing text.

**1.3** Users must be able to reverse or escape actions. Where an action is irreversible, warn the user clearly before they take it.

**1.4** Graphic elements and terminology must be used consistently. An icon, label, or colour that means one thing on one screen must mean the same thing on every screen.

**1.5** Errors must be prevented where possible. Where they cannot be prevented, they must be expressed in plain language with a clear path to resolution.

**1.6** Interfaces must minimise cognitive load. Do not ask a user to remember something from one screen to use on another. Keep task-relevant context visible.

**1.7** Frequent actions must be reachable quickly. Power users must not be slowed down by interfaces designed only for first-time use.

**1.8** Screens must show only what is needed for the current task. Remove clutter. Every element on screen competes for attention.

---

## 2. System status

### 2.1 Monitoring

The team must set up alerting to detect when any system dependency goes down. Sauce applications commonly depend on multiple systems simultaneously: a web or native client, an in-house API, hardware, third-party APIs, and third-party services. Failure of any one of these should surface to the user in a controlled way, not as a silent failure or broken state.

### 2.2 Proactive status communication

If a system dependency is unavailable, the interface must inform the user before they attempt an action that will fail. Do not wait for the action to fail before showing the problem.

### 2.3 Error message language

Error messages must be written in plain English. Never expose raw error codes, stack traces, or technical identifiers to end users. Every error message must tell the user what happened and what they can do next.

**Good:** "We couldn't save your changes. Check your connection and try again."  
**Bad:** "Error 503: Service unavailable."

---

## 3. Loading states

### 3.1 Always show loading state

When data is being fetched or an operation is running, the interface must make this visible. A blank screen must never be the loading state — users will assume data loss or a crash.

### 3.2 Skeleton screens

Where a screen has a known layout, use skeleton screens rather than generic spinners. Render placeholder shapes in the approximate position of real content, then replace them as data arrives. This reduces perceived load time and prevents layout shift.

Skeleton placeholders must use neutral, low-contrast colours (grey tones). They must not use the application's accent colours.

### 3.3 Partial data display

On screens with multiple independent data regions, each region should load and render independently. Do not hold the entire screen blank while a secondary region is still loading.

For search and filter interactions, the previous result set should remain visible until new results are ready. Do not flash a loading spinner between each keystroke.

### 3.4 Button loading states

When a user triggers an action via a button, the button must enter a loading state: a visual indicator that the action is in progress (typically a spinner replacing or accompanying the label). The button must be disabled while loading to prevent duplicate submissions. The button should not change size during this transition

If other actions on the same screen would conflict with the running action, those controls should also be disabled during loading.

Do not apply artificial minimum loading times. Instead, use optimistic UI updates (see section 3.5) to avoid jarring transitions for fast operations.

### 3.5 Optimistic UI

For actions that are very likely to succeed (toggling a setting, adding a tag, marking a record), apply the result immediately in the UI and then confirm with the server in the background. If the server rejects the action, revert the UI and show an error. This approach gives users immediate feedback without fake delays.

Optimistic UI should not be used for destructive or financially significant actions — wait for server confirmation before updating the UI.

### 3.6 Splash screens

If required native and web applications should display a branded loading screen on initial launch. This confirms to the user that the application has started. Subtle animation could improve the experience. The splash screen must not persist longer than necessary — dismiss it as soon as the application is ready.

---

## 4. Error states

### 4.1 Error screens

Every application must include a generic error screen for unhandled exceptions. When an unrecoverable error occurs, present this screen rather than leaving the user in a broken or frozen state. It must: explain that something went wrong, avoid technical detail, provide at least one recovery action (e.g. reload, go to home), and remain visually coherent with the product brand.

### 4.2 Inline errors

For expected, recoverable errors (a failed API call, a validation failure), show the error inline, close to the control or content that caused it. Do not use full-screen error states for errors a user can resolve themselves.

### 4.3 Offline detection

Applications must detect loss of network connectivity and inform the user. The offline notice must be visible and persistent while the device is offline. When connectivity is restored, the notice must be dismissed automatically and, where appropriate, pending actions retried.

### 4.4 Recovery paths

Every error state must give the user something to do. At minimum this is a "Try again" action. For errors the user cannot self-resolve, give them a way to contact support.

---

## 5. Empty states

When a screen has no content to show — whether on first use, after a failed search, or because a list is genuinely empty — the interface must explain why. Do not render a blank area with no context.

Empty state messages must:

- Explain the specific reason for the empty state in the context of this screen (not a generic "Nothing here" message)
- Where relevant, tell the user what they could do to populate the view
- Be short and use plain language

Empty states are not errors. Keep the tone neutral or lightly constructive. Avoid apologetic language.

---

## 6. Filter and search states

### 6.1 Filter visibility

When a list or data view is filtered, the interface must make this visible. The user must be able to see at a glance that they are looking at a filtered subset, not the full dataset.

### 6.2 Filter clearing

A filtered view must provide a clear, single action to remove all active filters and return to the unfiltered state.

### 6.3 No results vs. empty dataset

Distinguish between "no results match your search" and "this dataset is empty." The message and recovery action for each is different.

---

## 7. Forms

### 7.1 Labels

Every form field must have a visible label. Do not use placeholder text as a substitute for a label. Placeholder text disappears on input and cannot be read by screen readers as a label.

### 7.2 Validation timing

Validate on blur (when the user leaves a field), not on every keystroke. Keystroke validation interrupts users mid-input and produces false errors.

The exception is password strength indicators, which should update in real time.

### 7.3 Validation messages

Validation errors must appear adjacent to the field they relate to, not only in a summary at the top of the form. Messages must be specific: tell the user what is wrong and what the correct format is.

**Good:** "Enter a valid UK postcode, for example SW1A 1AA."  
**Bad:** "Invalid input."

### 7.4 Required fields

Mark required fields, not optional ones. If most fields are required, marking the few that are optional is less noisy. Use a text label ("Required" or "Optional") rather than a symbol that requires a legend.

### 7.5 Destructive form actions

Destructive actions (delete, archive, reset) must require explicit confirmation. The confirmation prompt must describe exactly what will be deleted or changed. Confirmation buttons must not use generic labels — use specific labels that mirror the action: "Delete customer record", not "OK".

The primary/default button in a destructive confirmation must not be the destructive action. The user should have to take a deliberate action to proceed.

### 7.6 Submission feedback

After a successful form submission, give the user clear confirmation. Do not leave them on a blank or unchanged screen.

### 7.7 Preserve user input

If a form submission fails, preserve the data the user entered. Do not reset the form on error.

---

## 8. Buttons

### 8.1 Hierarchy

Each screen must have at most one primary action. Secondary actions use a visually subordinate button style. Destructive actions (delete, archive, revoke) must use a distinct style — typically a red or danger-coded appearance — separate from both primary and secondary styles.

### 8.2 Labels

Button labels must describe the action that will happen, not a generic directive. Use verb phrases.

**Good:** "Save changes", "Delete record", "Send invoice"  
**Bad:** "Submit", "OK", "Yes"

### 8.3 Disabled state

Disabled buttons must be visually distinct from active buttons. Where possible, explain why a button is disabled — either with a tooltip or inline hint text — rather than leaving the user to guess.

Do not use disabled buttons as a way to block navigation to a broken state. If an action is not available, consider whether the button should be hidden rather than disabled.

### 8.4 Tooltips

Interactive elements that are not self-explanatory must have a tooltip. Tooltips must appear on hover and focus. They must be short — a single sentence at most. Do not put critical information in tooltips that is unavailable any other way.

---

## 9. Data-dense interfaces

This section applies to screens displaying lists, tables, dashboards, and other high-density data views. These are common in logistics, operations, and management software.

### 9.1 Tables

Tables must have clear column headers. Headers must be concise — one to three words. Units of measurement belong in the column header, not repeated in every cell.

Columns must be a consistent width or follow a defined proportional grid. Do not allow columns to render at arbitrary widths based on content.

Right-align numeric columns. Left-align text columns. Center alignment should be used sparingly and only for short, fixed-width values (status badges, boolean flags).

### 9.2 Sorting

Sortable columns must be visually indicated. The current sort column and direction must be visible. Default sort order must be the most operationally useful — not alphabetical by default unless that is genuinely the best starting view.

### 9.3 Pagination vs. continuous scroll

Use pagination for data tables where the user needs to navigate to a specific page or track their position in a dataset. Use continuous scroll for feeds or browsable content where position tracking is not important.

When using pagination, show the total record count and the current range (e.g. "Showing 51–100 of 340").

### 9.4 Row actions

Actions available on individual rows must be consistent and predictable. Use icon buttons or a contextual action menu (three-dot menu) within the row. Do not use full button labels inside table rows — they consume too much space and introduce visual noise.

Destructive row actions (delete, archive) must follow the same confirmation pattern as form destructive actions (section 7.5).

### 9.5 Status indicators

Status values must use both colour and a text label. Never use colour alone to convey status — this fails for colour-blind users and in low-colour environments.

Status colours must be consistent across the entire application. If "red" means "failed" on one screen, it must mean "failed" everywhere.

### 9.6 Inline editing

If a table supports inline editing, the editable state of a cell must be clearly distinct from the read state. Show edit affordance on hover. Commit changes on blur or explicit confirmation; allow cancel via Escape.

### 9.7 Bulk actions

When bulk selection is available, the number of selected records must be visible at all times. Bulk actions must only appear when at least one record is selected. Bulk destructive actions must follow the same confirmation pattern as individual destructive actions.

### 9.8 Filters on data views

Filters applied to a data view must be visible as active filter chips or a summary. The total count of active filters should be shown. Clearing individual filters and clearing all filters must both be available.

When filters produce no results, show an empty state (section 5) that reminds the user filters are active and offers a clear-all action.

---

## 10. Colour

### 10.1 Semantic colour use

Colours must have consistent meaning across the application. Reserve red for errors, destructive actions, and critical alerts. Reserve amber/yellow for warnings and non-critical alerts. Reserve green for success states. Use brand colours for primary UI elements, not for status communication.

**Exception — brand colour conflicts:** If a brand colour falls within the red, amber, or yellow range, it must not be used for status communication. In this case, shift the corresponding status colour to avoid ambiguity. For example, if the brand colour is yellow, use a distinct amber-orange for warnings that is visually separable from the brand yellow at a glance. Document the specific status colour values chosen and the rationale in the product's colour token definitions. The principle remains: a user must never mistake a brand element for a status signal or vice versa.

Do not reuse status colours for decorative purposes.

### 10.2 Colour token approach

Define colours using Tailwind's palette as the source of truth for raw values, but reference them through semantic CSS custom properties or a token layer rather than using utility classes directly for status and brand colours.

Tailwind's numeric scale (`red-500`, `yellow-400`) is acceptable for one-off utility use in layouts and spacing. For colours that carry meaning — status, actions, backgrounds, text hierarchy — map them to semantic tokens so they can be changed in one place.

```css
/* Define once */
--color-status-error: theme("colors.red.500");
--color-status-warning: theme("colors.amber.600");
--color-status-success: theme("colors.green.600");
--color-action-primary: theme("colors.brand.500");
```

```html
<!-- Use semantically -->
<span class="text-[var(--color-status-error)]">Failed</span>
```

Alternatively, extend Tailwind's config to register semantic aliases directly:

```js
// tailwind.config.js
colors: {
  'status-error': colors.red[500],
  'status-warning': colors.amber[600],
  'status-success': colors.green[600],
}
```

This keeps Tailwind's utility class pattern while making a rebrand or dark mode variant a config change rather than a codebase search-and-replace.

### 10.3 Colour contrast

All text must meet the following minimum contrast ratios against its background:

- Normal text (below 18pt or 14pt bold): 4.5:1 minimum, 7:1 preferred
- Large text (18pt or above, or 14pt bold and above): 3:1 minimum, 4.5:1 preferred
- Non-text elements (icons, form borders, chart lines): 3:1 minimum

Use a contrast checker to verify values during design. The a11y project (https://www.a11yproject.com) and Stark (https://www.getstark.co) are suitable tools.

### 10.4 Colour is not the only signal

Never use colour as the sole means of communicating information. Always pair colour with a text label, icon, or pattern. This applies to status indicators, form validation states, chart data series, and all other communicative uses of colour.

### 10.5 Dark mode

When creating colour systems, consider both light and dark variants from the start. If a dark mode is not built initially, the colour token architecture should not make it prohibitively difficult to add later. Choose the initial mode based on the likely usage environment.

---

## 11. Typography

### 11.1 Base font size

The base body font size must be 16px. Do not use font sizes below 12px for any user-facing text. Sizes below 14px should be used sparingly and only for secondary, non-critical information (timestamps, metadata labels).

### 11.2 Line length

Body text line length must not exceed 80 characters. Lines longer than this reduce readability.

### 11.3 Line height and spacing

Set line height to at least 1.5× the font size for body text. Set paragraph spacing to at least 1.5× the line height.

### 11.4 Alignment

Use left alignment for body text. Do not justify text — justified text produces uneven word spacing, particularly problematic for dyslexic readers. Centre alignment is acceptable for short headings and standalone labels only; do not centre body copy.

### 11.5 Heading hierarchy

Use a consistent heading hierarchy. Do not skip levels (e.g. H1 directly to H3). Heading levels must reflect content structure, not visual appearance preferences. If a smaller heading is needed for visual reasons, apply a style to the correct semantic level rather than using a lower heading level.

### 11.6 Font weight

Use weight to create hierarchy. Do not rely solely on size. Avoid using more than two or three weights in a single design — the contrast between regular and bold is usually sufficient.

---

## 12. Icons

### 12.1 Consistency

All icons in an application must come from a single icon set. Do not mix icons from different sets within the same product. Icons must be consistent in weight, style (outlined vs. filled), and corner radius.

### 12.2 Size grid

Design icons to a 24px × 24px grid. Icons must sit within this grid, centred horizontally and vertically. Icons may be rendered at other sizes in the application but must maintain the original aspect ratio.

### 12.3 Colour

The base icon set must be monochromatic. Colour values must use `currentColor` so icons inherit their colour from CSS, avoiding duplicate assets for different colour contexts.

Icons requiring a secondary shade (to differentiate overlapping shapes) may use `fill-opacity` rather than a second hardcoded colour.

### 12.4 Labels

Icons used as interactive controls must have an accessible label. This can be a visible text label alongside the icon, or an aria-label / title for icon-only controls. Do not use icon-only buttons for non-obvious actions.

### 12.5 Naming convention

Icon file names and paths must together convey three things: that it is an icon, the variant (e.g. outline, filled), and a description of the content. How this is achieved — whether through directory structure, filename, or both — is up to the team.

---

## 13. Accessibility

Accessibility must not be treated as a final-stage audit. It must be built in throughout design and development.

### 13.1 Minimum font size

As specified in section 11.1: 16px base, no user-facing text below 12px.

### 13.2 Colour contrast

As specified in section 10.3.

### 13.3 Colour is not the only signal

As specified in section 10.4.

### 13.4 Screen reader support

All non-decorative images must have alt text. Alt text must describe the content or function of the image, not its appearance.

Interactive elements (buttons, links, inputs) must have accessible names. If a visible label is not present, provide an aria-label.

Dynamic content changes (loading new results, errors appearing, notifications) must be announced to screen readers via appropriate ARIA live region attributes.

Content must be ordered logically in the DOM — screen readers read from top to bottom. Visual reordering via CSS (flex/grid `order`) must not create a mismatch between visual and DOM order that breaks reading flow.

### 13.5 Keyboard navigation

All interactive elements must be reachable and operable via keyboard. Tab order must follow a logical reading sequence. Focus state must be visually visible — do not remove or suppress the browser's default focus ring without providing a clearly visible replacement.

### 13.6 Touch targets

Tap targets on touch interfaces must be at least 44px × 44px. This applies to buttons, links, checkboxes, radio buttons, and any other interactive element. Where a visual element is smaller than this, expand the hit area using padding.

### 13.7 Text resize

Applications must not break at 200% browser text zoom. Test layouts at increased zoom levels. Avoid fixed-height containers that clip text when font size increases.

### 13.8 Dyslexia considerations

Typography settings (see section 11) support dyslexic readers by default. Do not use justified text. Avoid italics for anything longer than a word or short phrase. Where a typeface choice is available, prefer fonts with clear letter differentiation (e.g. avoid fonts where "l", "1", and "I" are near-identical).

### 13.9 Motion

Respect the user's operating system setting for reduced motion. Animations and transitions must be disabled or significantly reduced when `prefers-reduced-motion: reduce` is set. Do not use motion that flashes more than three times per second.

---

## 14. Motion and animation

### 14.1 Purpose

Animation must serve a functional purpose: confirming an action, communicating a state change, guiding attention, or showing the relationship between two states. Do not use animation for decoration alone.

### 14.2 Duration

Standard UI transitions (button state changes, panel open/close, tooltip appear): 150–250ms. Page-level transitions or content entering the viewport: 250–400ms. Do not exceed 500ms for any UI animation — longer durations feel sluggish.

### 14.3 Easing

Use ease-out for elements entering the screen (fast start, gentle finish). Use ease-in for elements leaving the screen. Use ease-in-out for elements moving from one position to another. Avoid linear easing for UI animations — it feels mechanical.

### 14.4 Reduced motion

As stated in section 13.9: respect `prefers-reduced-motion`.

---

## 15. AI-generated content in UI

This section applies to any interface that displays content generated by a language model or AI system, including summaries, suggestions, classifications, and conversational responses.

### 15.1 Loading and streaming

AI responses that stream token-by-token must show a visible streaming indicator (e.g. a blinking cursor or animated ellipsis) from the moment generation starts. Do not show a blank area while waiting for the first token.

If a response will take more than two seconds to begin, show a skeleton placeholder of the expected response area immediately.

### 15.2 Distinguishing AI content from user or system content

AI-generated content must be visually distinguishable from human-authored content and from system data. Use a consistent label, icon, or visual treatment across the application. Users must always know when they are reading AI output.

### 15.3 Confidence and uncertainty

Where an AI system produces a classification or recommendation with variable confidence, the interface should reflect this. Low-confidence outputs should be presented with appropriate hedging language and visual treatment. Do not present uncertain outputs with the same visual weight as definitive data.

### 15.4 Editability

Where a user is expected to review and potentially modify AI-generated content, the interface must make this edit pathway obvious. Do not present AI output in a read-only style if editing is intended.

### 15.5 Error handling

AI generation can fail or time out. These failures must be handled as standard errors (section 4): show a clear message, offer a retry action, and do not leave the user in a broken state.

### 15.6 Actions taken by AI agents

If an AI agent is taking actions on behalf of the user (submitting forms, updating records, sending messages), the interface must provide a visible log or summary of what was done. Users must be able to review and undo agent actions where feasible. Never perform irreversible agent actions without explicit user confirmation.

---

## 16. Responsive design and layout

### 16.1 Mobile-first approach

Design for the smallest relevant screen size first, then expand to larger viewports. This produces better prioritisation of content and avoids the common failure of desktop layouts that break on mobile.

### 16.2 Breakpoints

Use Tailwind's default breakpoint scale as the standard across all Sauce projects, extended with a Sauce-standard `xs` breakpoint for small screen layout tweaks:

| Prefix | Min-width | Typical target                |
| ------ | --------- | ----------------------------- |
| `xs`   | 375px     | Small mobile (Sauce addition) |
| `sm`   | 640px     | Large mobile / small tablet   |
| `md`   | 768px     | Tablet                        |
| `lg`   | 1024px    | Desktop                       |
| `xl`   | 1280px    | Wide desktop                  |
| `2xl`  | 1536px    | Large desktop                 |

Add `xs` to the Tailwind config on every project:

```js
// tailwind.config.js
theme: {
  screens: {
    xs: '375px',
    sm: '640px',
    // ... rest of defaults
  }
}
```

Do not define arbitrary one-off breakpoints in component styles. If a layout genuinely requires a breakpoint outside this scale, add it to the Tailwind config so it is available consistently across the project.

Design to a minimum viewport width of 375px. If a product has a known audience on older devices or has accessibility requirements that push the effective viewport narrower, reduce the minimum to 320px — but this should be an explicit decision, not a default.

### 16.3 Layout reflow

At mobile sizes, multi-column layouts must reflow to single columns. Horizontal scrolling must not be introduced by layout changes at any breakpoint — it is only acceptable in explicitly scrollable data components (e.g. wide tables).

### 16.4 Navigation patterns

Navigation patterns must adapt across breakpoints. A horizontal top nav suitable for desktop typically requires a different pattern on mobile (bottom nav bar, hamburger menu, or drawer). Choose the pattern appropriate to the navigation depth and frequency of use.

### 16.5 Data tables on small screens

Full data tables should not be forced into small viewports unchanged. Options include: horizontal scroll within a bounded container, card-based row layout, column prioritisation (hide lower-priority columns at smaller sizes), and dedicated mobile views for key workflows.

---

## 17. Performance and perceived performance

### 17.1 Perceived performance over raw performance

Interfaces that feel fast are more important than interfaces that are fast by raw metrics. Prioritise: showing something immediately, giving feedback on actions without delay, and loading critical content before secondary content.

### 17.2 Optimistic updates

As described in section 3.5: apply non-destructive action results immediately, confirm with the server in the background.

### 17.3 Progressive loading

Load and render the most important content first. Defer secondary content (related items, analytics, supplementary panels) until primary content is visible and interactive.

### 17.4 Avoid layout shift

Do not load content in a way that causes the page layout to jump after render. Reserve space for images, embeds, and async content before it loads.

---

## 18. Content and language standards

These standards apply to all user-facing text: labels, messages, tooltips, empty states, errors, notifications, and documentation within the UI.

### 18.1 Plain English

Write for the user, not for the system. Assume no technical knowledge unless the product is specifically for a technical audience. Avoid jargon, acronyms, and domain-specific terms unless the user's domain requires them.

### 18.2 Active voice

Use active voice. Active voice is shorter and clearer.
**Good:** "We couldn't load your data."  
**Bad:** "Your data could not be loaded."

### 18.3 Sentence case

Use sentence case for all UI text: labels, headings, button text, messages. Title case should be used only for proper nouns.

**Good:** "Add new customer"  
**Bad:** "Add New Customer"

### 18.4 Consistency in terminology

Use the same term for the same concept throughout the application. If a record is called a "job" in one screen, it must not be called a "task" or "order" in another unless they are genuinely different things. Maintain a terminology reference for each product.

### 18.5 Microcopy

Notification messages, confirmation prompts, and empty states must be written to the same standard as the rest of the product. These are not low-priority afterthoughts — they are often the text a user reads most frequently.

---

## 19. Notification and feedback patterns

### 19.1 Toast notifications

Use toast notifications for brief, non-blocking feedback on completed actions: "Changes saved", "Record deleted", "Email sent". Toasts must auto-dismiss after a short duration (3–5 seconds). They must not contain actions that are critical to the user's workflow — if the user might need to act on a notification, use a persistent alert instead.

### 19.2 Persistent alerts

Use persistent alerts for information the user must see and acknowledge before continuing, or for ongoing status conditions (a system in degraded mode, an integration that is disconnected). Persistent alerts must include a dismiss mechanism unless the underlying condition must be resolved before the product can function.

### 19.3 Notification placement

Toasts should appear in a consistent location across the application (typically top-right or bottom-centre). Multiple toasts must stack in a predictable direction and not overlap interactive content.

### 19.4 Destructive action confirmation

As covered in section 7.5: confirm before destructive actions, use specific button labels, do not default to the destructive option.

### 19.5 Print sheets

A page made to be printed (a booking sheet, an access code) is not the app page printed.

**19.5.1** It must open in a new tab (a link with `target="_blank"` and `rel="noopener"`, saying so in its accessible name) on a route outside the app shell: no navigation, header or sidebar.

**19.5.2** It must be black text on a white page whatever the app theme, including the page behind the sheet. It must not open the print dialog by itself; a visible Print button asks for it and is hidden when printing.

**19.5.3** It should fit one A4 page for a normal record, including a long company name, set with a named `@page` rule so other screens keep their own page. A very long free-text value (such as a note) is not cut off: it flows onto a second page, with the code and the first rows staying on page 1. Check by printing to PDF and counting pages.

**19.5.4** Codes (QR) must be drawn black on white, with print colour adjustment on so a browser's "background graphics" setting does not drop the tile.

---

## 20. Testing

Every interactive component must be testable in an automated way. This requires:

**20.1** Interactive elements must have stable, semantic identifiers — either native semantic HTML roles (button, input, link) or explicit test attributes (e.g. `data-testid`). Do not rely on class names or positional selectors in tests.

**20.2** Test identifiers must be consistent with design naming conventions. A component called "Save button" in design should have a test ID that reflects this, agreed between design and development before build.

**20.3** State changes (loading, error, empty, populated) must all be testable. Design must produce distinct, identifiable states for each.

---

## Appendix: Quick reference — must rules

The following is a consolidated list of all **must** requirements in this document.

| #    | Rule                                                                        |
| ---- | --------------------------------------------------------------------------- |
| 1.1  | Communicate system state at all times                                       |
| 2.1  | Set up alerting for dependency failures                                     |
| 2.2  | Inform users of system unavailability before they take an action            |
| 2.3  | Write error messages in plain English with a next step                      |
| 3.1  | Show a visible loading state whenever data is loading                       |
| 3.4  | Button enters loading state and is disabled during action                   |
| 4.1  | Provide a generic error screen for unhandled exceptions                     |
| 4.3  | Detect and communicate offline state                                        |
| 4.4  | Every error state must offer a recovery action                              |
| 5.0  | Empty states must explain why and what to do                                |
| 6.1  | Filtered views must visibly indicate active filters                         |
| 7.1  | All form fields must have visible labels                                    |
| 7.5  | Destructive actions must require explicit confirmation with specific labels |
| 7.7  | Preserve user input on failed form submission                               |
| 8.1  | Maximum one primary action per screen                                       |
| 9.1  | Tables must have clear column headers; units in headers not cells           |
| 9.5  | Status indicators must use colour and text label together                   |
| 10.2 | Define colours as semantic tokens, not raw hex values                       |
| 10.3 | All text must meet minimum contrast ratios                                  |
| 10.4 | Never use colour as the sole means of communication                         |
| 11.1 | Base font size 16px; no user-facing text below 12px                         |
| 11.4 | Left-align body text; do not justify                                        |
| 12.4 | Icon-only interactive controls must have accessible labels                  |
| 13.4 | Non-decorative images must have alt text                                    |
| 13.5 | All interactive elements must be keyboard accessible with visible focus     |
| 13.6 | Touch targets minimum 44×44px                                               |
| 13.9 | Respect prefers-reduced-motion                                              |
| 15.2 | AI-generated content must be visually distinguishable from other content    |
| 15.6 | AI agent actions must be logged; irreversible actions require confirmation  |
| 18.3 | Use sentence case for all UI text                                           |
| 18.4 | Use consistent terminology throughout the application                       |
| 20.1 | Interactive elements must have stable, semantic test identifiers            |
