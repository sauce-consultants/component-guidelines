# Sauce UI Component Guidelines

A directory of 40 common UI components specifying how each should be structured, behave, and meet accessibility standards across Sauce projects. Designed for consumption by AI coding agents building per-project component libraries.

## Why this exists

Every web application Sauce builds needs buttons, inputs, modals, tables, and dozens of other components. Rather than maintaining a shared component library (which introduces coupling and maintenance overhead across independent client projects), this repository provides **specifications** — so agents building per-project libraries produce components that are consistent in behaviour, accessibility, and interaction patterns, even though every project implements them independently.

Think of it as Kitchen Memory for UI.

## What this specifies and what it doesn't

These guidelines define **behaviour, structure, and standards** — not visual design.

**Specified:** component anatomy, interactive states, keyboard and touch behaviour, accessibility requirements, responsive adaptation, common mistakes to avoid.

**Not specified:** colours, border radii, spacing values, font choices, shadows, visual style. Each project is free to implement every component with its own visual identity. A button on one project may look completely different from a button on another — what matters is that both have the correct states, keyboard support, ARIA roles, and interaction patterns described in these files.

The Tailwind notes in each file provide structural CSS patterns (layout, positioning, transitions) rather than visual styling. Adapt them to the project's design system.

## How it works

### Phase 1: Build a component library

At the start of a project, before feature work begins, use these guidelines to build a project-specific component library.

1. Review `_index.md` and select the components the project needs — not every project needs all 40
2. Build each component to spec, following the anatomy, states, interaction, responsive, and accessibility sections
3. Apply the project's own visual design — colours, typography, spacing, elevation, border treatments
4. Review against the Common pitfalls section before considering a component done

### Phase 2: Reference during feature development

Once the library exists, these guidelines become a decision-making reference.

1. Check the **When to use / When not to use** sections to pick the right component for the job
2. Reference composite pattern files (`form.md`, `search.md`) when combining components
3. If a feature needs a variant that doesn't exist in the library, check the guidelines — the variant may already be specified. Build it into the library first, then use it in the feature

## Companion document

These component files work alongside the **Sauce UX Standards** (v2.0). The UX Standards define the rules (error handling, colour usage, accessibility baselines, typography, responsive design). The component files show how to apply those rules to each specific component.

Cross-references use the format `→ UX Standards §X.X` throughout.

## File structure

```
ui-components/
├── AGENTS.md           # Instructions for AI coding agents
├── README.md           # This file
├── _index.md           # Master directory — all 40 components by category
├── _template.md        # Standard structure every component file follows
│
├── button.md           # Core Inputs (12)
├── icon-button.md
├── input.md
├── textarea.md
├── select.md
├── combobox.md
├── checkbox.md
├── radio-group.md
├── toggle.md
├── slider.md
├── date-picker.md
├── file-upload.md
│
├── header.md           # Layout & Navigation (7)
├── sidebar.md
├── breadcrumbs.md
├── tabs.md
├── pagination.md
├── stepper.md
├── footer.md
│
├── table.md            # Data Display (9)
├── card.md
├── accordion.md
├── badge.md
├── avatar.md
├── tooltip.md
├── stat-card.md
├── timeline.md
├── empty-state.md
│
├── modal.md            # Feedback & Overlays (7)
├── slide-panel.md
├── toast.md
├── alert.md
├── progress-bar.md
├── skeleton.md
├── spinner.md
│
├── command-palette.md  # Composite Patterns (5)
├── dropdown-menu.md
├── context-menu.md
├── form.md
└── search.md
```

## What each component file covers

Every file follows the same template (`_template.md`):

| Section                   | What it covers                                             |
| ------------------------- | ---------------------------------------------------------- |
| **When to use / not use** | Appropriate contexts and recommended alternatives          |
| **Anatomy**               | The structural parts that make up the component            |
| **Variants**              | Standard variations (e.g. primary, secondary, destructive) |
| **States**                | All interactive states the component must support          |
| **Interaction spec**      | Mouse, keyboard, and touch behaviour                       |
| **Responsive behaviour**  | Adaptation across Tailwind breakpoints                     |
| **Accessibility**         | ARIA roles, keyboard nav, screen reader, WCAG criteria     |
| **Motion & animation**    | Transitions, timing, reduced motion handling               |
| **Common pitfalls**       | Known failure modes — written as warnings                  |
| **Tailwind notes**        | Structural CSS patterns (not visual styling)               |

## Requirement levels

Following the Sauce UX Standards convention:

- **must** — a standard. No exceptions
- **should** — a best practice. Apply unless client requirements explicitly prevent it
- **could** — an enhancement. Apply when there is time and clear value

## Tech defaults

- **CSS framework:** Tailwind CSS
- **Breakpoints:** Tailwind default scale + Sauce `xs` (375px) addition
- **Colour tokens:** semantic CSS custom properties → UX Standards §10.2
- **Accessibility target:** WCAG 2.1 AA minimum
- **Framework:** stack-agnostic. Guidelines apply to React, Vue, or vanilla HTML/CSS
- **Visual design:** per-project. These guidelines do not prescribe visual style

## Contributing

### Adding a component

1. Copy `_template.md` to `component-name.md`
2. Fill in every section
3. Add it to `_index.md` under the appropriate category
4. Include at least 4 common pitfalls

### Editing a component

1. Read the full file before changing it
2. Keep the section structure intact
3. Update cross-references if UX Standards section numbers have changed
4. Flag significant changes so other projects benefit

---

**Version:** 1.0
**Maintained by:** Sauce
