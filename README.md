# Sauce UI Guidelines

A complete reference for building consistent, accessible web application interfaces across Sauce projects. Covers UX standards, component behaviour, brand guidelines, layout patterns, page templates, and project setup. Designed for consumption by AI coding agents building per-project UI systems.

## Why this exists

Every web application Sauce builds needs buttons, inputs, modals, tables, layouts, and page structures. Rather than maintaining a shared component library (which introduces coupling and maintenance overhead across independent client projects), this repository provides **specifications** — so agents building per-project libraries produce interfaces that are consistent in behaviour, accessibility, and interaction patterns, even though every project implements them independently.

## What this specifies and what it doesn't

These guidelines define **behaviour, structure, and standards** — not visual design.

**Specified:** UX rules, component anatomy, interactive states, keyboard and touch behaviour, accessibility requirements, responsive adaptation, layout patterns, page compositions, project setup workflow, and common mistakes to avoid.

**Not specified:** colours, border radii, spacing values, font choices, shadows, visual style. Each project is free to apply its own visual identity. The `brand-guidelines-spec.md` defines what a project's brand guidelines must contain so the visual layer can be applied consistently.

## How it works

### Getting started with a new project

Follow `project-setup-checklist.md` for the full step-by-step sequence:

1. **Audit the brand guidelines** — verify the project has the visual assets and decisions needed to build (→ `brand-guidelines-spec.md`)
2. **Set up the design token layer** — translate brand decisions into Tailwind config and CSS custom properties
3. **Create the dev showcase** — build `/dev` routes that demonstrate every component, layout, and page template in isolation. This forces components to be decoupled from business logic and gives the team a living reference
4. **Build the component library** — implement components to spec (→ `components/`)
5. **Build the layout shells** — implement app shell and content area patterns (→ `layout-patterns.md`)
6. **Build the page templates** — compose standard page types from layouts and components (→ `page-templates.md`)
7. **Begin feature development** — build features by composing existing library components, not from scratch

### During feature development

Once the library exists, these guidelines become a decision-making reference. Choose the right component, choose the right layout, compose them following the page templates, and extend the library (not feature code) when new variants are needed.

## Documents in this repository

| Document | Purpose |
|---|---|
| `ux-standards.md` | The authority on UX rules — error handling, colour, typography, accessibility, responsive design. All other documents cross-reference it |
| `brand-guidelines-spec.md` | Checklist for what a brand guideline must contain to be buildable |
| `layout-patterns.md` | Shell patterns (sidebar, top nav, bottom nav) and content area patterns (single column, dashboard grid, split view, etc.) |
| `page-templates.md` | Standard CRUD/admin page types — dashboard, list, create, detail, edit, delete, has-many list |
| `project-setup-checklist.md` | Step-by-step new project setup including the dev showcase requirement |
| `components/_index.md` | Master directory of all 41 component specs by category |
| `components/_template.md` | Standard structure every component file follows |
| `AGENTS.md` | Instructions for AI coding agents working with these guidelines |

Cross-references between documents use the format `→ UX Standards §X.X` and `→ document-name.md`.

## File structure

```
component-guidelines/
├── AGENTS.md                    # Instructions for AI coding agents
├── README.md                    # This file
├── ux-standards.md              # UX rules — the authority document
├── brand-guidelines-spec.md     # What a brand guideline must contain
├── layout-patterns.md           # Shell and content area layout patterns
├── page-templates.md            # Standard CRUD/admin page compositions
├── project-setup-checklist.md   # Step-by-step new project setup
│
└── components/                  # 41 component specs
    ├── _index.md                # Master directory by category
    ├── _template.md             # Standard structure for component files
    ├── button.md                # Core Inputs (12)
    ├── input.md
    ├── ...
    ├── table.md                 # Data Display (9)
    ├── ...
    ├── modal.md                 # Feedback & Overlays (7)
    ├── ...
    ├── bulk-action-bar.md       # Composite Patterns (6)
    ├── form.md
    └── ...
```

See `components/_index.md` for the full component directory by category.

## What each component file covers

Every file follows the same template (`components/_template.md`):

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

1. Copy `components/_template.md` to `components/component-name.md`
2. Fill in every section
3. Add it to `components/_index.md` under the appropriate category
4. Cross-reference the UX Standards wherever a rule applies
5. Include at least 4 common pitfalls

### Editing a component

1. Read the full file before changing it
2. Keep the section structure intact
3. Update cross-references if UX Standards section numbers have changed
4. Flag significant changes so other projects benefit

---

**Version:** 2.0
**Maintained by:** Sauce
