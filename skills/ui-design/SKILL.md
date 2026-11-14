---
name: ui-design
description: Design interface hierarchy and composition, review rendered UI, or implement pages, components, and restyling around established user tasks and data. Preserve business behavior and data contracts. Change code only when implementation is requested.
---

# UI Design

Use composition to shorten the user's path to the task.

## Principles

- **Task first:** Let the primary task determine reading order and action priority.
- **Semantic consistency:** Use the same treatment for the same meaning.
  Use different structures when comparison, explanation, or action requires them.
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

Preserve business behavior, including exception paths, and data contracts for every request.
Clarify unknowns that could change the task or interaction. Keep small requests small.

## 1. Frame the task

Identify the primary task, key information, secondary actions, and interaction
sequence. Inspect the supplied content and existing conventions.

**Exit:** The task and information priorities are clear, or affected choices
are marked as provisional.

## 2. Design or assess the composition

Choose regions, alignment, typography, density, and emphasis using the principles.
Explain important choices, such as a comparison table instead of individual cards.
For reviews, apply step 4 to the existing rendering. Do not redesign by default.

**Exit:** Each recommendation addresses a task need or a problem supported by evidence.
For design-only requests, deliver the proposal and its validation needs.

## 3. Implement when requested

Use the project's framework, components, and design tokens.
Keep related callers and interactions consistent. Fix the most important problems first.

**Exit:** Requested changes are implemented, or blockers are explicit.

## 4. Verify the rendering and interactions

For reviews, inspect the existing page. For implementations, inspect the changed page.
Run the application when available. Capture screenshots of the actual rendering.

Check applicable behavior:

- The primary action is distinguishable from secondary actions.
- Alignment and labels support scanning or comparison.
- Long content and narrow screens preserve access and task order.
- Keyboard navigation, focus, and labels support the primary interaction.
- Relevant empty, loading, and error states remain understandable.

Use screenshots to support visual findings. Exercise interactions to verify behavior.
If rendering or interaction tools are unavailable, identify the missing evidence.
Mark the affected conclusions as unverified. Code inspection does not prove rendering.

**Exit:** Report findings or changes with evidence, checks, and unverified items.
Describe an interface as working only after verifying its implementation.
