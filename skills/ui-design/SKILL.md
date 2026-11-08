---
name: ui-design
description: Design interface hierarchy and composition, review rendered UI, or implement pages, components, and restyling around established user tasks and data. Preserve business behavior and data contracts. Change code only when implementation is requested.
---

# UI Design

Composition should shorten the user's path to the task, not compete with it.

## Principles

- **Task first:** Let the primary task determine reading order and action priority.
- **Semantic consistency:** Use the same treatment for the same meaning; choose
  different structures when comparison, explanation, or action requires them.
- **Useful density:** Balance scanability and detail for the task, not a preferred
  amount of whitespace.
- **Earned emphasis:** Reserve visual prominence for important information and
  actions. Decoration comes after behavior, readability, and responsive layout.

**Good:** Compare pending approvals in aligned rows, with a clear primary action
and quieter supporting details.

**Avoid:** Turn every approval into a large card, forcing scrolling and making
cross-item comparison harder.

## Execution contract

| Request | Work | Finish |
|---|---|---|
| Design | Propose hierarchy, composition, and trade-offs | Explain the task path and key choices; leave code unchanged |
| Review | Inspect the existing rendering and rank findings | Cite screenshots or specific regions; leave code unchanged |
| Implement | Change the interface and inspect its rendering | Report changes, checks, and remaining limitations |

Preserve business behavior and data contracts in every branch. Clarify unknowns
that would change the task or interaction. Keep a small request small.

## 1. Frame the task

Identify the primary task, key information, secondary actions, and interaction
sequence. Inspect supplied content and existing conventions.

**Exit:** The task and information priorities are clear, or affected choices
are explicitly provisional.

## 2. Design or assess the composition

Choose regions, alignment, typography, density, and emphasis using the principles.
Explain consequential choices, such as a comparison table versus individual cards.
For reviews, apply step 4's checks to the existing rendering rather than
redesigning by default.

**Exit:** Each recommendation addresses a task need or an evidenced problem.
Design-only requests finish with the proposal and its validation needs.

## 3. Implement when requested

Use the project's framework, components, and design tokens. Keep related callers
and interactions consistent; prioritize the most consequential problems.

**Exit:** Requested changes are implemented, or blockers are explicit.

## 4. Verify the rendering and interactions

For reviews, inspect the existing page; for implementations, inspect the changed
page. Run the application when available and capture actual screenshots.

Check applicable behavior:

- The primary action is distinguishable from secondary actions.
- Alignment and labels support scanning or comparison.
- Long content and narrow screens preserve access and task order.
- Keyboard navigation, focus, and labels support the primary interaction.
- Relevant empty, loading, and error states remain understandable.

Screenshots support visual findings; exercise interactions to verify behavior.
If rendering or interaction tools are unavailable, name the missing evidence
and keep those conclusions unverified. Code inspection is not rendered proof.

**Exit:** Report findings or changes with evidence, checks, and unverified items.
Only a verified implementation is described as a working interface.