# _template.md — Component file structure

Every component file follows this structure. Sections may be shortened or expanded based on the component's complexity, but all sections must be present.

---

```markdown
# [Component Name]

> One-line description of what this component is and its primary role in the interface.

---

## When to use
- Bullet list of appropriate use cases

## When not to use
- Bullet list of situations where a different component is more appropriate
- Include the recommended alternative

---

## Anatomy

Describe the parts that make up the component. Use a simple labelled list:

1. **[Part name]** — description and whether it is required or optional
2. **[Part name]** — description
3. ...

---

## Variants

List the standard variants this component supports. For each variant:

### [Variant name]
- **Purpose:** when to use this variant
- **Visual distinction:** how it differs from the default

---

## States

List all interactive states the component must support:

| State | Visual treatment | Notes |
|---|---|---|
| Default | ... | ... |
| Hover | ... | ... |
| Focus | ... | ... |
| Active / Pressed | ... | ... |
| Disabled | ... | ... |
| Loading | ... | (if applicable) |
| Error | ... | (if applicable) |

→ Cross-reference UX Standards sections where relevant (e.g. → UX Standards §3.4 for button loading states)

---

## Interaction spec

### Mouse / pointer
- Click, hover, drag behaviours

### Keyboard
- Key bindings, tab order, focus management

### Touch
- Tap targets, gestures, long-press behaviour

---

## Responsive behaviour

How the component adapts across breakpoints. Reference Tailwind's breakpoint scale:

| Breakpoint | Behaviour |
|---|---|
| `xs` (375px) | ... |
| `sm` (640px) | ... |
| `md` (768px) | ... |
| `lg` (1024px+) | ... |

→ UX Standards §16 for breakpoint scale and responsive rules

---

## Accessibility

- **Role / semantics:** required HTML element or ARIA role
- **Accessible name:** how the component gets its label
- **Keyboard nav:** tab order, key bindings, focus management
- **Screen reader:** announcements, live regions, state communication
- **WCAG criteria:** specific success criteria this component must satisfy

→ UX Standards §13 for baseline accessibility requirements

---

## Motion & animation

- Entry/exit transitions
- Micro-interactions (hover, state changes)
- Timing and easing values
- Reduced motion handling

→ UX Standards §14 for duration, easing, and reduced motion rules

---

## Common pitfalls

Numbered list of mistakes agents frequently make when building this component. Written as direct warnings:

1. **[Pitfall]** — why it's wrong and what to do instead
2. ...

---

## Tailwind notes

Structural guidance for implementing with Tailwind CSS. Not a copy-paste snippet — guidance on class patterns, spacing conventions, and token usage.

→ UX Standards §10.2 for semantic colour tokens
```
