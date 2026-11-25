# Layout Patterns

Use this reference when choosing or contrasting layout topologies for a new
feature, multi-region screen, or redesign. Match layout structure to how users
scan, compare, inspect, or edit the data—not visual habit.

## Core layout topologies

| Pattern | Best for / Information shape | Avoid when | Narrow-screen fallback |
| --- | --- | --- | --- |
| **Aligned table / structured list** | Comparing 5+ homogeneous items across shared attributes, scanning statuses, or taking row/bulk actions | Items have wildly different structures or require deep multi-section reading per item | Collapse secondary columns into a stacked row summary; keep key identifier, status, and primary action visible |
| **Master-detail split view** | Rapidly stepping through a list while inspecting or editing rich item details without losing list position | List items need wide multi-column comparison, or detail inspection is rare | Two-step navigation (list view to detail view with clear back navigation) |
| **List + slide-over drawer** | List/table scanning is primary, with occasional detail inspection or quick edits that should preserve filters and scroll position | Users must compare two items side by side or perform lengthy multi-section editing | Full-screen sheet or dedicated detail route |
| **Focused single-column form** | Creating or editing a single record with linear reading order and low distraction | Users must reference external lists, live previews, or unrelated records while editing | Same single-column flow with sticky or clearly placed primary action |
| **Stepper / multi-step wizard** | High-risk or sequential workflows where later choices depend on earlier answers, or upfront input would overwhelm one screen | Users need to jump freely across sections or frequently revisit earlier fields | Compact step indicator (e.g., `Step 2 of 4`) with sticky Next/Back controls |
| **Split workbench (controls + canvas)** | Live preview, query/filter exploration, or direct manipulation where control changes immediately update a main output surface | Changes do not need immediate visual feedback, or controls are trivial | Primary canvas on top with collapsible control sheet or segmented toggle |
| **Summary-to-detail overview** | Triage or monitoring across heterogeneous metrics/queues that route users into targeted follow-up views | The user already knows the exact record or action and just wants to execute it | Stack priority KPIs/alerts first, followed by actionable queues |

## Axes for generating distinct options

When proposing 2–3 layout options, vary at least one structural axis below so
each option represents a genuine trade-off:

- **Spatial topology:** Single-column flow vs. master-detail split vs. list +
  slide-over drawer vs. dedicated detail page.
- **Information disclosure:** Inline expansion vs. summary-with-drill-down vs.
  progressive reveal based on user selection.
- **Flow pacing:** Single-page immediate editing vs. guided multi-step wizard
  vs. quick-create modal with full-page advanced editor.
- **Density vs. focus:** High-density comparison table (optimizes cross-item
  scanning) vs. focused single-item workspace (optimizes deep inspection or
  error-free editing).

Do not present cosmetic differences (card borders, accent colors, padding
tweaks, or icon swaps) as separate layout options.

## Wireframe checklist

Before presenting options, verify each ASCII wireframe makes four things
immediately visible:

1. **Primary reading order:** Where the user looks first and what follows.
2. **Primary action placement:** Where the main decision or submit control sits
   relative to the information needed to make it.
3. **Secondary/supporting regions:** Filters, metadata, or secondary actions
   visually subordinated to the main task.
4. **Overflow and state resilience:** Where empty, loading, error, and long
   content states land without breaking the layout.
