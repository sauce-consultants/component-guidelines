# AGENTS.md — Instructions for AI Coding Agents

## What this project is

This is Sauce's UI Component Guidelines — a directory of 40 common web application components. Each component has its own markdown file specifying how it should be designed, built, and behave.

**This is not a component library.** It is the reference used to build one. At the start of every project, a component library should be created following these guidelines. During feature development, this reference is used to decide which components to reach for and how they should behave.

## Companion document

The **Sauce UX Standards** document is the authority on rules. Component files cross-reference it using `→ UX Standards §X.X` notation. You must load and follow the UX Standards alongside any component file you reference. The two documents work as a pair — the UX Standards define the rules, the component files show how to apply those rules to each specific component.

If a component file and the UX Standards conflict, the UX Standards take precedence.

---

## Two phases of use

### Phase 1: Build the component library

At the start of a project, before feature work begins, build a project-specific component library using these guidelines as the specification.

1. **Identify required components.** Review `_index.md` and determine which components the project needs. Not every project needs all 40 — select the set that matches the product's scope
2. **Build each component to spec.** For every component selected:
   - Follow the **Anatomy** section for component structure
   - Implement all **Variants** the project requires
   - Implement every **State** listed in the state table — do not skip loading, error, empty, or disabled states
   - Follow the **Interaction spec** for mouse, keyboard, and touch behaviour
   - Apply the **Responsive behaviour** table across Tailwind breakpoints
   - Meet every requirement in the **Accessibility** section
   - Use the **Tailwind notes** section for structural CSS patterns
   - Use semantic colour tokens (`var(--color-status-error)`, etc.) as specified in UX Standards §10.2
3. **Review against pitfalls.** Before considering a component done, read its **Common pitfalls** section as a checklist. Every item is a known failure mode
4. **Test.** Verify keyboard navigation, screen reader announcements, responsive behaviour at `xs` through `lg`, and all interactive states

### Phase 2: Reference during feature development

Once the component library exists, these guidelines shift from build specs to decision-making references.

1. **Choose the right component.** Read the **When to use / When not to use** sections to confirm you're reaching for the correct component. These sections explain which component fits which situation and recommend alternatives when the current one isn't right
2. **Compose correctly.** When combining components (e.g. a form with inputs, selects, and a submit button), reference the composite pattern files (`form.md`, `search.md`) for layout and interaction conventions
3. **Don't deviate from the library.** If a feature seems to need a component variant that doesn't exist in the project library, check the guidelines first — the variant may already be specified. Build it into the library, then use it in the feature. Do not create one-off component implementations inside feature code
4. **Flag gaps.** If a feature needs a component not covered by these guidelines, flag it. It may warrant a new guideline file

---

## File structure

```
ui-components/
├── AGENTS.md           # You are here
├── README.md           # Human-facing project documentation
├── _index.md           # Master directory — all 40 components by category
├── _template.md        # Standard structure every component file follows
├── button.md           # One file per component (40 total)
├── input.md
├── modal.md
└── ...
```

## Conventions

### Cross-references

- UX Standards: `→ UX Standards §X.X`
- Other component files: `(see component-name.md)`
- Do not duplicate UX Standards rules — cross-reference them

### Requirement levels

Following the Sauce UX Standards convention:

- **must** — a standard. No exceptions
- **should** — a best practice. Apply unless explicitly prevented
- **could** — an enhancement. Apply when there is time and value

Within component files: **States** tables and **Accessibility** sections are must-level. **Common pitfalls** are must-avoid. **Motion & animation** is should-level except `prefers-reduced-motion` handling, which is must-level.

### Tailwind CSS

- Tailwind is the default CSS framework across Sauce projects
- Breakpoint scale: `xs` (375px), `sm` (640px), `md` (768px), `lg` (1024px), `xl` (1280px), `2xl` (1536px)
- Use semantic colour tokens for status and action colours, not raw palette classes → UX Standards §10.2
- Tailwind notes in each file provide structural guidance, not copy-paste snippets. Adapt to the project's design system

### Accessibility baseline

Every component must meet WCAG 2.1 AA as a minimum. Key universal requirements:

- All interactive elements keyboard accessible with visible focus → UX Standards §13.5
- Minimum 44×44px touch targets → UX Standards §13.6
- Colour never used as the sole means of communication → UX Standards §10.4
- Respect `prefers-reduced-motion` → UX Standards §13.9

---

## Adding a new component guideline

1. Copy `_template.md` to `component-name.md`
2. Fill in every section following the conventions above
3. Add the component to `_index.md` in the appropriate category
4. Cross-reference the UX Standards wherever a rule applies
5. Include at least 4 items in Common pitfalls

## Editing an existing guideline

1. Read the full file before making changes
2. Preserve the section structure — do not remove or reorder sections
3. Update cross-references if UX Standards section numbers have changed
4. If the change is significant, flag it so other projects benefit

## Tone and style

- Direct, declarative language. "The button must enter a loading state" — not suggestions
- Practical, not theoretical. What to build and what to avoid
- Opinionated where it matters. Make decisions so agents don't have to guess
- Cross-reference, don't duplicate. If the UX Standards cover it, point to it
