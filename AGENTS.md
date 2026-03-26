# AGENTS.md — Instructions for AI Coding Agents

## What this project is

This is Sauce's UI Guidelines — a complete reference for building consistent, accessible web application interfaces. It covers component behaviour, UX standards, brand guidelines, layout patterns, page templates, and project setup.

**This is not a component library.** It is the reference used to build one. At the start of every project, follow the project setup checklist (`project-setup-checklist.md`) to create a project-specific component library, layout system, and dev showcase using these guidelines as the specification.

## Document hierarchy

These documents work together. Load the relevant ones for the task at hand.

| Document | Purpose | When to load |
|---|---|---|
| `ux-standards.md` | The authority on UX rules. All other documents cross-reference it | Always — this is the foundation |
| `project-setup-checklist.md` | Step-by-step sequence for setting up a new project's UI foundation | At project start |
| `brand-guidelines-spec.md` | What a brand guideline must contain to be buildable | When auditing or creating brand guidelines |
| `layout-patterns.md` | Shell patterns and content area patterns for page structure | When choosing or building layouts |
| `page-templates.md` | Standard CRUD/admin page types with component composition | When building pages |
| `components/_index.md` | Master directory of all component specs | When identifying or looking up components |
| `components/*.md` | Individual component specs (anatomy, states, interaction, a11y) | When building or reviewing a specific component |

If any document conflicts with the UX Standards, the UX Standards take precedence.

---

## How to use these guidelines

### Starting a new project

Follow `project-setup-checklist.md`. It walks through the full sequence:

1. Audit the brand guidelines → `brand-guidelines-spec.md`
2. Set up the design token layer
3. Create the dev showcase routes (`/dev/*`) — components must be built in isolation, decoupled from business logic
4. Build the component library → `components/_index.md` and individual spec files
5. Build the layout shells → `layout-patterns.md`
6. Build the page templates → `page-templates.md`
7. Begin feature development

Do not skip to step 7. The dev showcase (step 3) is mandatory — every component must be demonstrated there before it is used in feature code.

### Building a component

For every component selected from the index:

1. Follow the **Anatomy** section for component structure
2. Implement all **Variants** the project requires
3. Implement every **State** listed in the state table — do not skip loading, error, empty, or disabled states
4. Follow the **Interaction spec** for mouse, keyboard, and touch behaviour
5. Apply the **Responsive behaviour** table across Tailwind breakpoints
6. Meet every requirement in the **Accessibility** section
7. Use the **Tailwind notes** section for structural CSS patterns
8. Use semantic colour tokens (`var(--color-status-error)`, etc.) as specified in UX Standards §10.2
9. Review against the **Common pitfalls** section as a checklist — every item is a known failure mode
10. Add the component to the dev showcase with all variants and states
11. Test keyboard navigation, screen reader announcements, responsive behaviour at `xs` through `lg`, and all interactive states

### Building a page

Reference `page-templates.md` for standard CRUD/admin page types. Each template specifies:

- Which layout pattern to use (→ `layout-patterns.md`)
- The component composition (which components, in what order)
- Every page state (loading, empty, error, populated)
- Key interactions and navigation flows

### During feature development

Once the component library, layouts, and page templates exist, these guidelines shift to decision-making references.

1. **Choose the right component.** Read the **When to use / When not to use** sections to confirm you're reaching for the correct component
2. **Choose the right layout.** Reference `layout-patterns.md` for shell and content area pattern selection
3. **Compose correctly.** Reference `page-templates.md` and the composite pattern files (`form.md`, `search.md`) for layout and interaction conventions
4. **Don't deviate from the library.** If a feature needs a component variant that doesn't exist in the project library, check the guidelines first — the variant may already be specified. Build it into the library and the dev showcase, then use it in the feature. Do not create one-off component implementations inside feature code
5. **Flag gaps.** If a feature needs a component not covered by these guidelines, flag it. It may warrant a new guideline file

---

## File structure

```
component-guidelines/
├── AGENTS.md                    # You are here
├── README.md                    # Human-facing project documentation
├── ux-standards.md              # UX rules — the authority document
├── brand-guidelines-spec.md     # What a brand guideline must contain
├── layout-patterns.md           # Shell and content area layout patterns
├── page-templates.md            # Standard CRUD/admin page compositions
├── project-setup-checklist.md   # Step-by-step new project setup
│
└── components/
    ├── _index.md                # Master directory — all components by category
    ├── _template.md             # Standard structure every component file follows
    ├── button.md                # One file per component (41 total)
    ├── input.md
    ├── modal.md
    ├── bulk-action-bar.md
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
