# UI Component Guidelines — Index

**Version:** 1.0  
**Maintained by:** Sauce  
**Companion document:** Sauce UX Standards v2.0

---

## Purpose

This is a directory of common UI components required for most web application systems. Each component has its own file containing design, interaction, responsive, accessibility, and implementation guidance.

These guidelines are **not a shared component library**. They instruct agents on how to build per-project toolkits that are consistent in quality, behaviour, and accessibility — even though implementations are independent.

## How to use

- **At project setup (Finding a Table / Ordering):** Review the index to identify which components the project will need. Load relevant component files into Table Memory.
- **At Mise en Place:** Pull the component files relevant to the current dish. Cross-reference with the Sauce UX Standards for rules on states, errors, colour, typography, and accessibility.
- **At Cooking:** Follow the specs in each component file. Use the Tailwind Notes section for structural CSS guidance.
- **At Clearing:** Flag any component-level learnings for Kitchen Memory.

## Companion document

The **Sauce UX Standards** define the rules. These component files show how to build to those rules for each specific component. Cross-references to UX Standards sections are marked as `→ UX Standards §X.X` throughout.

---

## Component directory

### Core Inputs
| Component | File | Summary |
|---|---|---|
| Button | `button.md` | Primary, secondary, ghost, destructive, icon, loading |
| Icon Button | `icon-button.md` | Compact action triggers with mandatory accessible labels |
| Input | `input.md` | Single-line text entry with labels, validation, affixes |
| Textarea | `textarea.md` | Multi-line text entry with optional auto-resize |
| Select | `select.md` | Single-value dropdown selection |
| Combobox | `combobox.md` | Searchable select with autocomplete |
| Checkbox | `checkbox.md` | Binary or multi-select toggle with indeterminate state |
| Radio Group | `radio-group.md` | Single selection from a visible set of options |
| Toggle / Switch | `toggle.md` | Immediate on/off control for settings |
| Slider | `slider.md` | Continuous or stepped value selection within a range |
| Date Picker | `date-picker.md` | Calendar-based date and date-range selection |
| File Upload | `file-upload.md` | File selection via click or drag-and-drop |

### Layout & Navigation
| Component | File | Summary |
|---|---|---|
| Header / Navbar | `header.md` | Top-level navigation bar with responsive adaptation |
| Sidebar | `sidebar.md` | Vertical navigation panel, collapsible on smaller screens |
| Breadcrumbs | `breadcrumbs.md` | Hierarchical location indicator |
| Tabs | `tabs.md` | Content switching within a single view |
| Pagination | `pagination.md` | Page-based navigation for datasets |
| Stepper / Wizard | `stepper.md` | Multi-step process indicator and navigation |
| Footer | `footer.md` | Page-level footer with links and metadata |

### Data Display
| Component | File | Summary |
|---|---|---|
| Table / Data Grid | `table.md` | Structured data display with sorting, filtering, row actions |
| Card | `card.md` | Contained content block for previews and summaries |
| Accordion | `accordion.md` | Collapsible content sections |
| Badge / Tag | `badge.md` | Small labelling elements for status, categories, counts |
| Avatar | `avatar.md` | User or entity identity indicator |
| Tooltip | `tooltip.md` | Contextual hint on hover/focus |
| Stat / KPI Card | `stat-card.md` | Key metric display with label, value, and optional trend |
| Timeline | `timeline.md` | Chronological event sequence display |
| Empty State | `empty-state.md` | Placeholder content for views with no data |

### Feedback & Overlays
| Component | File | Summary |
|---|---|---|
| Modal / Dialog | `modal.md` | Focused overlay requiring user attention or input |
| Slide Panel / Drawer | `slide-panel.md` | Side-anchored overlay for detail views or forms |
| Toast / Notification | `toast.md` | Brief, auto-dismissing feedback message |
| Alert / Banner | `alert.md` | Persistent inline message for warnings, errors, or info |
| Progress Bar | `progress-bar.md` | Determinate or indeterminate progress indicator |
| Skeleton Loader | `skeleton.md` | Placeholder shapes shown while content loads |
| Spinner | `spinner.md` | Indeterminate loading indicator for small regions |

### Composite Patterns
| Component | File | Summary |
|---|---|---|
| Bulk Action Bar | `bulk-action-bar.md` | Contextual toolbar for multi-select actions on tables and lists |
| Command Palette | `command-palette.md` | Keyboard-driven search and action launcher |
| Dropdown Menu | `dropdown-menu.md` | Action list triggered from a button |
| Context Menu | `context-menu.md` | Right-click or long-press action list |
| Form (pattern) | `form.md` | Layout, validation, and submission patterns for forms |
| Search | `search.md` | Search input with results, filtering, and keyboard nav |

---

## Template

See `_template.md` for the standard structure every component file follows.
