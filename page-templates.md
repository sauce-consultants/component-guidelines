# Page templates — common admin/CRUD pages

> A guide to composing layouts and components into standard admin page types. This is the connective tissue between the layout patterns (which define structure) and the component specs (which define behaviour). When building a page, start here.

**Version:** 1.0
**Status:** Active

---

## How to use this document

Each template describes:

- **Layout** — which shell and content area pattern to use (→ layout-patterns.md)
- **Anatomy** — the specific component composition, top to bottom
- **Page states** — every state the page must handle (loading, empty, error, populated)
- **Interactions** — key user flows on the page
- **Component checklist** — the components needed, linked to their spec files

Templates assume a **sidebar app shell** as the outer wrapper, since this is the standard for admin products. Adapt if your product uses a different shell.

Templates describe structure and behaviour, not visual design. Apply the project's brand guidelines for colours, typography, spacing, and elevation.

---

## Shared page elements

These elements appear across most or all admin pages. They are described once here rather than repeated in every template.

### Page header

Every page inside the app shell has a page header at the top of the content region. It provides context and page-level actions.

**Anatomy:**

1. **Breadcrumbs** (optional) — show the user's position in the hierarchy (→ breadcrumbs.md). Use when the page is more than one level deep
2. **Page title** — the name of the current page. Use sentence case (→ UX Standards §18.3). On resource pages, this is typically the resource type ("Customers") or the specific resource name ("Acme Corp")
3. **Description** (optional) — a short line below the title explaining the page's purpose. Use on pages where the title alone is ambiguous
4. **Primary action** — the main action for this page, right-aligned. At most one primary button per page (→ UX Standards §8.1, → button.md). Example: "Add customer", "Save changes"
5. **Secondary actions** (optional) — additional actions, visually subordinate. May be individual buttons or grouped in a dropdown menu (→ dropdown-menu.md)

```
┌─────────────────────────────────────────────────────┐
│ Home / Customers / Acme Corp                        │
│                                                     │
│ Acme Corp                          [Edit] [Archive] │
│ Customer since 14 Mar 2024                          │
└─────────────────────────────────────────────────────┘
```

**Responsive:** At `xs`–`sm`, secondary actions collapse into a dropdown menu. The primary action may move below the title if horizontal space is tight.

### Page states

Every page must handle these states. Templates below specify what each state looks like for that page type, but the principle is universal.

| State         | Requirement                                                                                                                                                 |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Loading**   | Show skeleton placeholders in the shape of the expected content (→ UX Standards §3.2, → skeleton.md). Never show a blank page                               |
| **Error**     | Show an inline error with a retry action (→ UX Standards §4.2, → alert.md). If the entire page fails, show the full-page error screen (→ UX Standards §4.1) |
| **Empty**     | Explain why there is no data and what the user can do about it (→ UX Standards §5, → empty-state.md)                                                        |
| **Populated** | The normal state. Show the data                                                                                                                             |
| **Offline**   | Show a persistent banner indicating loss of connectivity (→ UX Standards §4.3)                                                                              |

---

## Template 1: Home / dashboard

The landing page after login. Gives the user a high-level overview of what matters right now.

### Layout

- **Shell:** Sidebar app shell
- **Content area pattern:** Dashboard grid (→ layout-patterns.md, Content 2)

### Anatomy

```
┌──────┬─────────────────────────────────────────────┐
│      │ Dashboard              [Date range] [Export]│
│      ├─────────────────────────────────────────────┤
│      │ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ │
│ Side │ │ Stat 1 │ │ Stat 2 │ │ Stat 3 │ │ Stat 4 │ │
│ bar  │ └────────┘ └────────┘ └────────┘ └────────┘ │
│      │ ┌───────────────────┐ ┌───────────────────┐ │
│      │ │                   │ │                   │ │
│      │ │   Chart/graph     │ │   Recent items    │ │
│      │ │                   │ │   table           │ │
│      │ └───────────────────┘ └───────────────────┘ │
│      │ ┌─────────────────────────────────────────┐ │
│      │ │        Activity / timeline              │ │
│      │ └─────────────────────────────────────────┘ │
└──────┴─────────────────────────────────────────────┘
```

1. **Page header** — title ("Dashboard" or product-specific name), optional date range selector, optional export action
2. **Stat cards row** — 3–5 key metrics (→ stat-card.md). These are the numbers the user checks first
3. **Widget grid** — charts, tables, lists, and summaries arranged in the dashboard grid layout. Each widget is a card (→ card.md) containing its own content
4. **Activity feed** (optional) — recent actions or events (→ timeline.md)

### Page states

| State                 | Treatment                                                                                                                                                                                             |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Loading               | Each widget shows its own skeleton independently (→ UX Standards §3.3). Stat cards show skeleton numbers. Chart areas show skeleton rectangles. Do not hold the entire page blank for one slow widget |
| Error (single widget) | The widget shows an inline error with retry, while other widgets render normally                                                                                                                      |
| Error (full page)     | Full-page error screen with retry                                                                                                                                                                     |
| Empty (new account)   | Friendly empty state explaining the dashboard will populate as they use the product. Provide a quick-start action: "Add your first [resource]"                                                        |
| Populated             | Normal state                                                                                                                                                                                          |

### Interactions

- **Date range selector** — filters all widgets to the selected period. Each widget re-fetches independently. Show loading state per widget during refresh
- **Stat card click** — optionally navigates to the relevant list page, pre-filtered
- **Widget actions** — individual widgets may have their own actions (expand, export, view all) in a card header

### Component checklist

| Component     | Spec             | Notes                         |
| ------------- | ---------------- | ----------------------------- |
| Stat card     | stat-card.md     | 3–5 across the top            |
| Card          | card.md          | Wrapper for each widget       |
| Table         | table.md         | For "recent items" widgets    |
| Skeleton      | skeleton.md      | Per-widget loading            |
| Empty state   | empty-state.md   | New account / no data         |
| Alert         | alert.md         | Widget-level errors           |
| Date picker   | date-picker.md   | Date range filter (if needed) |
| Dropdown menu | dropdown-menu.md | Export/actions menu           |

---

## Template 2: Resource list / table page

The primary page for browsing, searching, filtering, and acting on a collection of resources. This is the most-used page type in most admin products.

### Layout

- **Shell:** Sidebar app shell
- **Content area pattern:** Single column, wide (→ layout-patterns.md, Content 1). The table needs full available width

### Anatomy

```
┌──────┬─────────────────────────────────────────────┐
│      │ Customers                    [Add customer] │
│      ├─────────────────────────────────────────────┤
│      │ [Search...         ] [Filters ▼]            │
│      │                                             │
│      │ ☐ │ Name  ↕ │ Email      ↕ │ Status │ ···   │
│ Side │ ──┼─────────┼──────────────┼────────┼─────  │
│ bar  │ ☐ │ Acme    │ a@acme.com   │ Active │ •••   │
│      │ ☐ │ Beta Co │ b@beta.com   │ Active │ •••   │
│      │ ☐ │ Gamma   │ g@gamma.com  │ Paused │ •••   │
│      │                                             │
│      │ ← 1 2 3 ... 12 →    Showing 1–25 of 286     │
└──────┴─────────────────────────────────────────────┘
```

1. **Page header** — resource type as title (plural: "Customers"), primary action to create a new resource
2. **Filter bar** — search input (→ search.md) and filter controls. For simple filter sets (1–3 filters), show controls inline in the bar (→ select.md or dropdown). For complex filter sets (4+ filters, date ranges, multi-select), show a filter toggle button that opens a modal or slide panel containing the full filter form (→ modal.md, → slide-panel.md). The toggle should show a count of active filters (e.g. "Filters (3)"). When filters are active, the user must be able to see what is filtered and clear individual or all filters (→ UX Standards §6.1, §6.2). How active filters are displayed is a project-level decision — options include filter chips, a summary banner above or within the table, or inline indicators. Choose one approach and apply it consistently
3. **Bulk action bar** — appears when rows are selected. Shows selected count and bulk actions (→ UX Standards §9.7)
4. **Data table** — the resource list (→ table.md). Columns appropriate to the resource type. Row actions via a three-dot menu (→ context-menu.md or dropdown-menu.md, → UX Standards §9.4)
5. **Pagination** — below the table (→ pagination.md). Shows current range and total count (→ UX Standards §9.3)

### Page states

| State                              | Treatment                                                                                                                                                                                                        |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Loading (initial)                  | Table skeleton: header row visible, body rows as skeleton lines (→ skeleton.md). Filter bar is interactive but disabled until data loads                                                                         |
| Loading (filter/search)            | Previous results remain visible. A subtle loading indicator on the table (e.g. reduced opacity or a progress bar at the top of the table). Do not flash a full skeleton between keystrokes (→ UX Standards §3.3) |
| Empty (no resources exist)         | Full empty state in the table area: explain that no resources exist yet, provide the primary create action prominently (→ empty-state.md)                                                                        |
| Empty (filters produce no results) | Empty state explaining no results match the current filters. Show active filter summary and a "Clear all filters" action (→ UX Standards §6.3)                                                                   |
| Error                              | Inline error in the table area with retry. Filter bar remains visible and interactive                                                                                                                            |
| Populated                          | Normal state                                                                                                                                                                                                     |

### Interactions

- **Search** — filters the table. Debounce input (300ms). Show previous results until new results arrive (→ UX Standards §3.3)
- **Filter** — updates the table. Active filters must be visible and clearable (display approach is a project-level decision). URL should reflect current filters so the view is shareable/bookmarkable
- **Sort** — integrated into table column headers, not the filter bar. Clicking a column header toggles sort direction. The current sort column and direction must be visible via an indicator in the header (→ UX Standards §9.2). Not all columns need to be sortable — only columns where sorting is meaningful
- **Row click** — navigates to the resource detail page. The entire row is clickable, not just the name
- **Row actions (three-dot menu)** — quick actions without navigating: edit, archive, delete. Destructive actions require confirmation (→ UX Standards §7.5)
- **Bulk select** — checkbox in each row and a select-all in the header. Bulk action bar appears with selected count and available actions
- **Create** — primary button navigates to the create page or opens a modal/slide panel, depending on form complexity

### Component checklist

| Component     | Spec             | Notes                     |
| ------------- | ---------------- | ------------------------- |
| Table         | table.md         | Main content              |
| Search        | search.md        | Filter bar                |
| Select        | select.md        | Inline filter dropdowns   |
| Badge         | badge.md         | Status indicators         |
| Dropdown menu | dropdown-menu.md | Row actions, bulk actions |
| Pagination    | pagination.md    | Below table               |
| Skeleton      | skeleton.md      | Table loading state       |
| Empty state   | empty-state.md   | No data / no results      |
| Modal         | modal.md         | Delete confirmation       |
| Slide panel   | slide-panel.md   | Complex filter panel      |
| Button        | button.md        | Primary create action     |
| Checkbox      | checkbox.md      | Row selection             |

---

## Template 3: Resource create / new page

A form for creating a new resource. The user's goal is focused: fill in the details and submit.

### Layout

- **Shell:** Sidebar app shell
- **Content area pattern:** Single column (→ layout-patterns.md, Content 1) for simple forms. Main + aside (→ layout-patterns.md, Content 4) if the form benefits from contextual help, a live preview, or a summary panel

### Anatomy — single column

```
┌──────┬─────────────────────────────────────────────┐
│      │ ← Customers                                 │
│      │ Add customer                                │
│      ├─────────────────────────────────────────────┤
│      │                                             │
│      │  ┌─ Section 1 ───────────────────────────┐  │
│ Side │  │ Name        [                       ] │  │
│ bar  │  │ Email       [                       ] │  │
│      │  │ Phone       [                       ] │  │
│      │  └───────────────────────────────────────┘  │
│      │                                             │
│      │  ┌─ Section 2 ───────────────────────────┐  │
│      │  │ Address     [                       ] │  │
│      │  │ City        [            ]            │  │
│      │  └───────────────────────────────────────┘  │
│      │                                             │
│      │             [Cancel]  [Create customer]     │
│      │                                             │
└──────┴─────────────────────────────────────────────┘
```

### Anatomy — main + aside

```
┌──────┬──────────────────────────────┬──────────────┐
│      │ ← Customers                  │              │
│      │ Add customer                 │              │
│      ├──────────────────────────────┤   Preview    │
│ Side │                              │   or         │
│ bar  │  Form fields                 │   help       │
│      │  ...                         │   panel      │
│      │                              │              │
│      │  [Cancel]  [Create customer] │              │
└──────┴──────────────────────────────┴──────────────┘
```

1. **Page header** — breadcrumb back to the resource list ("← Customers"). Title: "Add [resource]". No primary action in the header — the submit button is in the form
2. **Form** — organised in logical sections with clear headings (→ form.md). Every field has a visible label (→ UX Standards §7.1). Required fields are marked (→ UX Standards §7.4)
3. **Form sections** — group related fields under section headings. Use cards or visual separators to distinguish sections in long forms
4. **Action bar** — at the bottom of the form. Primary action: "Create [resource]" (specific label, not "Submit" — → UX Standards §8.2). Secondary action: "Cancel" (returns to list)
5. **Aside** (if using main + aside) — live preview, contextual help, or a summary of what the user has entered so far

### Page states

| State                  | Treatment                                                                                                                                            |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Default                | Empty form, ready for input                                                                                                                          |
| Loading (dependencies) | If the form needs to load options (e.g. dropdown data from an API), show skeleton placeholders on affected fields. Other fields remain interactive   |
| Submitting             | Submit button enters loading state (→ UX Standards §3.4). Form fields disabled. Cancel button disabled                                               |
| Validation error       | Inline errors on affected fields (→ UX Standards §7.3). Scroll to the first error if it is off-screen. Preserve all user input (→ UX Standards §7.7) |
| Server error           | Inline alert above the action bar explaining the failure with a retry path. Preserve all user input                                                  |
| Success                | Navigate to the new resource's detail page, or back to the list. Show a toast confirming creation (→ toast.md)                                       |

### Interactions

- **Field validation** — validate on blur, not on keystroke (→ UX Standards §7.2)
- **Cancel** — if the form has unsaved changes, confirm before navigating away. Use a modal: "You have unsaved changes. Discard changes?" with "Keep editing" (default) and "Discard" (→ UX Standards §7.5)
- **Browser back / navigation** — same unsaved changes protection as cancel
- **Submit** — disable the button during submission. On success, navigate and show toast. On error, preserve input and show error

### Component checklist

| Component   | Spec           | Notes                        |
| ----------- | -------------- | ---------------------------- |
| Form        | form.md        | Overall form structure       |
| Input       | input.md       | Text fields                  |
| Textarea    | textarea.md    | Long text fields             |
| Select      | select.md      | Dropdown fields              |
| Combobox    | combobox.md    | Searchable dropdowns         |
| Checkbox    | checkbox.md    | Boolean fields               |
| Radio group | radio-group.md | Choice fields                |
| Date picker | date-picker.md | Date fields                  |
| File upload | file-upload.md | Attachment fields            |
| Button      | button.md      | Submit and cancel            |
| Toast       | toast.md       | Success confirmation         |
| Modal       | modal.md       | Unsaved changes confirmation |
| Alert       | alert.md       | Server error display         |
| Skeleton    | skeleton.md    | Dependency loading           |

---

## Template 4: Resource detail / dashboard page

The page for viewing a single resource in full. This is where the user goes after clicking a row in the list page. It shows all the resource's data and provides actions to operate on it.

### Layout

- **Shell:** Sidebar app shell
- **Content area pattern:** Main + aside (→ layout-patterns.md, Content 4) is the most common. Use single column if the resource has no meaningful sidebar content. Use dashboard grid if the resource has multiple data widgets (e.g. a customer's orders, activity, stats)

### Anatomy — main + aside

```
┌──────┬──────────────────────────────┬──────────────┐
│      │ ← Customers                  │              │
│      │ Acme Corp        [Edit] [···]│  Summary     │
│      ├──────────────────────────────┤  card        │
│      │                              │              │
│      │  ┌─ Details ──────────────┐  │  Status      │
│ Side │  │ Name: Acme Corp        │  │  badge       │
│ bar  │  │ Email: a@acme.com      │  │              │
│      │  │ Phone: +44 ...         │  │  Quick       │
│      │  └────────────────────────┘  │  actions     │
│      │                              │              │
│      │  ┌─ Recent orders ────────┐  │  Activity    │
│      │  │ (table or list)        │  │  feed        │
│      │  └────────────────────────┘  │              │
│      │                              │              │
└──────┴──────────────────────────────┴──────────────┘
```

### Anatomy — dashboard variant

```
┌──────┬─────────────────────────────────────────────┐
│      │ ← Customers                                 │
│      │ Acme Corp                    [Edit] [···]   │
│      ├─────────────────────────────────────────────┤
│      │ ┌────────┐ ┌────────┐ ┌────────┐            │
│ Side │ │ Orders │ │Revenue │ │ Tickets│            │
│ bar  │ └────────┘ └────────┘ └────────┘            │
│      │ ┌───────────────────┐ ┌───────────────────┐ │
│      │ │  Order history    │ │  Contact info     │ │
│      │ └───────────────────┘ └───────────────────┘ │
│      │ ┌─────────────────────────────────────────┐ │
│      │ │  Activity timeline                      │ │
│      │ └─────────────────────────────────────────┘ │
└──────┴─────────────────────────────────────────────┘
```

1. **Page header** — breadcrumb back to the list. Resource name as title. Primary action: "Edit" (navigates to edit page). Secondary actions in a dropdown: archive, delete, export, etc.
2. **Summary aside** (main + aside layout) — a sticky card showing key attributes at a glance: status badge, key dates, quick actions. This stays visible while the user scrolls the main content
3. **Detail sections** — grouped by domain. Each section is a card with a heading. Read-only field display (label: value pairs)
4. **Related resource tables** — recent orders, linked tickets, team members, etc. Shown as compact tables or lists within cards. Each has a "View all" link to the full related list page
5. **Activity feed** (optional) — timeline of actions taken on this resource (→ timeline.md)

### Page states

| State     | Treatment                                                                                                       |
| --------- | --------------------------------------------------------------------------------------------------------------- |
| Loading   | Skeleton in the shape of the expected layout. Summary aside skeleton loads alongside main content skeleton      |
| Error     | Full-page error if the resource cannot be loaded. Inline errors for individual sections that fail independently |
| Not found | Specific empty state: "This [resource] doesn't exist or has been deleted." Provide a link back to the list page |
| Populated | Normal state                                                                                                    |

### Interactions

- **Edit** — navigates to the edit page (Template 5) or opens a slide panel for quick edits of individual sections
- **Delete / archive** — from the secondary actions menu. Confirmation modal with specific description of what will happen (→ UX Standards §7.5)
- **Related resource links** — clicking a related item (e.g. an order) navigates to that resource's detail page
- **View all (related resources)** — navigates to the resource HasMany list page (Template 7), pre-filtered to this parent resource
- **Quick actions in aside** — common actions like "Send email", "Change status" can live in the aside for fast access

### Component checklist

| Component     | Spec             | Notes                          |
| ------------- | ---------------- | ------------------------------ |
| Card          | card.md          | Section containers             |
| Badge         | badge.md         | Status indicator               |
| Table         | table.md         | Related resource tables        |
| Timeline      | timeline.md      | Activity feed                  |
| Stat card     | stat-card.md     | Dashboard variant metrics      |
| Dropdown menu | dropdown-menu.md | Secondary actions              |
| Modal         | modal.md         | Delete/archive confirmation    |
| Toast         | toast.md         | Action confirmation            |
| Skeleton      | skeleton.md      | Loading state                  |
| Empty state   | empty-state.md   | Not found                      |
| Breadcrumbs   | breadcrumbs.md   | Navigation back to list        |
| Tooltip       | tooltip.md       | Truncated values, icon actions |

---

## Template 5: Resource edit / update page

A form for editing an existing resource. Structurally similar to the create page, but pre-populated with existing data and with different state handling.

### Layout

- **Shell:** Sidebar app shell
- **Content area pattern:** Same as create page — single column or main + aside (→ layout-patterns.md, Content 1 or Content 4)

### Anatomy

```
┌──────┬─────────────────────────────────────────────┐
│      │ ← Acme Corp                                  │
│      │ Edit customer                                │
│      ├─────────────────────────────────────────────┤
│      │                                             │
│      │  ┌─ Section 1 ───────────────────────────┐  │
│ Side │  │ Name        [Acme Corp              ] │  │
│ bar  │  │ Email       [a@acme.com             ] │  │
│      │  │ Phone       [+44 7700 900000        ] │  │
│      │  └───────────────────────────────────────┘  │
│      │                                             │
│      │             [Cancel]  [Save changes]        │
│      │                                             │
└──────┴─────────────────────────────────────────────┘
```

1. **Page header** — breadcrumb back to the resource detail page (not the list). Title: "Edit [resource]"
2. **Form** — identical structure to the create form, but pre-populated with the resource's current values
3. **Action bar** — primary action: "Save changes" (not "Update" or "Submit"). Secondary: "Cancel" (returns to detail page)

### Key differences from create page

- **Loading state** — the form must load the existing resource data before rendering. Show a full form skeleton until data arrives. Do not show an empty form that then fills in — this flickers and can cause the user to start typing in blank fields
- **Breadcrumb** — points back to the detail page, not the list page
- **Submit label** — "Save changes", not "Create [resource]"
- **Success navigation** — returns to the detail page, not the list
- **Dirty tracking** — track which fields have been modified. Only submit changed fields if the API supports partial updates. Disable the save button until at least one field has been modified

### Page states

| State                       | Treatment                                                                                                                                                                            |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Loading (fetching resource) | Full form skeleton. Do not show empty fields                                                                                                                                         |
| Default (data loaded)       | Form populated with current values. Save button disabled until a change is made                                                                                                      |
| Dirty (changes made)        | Save button enabled. Unsaved changes protection active on navigation                                                                                                                 |
| Submitting                  | Save button enters loading state. Form fields disabled                                                                                                                               |
| Validation error            | Same as create page — inline errors, scroll to first error, preserve input                                                                                                           |
| Server error                | Inline alert. Preserve user's changes                                                                                                                                                |
| Success                     | Navigate to detail page. Toast: "Changes saved"                                                                                                                                      |
| Conflict (409)              | Another user has modified the resource since it was loaded. Show an alert explaining the conflict and offer options: reload (losing changes), or force save (if the API supports it) |

### Interactions

Same as create page, plus:

- **Dirty tracking** — highlight modified fields with a subtle visual indicator (optional). Enable the save button only when changes exist
- **Conflict detection** — if the API provides an ETag or updated_at timestamp, compare on save. Handle 409 conflicts gracefully rather than silently overwriting

### Component checklist

Same as Template 3 (create page), plus:

| Component | Spec        | Notes                   |
| --------- | ----------- | ----------------------- |
| Skeleton  | skeleton.md | Full form loading state |
| Alert     | alert.md    | Conflict warning        |

---

## Template 6: Resource delete / archive page

Destructive actions are not typically full pages — they are confirmation flows triggered from a detail page, list page, or bulk action. This template covers the confirmation patterns rather than a standalone page.

### Pattern A: Confirmation modal (most common)

Use for single-resource deletion or archiving triggered from a detail page or row action.

```
┌─────────────────────────────────────┐
│                                     │
│  ┌───────────────────────────────┐  │
│  │ Delete customer               │  │
│  │                               │  │
│  │ Are you sure you want to      │  │
│  │ delete "Acme Corp"? This      │  │
│  │ will permanently remove the   │  │
│  │ customer and all associated   │  │
│  │ data.                         │  │
│  │                               │  │
│  │ This action cannot be undone. │  │
│  │                               │  │
│  │         [Cancel]  [Delete]    │  │
│  └───────────────────────────────┘  │
│                                     │
└─────────────────────────────────────┘
```

### Pattern B: Confirmation with input

Use when the destructive action is severe and you want to ensure the user is deliberate. Require them to type the resource name or a confirmation phrase.

```
┌───────────────────────────────────┐
│ Delete customer                   │
│                                   │
│ This will permanently delete      │
│ "Acme Corp" and all 47 orders,    │
│ 12 invoices, and 3 support        │
│ tickets associated with it.       │
│                                   │
│ Type "Acme Corp" to confirm:      │
│ [                              ]  │
│                                   │
│          [Cancel]  [Delete]       │
└───────────────────────────────────┘
```

### Pattern C: Bulk delete confirmation

Use when deleting or archiving multiple resources from a list page bulk action.

```
┌───────────────────────────────────┐
│ Delete 5 customers                │
│                                   │
│ Are you sure you want to delete   │
│ these 5 customers? This will      │
│ permanently remove them and all   │
│ associated data.                  │
│                                   │
│ • Acme Corp                       │
│ • Beta Co                         │
│ • Gamma Ltd                       │
│ • Delta Inc                       │
│ • Epsilon SA                      │
│                                   │
│          [Cancel]  [Delete 5]     │
└───────────────────────────────────┘
```

### Rules (all patterns)

These follow → UX Standards §7.5 and §19.4:

- **Title** — state the action: "Delete [resource]", "Archive [resource]"
- **Description** — explain specifically what will happen, including cascading effects ("and all 47 orders"). Do not use vague language
- **Consequences** — state whether the action is reversible. If it is (archive), say so: "You can restore this later from the archive." If it is not (delete), say so plainly: "This action cannot be undone"
- **Confirm button** — specific label matching the action: "Delete customer", "Archive 5 customers". Not "OK", "Yes", or "Confirm" (→ UX Standards §8.2). Use the destructive button variant (→ button.md)
- **Cancel button** — always present. Must be the default/focused action — the user should not accidentally confirm a destructive action (→ UX Standards §7.5)
- **Loading state** — confirm button enters loading state during the operation. Both buttons disabled. Modal stays open until the operation completes
- **Success** — close the modal. Show a toast confirming the action ("Customer deleted"). If triggered from the detail page, navigate back to the list. If from the list page, remove the row(s) from the table
- **Error** — keep the modal open. Show an inline error in the modal explaining the failure. "Cancel" still works

### Archive vs delete

If the product supports both soft delete (archive) and hard delete:

- **Archive** is the default destructive action. Use a warning-style confirmation, not a danger-style one. The consequence is reversible
- **Delete** is the nuclear option. Use danger-style confirmation. Consider requiring input confirmation (Pattern B). Only offer this in admin/settings contexts, not as a primary action

### Component checklist

| Component | Spec      | Notes                                                 |
| --------- | --------- | ----------------------------------------------------- |
| Modal     | modal.md  | Confirmation container                                |
| Button    | button.md | Destructive variant for confirm, secondary for cancel |
| Input     | input.md  | Pattern B: confirmation input                         |
| Toast     | toast.md  | Success feedback                                      |
| Alert     | alert.md  | Error within modal                                    |

---

## Template 7: Resource HasMany list / table page

A list page for resources that belong to a parent resource. Structurally similar to the main list page (Template 2), but scoped to a specific parent and with navigation context that reflects the relationship.

### Layout

- **Shell:** Sidebar app shell
- **Content area pattern:** Single column, wide (same as Template 2)

### Anatomy

```
┌──────┬───────────────────────────────────────────────┐
│      │ ← Acme Corp                                   │
│      │ Orders for Acme Corp         [Create order]   │
│      ├───────────────────────────────────────────────┤
│      │ [Search...         ] [Filters ▼]              │
│      │                                               │
│      │ ☐ │ Order # ↕ │ Date     ↕ │ Total  │ Status. │
│ Side │ ──┼───────────┼────────────┼────────┼──────── │
│ bar  │ ☐ │ #1042   │ 25 Mar 26  │ £1,200 │ Paid    │
│      │ ☐ │ #1039   │ 20 Mar 26  │ £800   │ Pending │
│      │ ☐ │ #1035   │ 14 Mar 26  │ £2,400 │ Paid    │
│      │                                               │
│      │ ← 1 2 3 →                Showing 1–25 of 67   │
└──────┴───────────────────────────────────────────────┘
```

1. **Page header** — breadcrumb back to the parent resource detail page ("← Acme Corp"). Title: "[Child resources] for [Parent name]" (e.g. "Orders for Acme Corp"). Primary action: "Create [child resource]" (if creation is allowed in this context)
2. **Filter bar** — same as Template 2, but filters are scoped to this parent's children. The parent filter is implicit — do not show a filter for "Customer: Acme Corp" since that is the entire page context
3. **Data table** — same as Template 2
4. **Pagination** — same as Template 2

### Key differences from the main list page (Template 2)

- **Scope is implicit.** The page only shows resources belonging to the parent. This scope must be clear from the page title and breadcrumb. Do not show a filter chip for the parent — it is not removable
- **Breadcrumb goes to the parent detail page**, not the main list. The user's context is "I am looking at Acme Corp's orders", not "I am looking at all orders, filtered to Acme Corp"
- **Create action** creates a child resource pre-associated with the parent. The create form should pre-fill and lock the parent field
- **Empty state** is specific: "Acme Corp has no orders yet" — not "No orders found"

### When to use this vs. filtering the main list

Use the HasMany page when:

- The user arrived from the parent detail page and their mental model is "this parent's children"
- The child resources have a clear ownership relationship (orders belong to a customer)
- The page title and breadcrumb should reflect the parent context

Use the main list page with a filter when:

- The user wants to browse all resources and optionally filter by parent
- The relationship is many-to-many or loose
- The filter is removable — the user may want to see all resources, not just one parent's

### Page states

Same as Template 2, with these adjustments:

| State                              | Treatment                                                                                      |
| ---------------------------------- | ---------------------------------------------------------------------------------------------- |
| Empty (parent has no children)     | "Acme Corp has no orders yet." with a create action                                            |
| Empty (filters produce no results) | Same as Template 2 — clear filters action                                                      |
| Parent not found                   | If the parent resource doesn't exist, show a not-found state with a link to the main list page |

### Interactions

Same as Template 2. Additionally:

- **Create child** — navigates to the create page with the parent pre-filled and locked
- **Breadcrumb "parent name"** — navigates back to the parent detail page

### Component checklist

Same as Template 2.

---

## Template relationships

These templates form a connected navigation flow. Understanding the relationships helps ensure consistent navigation and breadcrumbs.

```
                    ┌──────────────┐
                    │  Dashboard   │
                    │ (Template 1) │
                    └──────┬───────┘
                           │ sidebar nav
                           ▼
                    ┌──────────────┐
                    │ Resource     │
                ┌───│ List         │───────────────┐
                │   │ (Template 2) │               │
                │   └──────┬───────┘               │
                │          │ row click             │ create action
                │          ▼                       ▼
                │   ┌──────────────┐        ┌──────────────┐
                │   │ Resource     │───────▶│ Resource     │
                │   │ Detail       │ edit   │ Edit         │
                │   │ (Template 4) │◀───────│ (Template 5) │
                │   └──┬───┬───┬──┘ save    └──────────────┘
                │      │   │   │
         delete/│      │   │   │ view all
        archive │      │   │   │ related
                │      │   │   ▼
                ▼      │   │  ┌──────────────┐
        ┌────────────┐ │   │  │ HasMany      │
        │ Delete/    │ │   │  │ List         │
        │ Archive    │ │   │  │ (Template 7) │
        │(Template 6)│ │   │  └──────┬───────┘
        └────────────┘ │   │         │ create
                       │   │         ▼
                       │   │  ┌──────────────┐
                       │   │  │ Child Create │
                       │   │  │ (Template 3) │
                       │   │  └──────────────┘
                       │   │
                       │   │ create action
                       │   ▼
                       │  ┌──────────────┐
                       │  │ Resource     │
                       │  │ Create       │
                       │  │ (Template 3) │
                       │  └──────────────┘
                       │
                       │ stat click
                       ▼
                ┌──────────────┐
                │ Resource     │
                │ List         │ (pre-filtered)
                │ (Template 2) │
                └──────────────┘
```

### Breadcrumb patterns

| Page            | Breadcrumb                                             |
| --------------- | ------------------------------------------------------ |
| Dashboard       | (none — it is the root)                                |
| Resource list   | Dashboard > Customers                                  |
| Resource create | Dashboard > Customers > Add customer                   |
| Resource detail | Dashboard > Customers > Acme Corp                      |
| Resource edit   | Dashboard > Customers > Acme Corp > Edit               |
| HasMany list    | Dashboard > Customers > Acme Corp > Orders             |
| HasMany create  | Dashboard > Customers > Acme Corp > Orders > Add order |
| Delete/archive  | (modal — no breadcrumb)                                |

### URL patterns

Consistent URL structure reinforces the template hierarchy:

| Page            | URL pattern                 |
| --------------- | --------------------------- |
| Dashboard       | `/` or `/dashboard`         |
| Resource list   | `/customers`                |
| Resource create | `/customers/new`            |
| Resource detail | `/customers/:id`            |
| Resource edit   | `/customers/:id/edit`       |
| HasMany list    | `/customers/:id/orders`     |
| HasMany create  | `/customers/:id/orders/new` |
