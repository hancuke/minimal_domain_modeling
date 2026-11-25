---
name: ui-design
description: Design UI layout, information hierarchy, and interaction flow for a feature or redesign. Propose structurally distinct layout options with ASCII wireframes and trade-offs for user selection, then refine the chosen option into an implementation-ready UI specification. Preserve business behavior and data contracts. Do not modify code.
---

# UI Design

Turn a feature goal and its data shape into concrete, comparable UI layout
options, then refine the user's chosen option into an implementation-ready
specification.

## Contract

- **Required inputs:** Feature goal or user task, key data and actions (or
  confirmed domain/API constraints), and optional context such as target
  viewport, platform, or existing UI conventions.
- **Non-goals (refuse):** Refuse modifying or implementing code directly.
  Refuse inventing or silently changing business rules, exception paths, or
  data contracts. Refuse styling or CSS polish before layout, hierarchy, and
  task flow are selected. Refuse fake alternatives that differ only in visual
  decoration rather than layout structure or interaction flow.
- **Handoff targets:** Hand off missing or contradictory business rules or
  domain concepts to `minimal-domain-modeling`. Hand off underlying module
  architecture or API contract changes to `software-design`. Hand off the
  approved UI specification to `implement` for code changes.
- **Mandatory approval gates (must stop):**
  - **Task and data-shape ambiguity gate:** When missing user tasks, data
    cardinality (e.g., 3 items vs. 300 items), or business constraints would
    materially change the layout structure, stop and clarify before proposing
    options. Mark non-blocking assumptions explicitly as provisional.
  - **Option selection gate:** After presenting structurally distinct layout
    options, trade-offs, and a recommendation, stop and wait for the user to
    select, combine, or adjust an option before writing a detailed UI
    specification.

## Principles

- **Task first:** Let the primary user task and its frequency determine reading
  order, spatial weight, and action priority.
  - **Good:** On an order-approval screen, place the status summary, key
    risk fields, and primary Approve/Reject actions above the fold, with audit
    history below.
  - **Avoid:** Render fields in database-schema order and bury the Approve
    button at the bottom of a long scrolling form.
- **Structure matches information shape:** Choose layout topology (table,
  master-detail, drawer, stepper, workbench) from how users compare, inspect,
  or edit the data—not habit or visual trend.
  - **Good:** Use aligned rows in a table to compare pending approvals across
    amount, requester, and due date, with a row action for quick decision.
  - **Avoid:** Turn every pending approval into a tall multi-line card,
    forcing scrolling and breaking column-by-column comparison.
- **Useful density and earned emphasis:** Balance scanability and detail for
  the task; reserve visual prominence for primary actions and critical states
  rather than decoration.
  - **Good:** Style one primary action distinctly, keep secondary actions
    quiet, and use spacing and alignment rather than nested boxes to group
    related fields.
  - **Avoid:** Give every button a strong filled style or wrap every section
    in heavy borders, shadows, and decorative icons that compete with data.
- **Structurally distinct options:** When proposing choices, vary real
  architectural axes—spatial topology, disclosure model, flow pacing, or
  density vs. focus—so the user makes a meaningful trade-off.
  - **Good:** Contrast a dense table + slide-over detail drawer against a
    persistent master-detail split view, explaining how each trades comparison
    width for fast item switching.
  - **Avoid:** Present three "options" that use the same layout and differ
    only in card rounding, accent color, or sidebar width.

## Workflow

### 1. Frame the task and information shape
- Identify the primary user task, secondary actions, and task frequency.
- Capture the information shape: item count/cardinality, field length
  variation, whether cross-item comparison is needed, and required states
  (empty, loading, error, partial/invalid).
- Note platform, viewport, and existing UI conventions when supplied.
- Trigger the *Task and data-shape ambiguity gate* if blocking unknowns remain;
  otherwise mark minor assumptions as provisional.
- *Intermediate deliverable:* Concise task and information-shape brief with
  explicit provisional assumptions.
- *Stopping condition:* Primary task, key data/actions, and layout-shaping
  constraints are clear.

### 2. Propose structurally distinct layout options
- Read [layout-patterns.md](references/layout-patterns.md) when selecting among
  layout topologies for a new feature, multi-region screen, or redesign.
- Propose 2–3 genuinely distinct layout and interaction options (for a small,
  bounded control or dialog, propose 1 default and 1 meaningful alternative).
- For each option, provide:
  - A compact ASCII wireframe showing regions, reading order, and primary
    action placement.
  - The primary task flow (how the user completes the core job).
  - What it optimizes for, what it sacrifices, and narrow-screen behavior.
- Provide a structured comparison table and a clear recommendation explaining
  why one option best fits the framed constraints.
- *Intermediate deliverable:* Comparable layout options with ASCII wireframes,
  comparison table, and recommendation.
- *Stopping condition:* Trigger the *Option selection gate* and stop for user
  selection.

### 3. Refine the selected option into an implementation-ready spec
- Run this step only after the user selects or combines an option.
- Specify region and component hierarchy, information priority per region,
  interaction states (empty, loading, error, disabled, overflow/long text),
  responsive degradation for narrow screens, and keyboard/accessibility
  priorities (focus order, labels, primary action reachability).
- List concrete visual and interaction verification checks for implementation.
- *Intermediate deliverable:* Implementation-ready UI layout and interaction
  specification.
- *Stopping condition:* An implementer can build the interface without guessing
  layout structure, state behavior, or interaction intent.

## Verification

Do not treat a UI design response as complete until all applicable checks below
are satisfied.

### Observable evidence
- Walk through the primary task end-to-end on each proposed ASCII wireframe
  (initial visual focus, scanning path, and steps to complete the action).
- Walk through at least two stress or edge cases (such as empty state, high
  item count, long text, narrow viewport, or error state) to verify the layout
  does not break.

### Validation criteria
- **Genuine differentiation:** Proposed options differ on layout topology,
  disclosure model, or interaction flow rather than superficial styling.
- **Task-path clarity:** Primary information and the primary action are
  immediately distinguishable in every wireframe.
- **State and edge coverage:** Empty, loading, error, overflow, and
  narrow-screen behaviors have explicit handling (summarized in option
  trade-offs, detailed in the refined spec).
- **Contract preservation:** Confirmed business rules, exception paths, and
  data contracts remain intact; provisional assumptions are explicitly marked.

### Delivery constraints
- Present options using compact ASCII wireframes plus a structured comparison
  table (`Option` / `Optimizes for` / `Trade-off` / `Best when`) and a
  recommended choice.
- Keep small requests small: do not inflate a simple modal or single control
  into three elaborate page proposals.
- Never modify code or create implementation files. Stop after presenting
  options for selection, and stop again after delivering the refined spec for
  handoff.

## References

Read references only under these conditions.

| Condition | Reference |
| --- | --- |
| Choosing or contrasting layout topologies for a new feature, multi-region screen, or redesign | [layout-patterns.md](references/layout-patterns.md) |

If a required reference cannot be read, report its path and mark affected
layout comparisons as provisional.
