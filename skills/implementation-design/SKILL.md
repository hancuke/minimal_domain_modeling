---
name: implementation-design
description: Design object models and minimal interfaces from established requirements or a selected refactoring recommendation. Use when defining project architecture, planning a feature's implementation, or turning a chosen technical change into a delivery plan. Present the design for approval before implementing requested code changes.
---

# Implementation Design

Build a coherent object model for the problem, derive its smallest useful
interface, then plan the implementation and migration.

## Execution contract

- Treat confirmed requirements, business rules, and visual design as inputs;
  surface contradictions for clarification.
- Reuse a supplied diagnosis and selected recommendation, checking only
  assumptions relevant to the design. New features need no prior diagnosis;
  route open-ended structural diagnosis to `module-deepening`.
- **Approval gate:** Before editing code, present the design and stop for
  explicit user approval. A code-change request alone is not design approval.
  Implement only when both are present; seek renewed approval for material
  changes to the approved model, interface, or behavior.
- Keep detail proportionate to the decision. Design-only and review requests
  end with the design.

## Design principles

- **Whole before parts.** Optimize the model within the problem's scope before
  local details. Existing code is evidence, not a template; reorganize or remove
  objects and responsibilities when that yields a simpler model.
- **Small interfaces, deep modules.** Minimize the concepts, operations, and
  constraints callers must understand. Count that knowledge, not just methods;
  generic entry points and parameter bags can merely move complexity to callers.
- **Regular rules, clear flow.** Keep rules orthogonal, names literal, and data
  flow visible so readers can follow the main path.
- **Only necessary structure.** Prefer simple functions, objects, or pipelines
  over speculative layers and options. Whole-model thinking does not require
  a whole-codebase rewrite.

## Workflow

### 1. Confirm inputs and constraints

State the main job in one sentence. Identify required scenarios, acceptance
criteria, scope, and compatibility needs. Inspect relevant code, terminology,
conventions, and tests; distinguish business constraints from historical
implementation choices.

Clarify unknowns that would materially change the model. If clarification is
unavailable, mark the affected design provisional.

**Exit:** The problem and significant constraints are explicit; unresolved
questions visibly limit the design.

### 2. Build the target model and derive interfaces

Use known requirements and domain knowledge to define objects, responsibilities,
relationships, invariants, and state transitions. Compare coherent models before
settling local implementation details; existing object divisions need not survive.

Derive the minimum interface needed by callers: operations, inputs, results,
usage constraints, and failure semantics. Assign ownership of state and effects,
and make dependency direction clear. Use project terminology.

Treat compatibility as a migration obligation, not a limit on exploring better
abstractions. Identify proposed contract changes for explicit approval.

**Exit:** The target model and interfaces explain the required scenarios and
who owns their rules, state, and effects.

### 3. Challenge the solution and plan migration

Trace normal, failure, and change scenarios through the proposed interfaces.
Apply the principles and read the [references](#references) when their conditions
apply. Check whether the model removes complexity or merely shifts it to callers.

Then compare the target model with the current implementation. Plan caller
migration, compatibility checks, and recovery proportionate to deployment and
data risk. Identify tests, types, and integration checks that protect behavior;
map existing coverage when refactoring.

**Exit:** Added complexity is justified, significant trade-offs are explicit,
and migration and verification cover important risks.

### 4. Present the design and await approval

Present the target model, proposed interfaces with representative usage,
important differences from the current design, and the migration and verification
plan. Include signatures, a diagram, or code sketches where they clarify the
decision; explain only significant trade-offs.

State unresolved assumptions separately from approval status. For implementation
requests, ask for approval and stop under the approval gate.

**Exit:** The design is submitted for review. Submission completes the design
phase; it does not authorize implementation.

### 5. Implement and verify after approval

Proceed only when the approval gate is satisfied and consequential uncertainties
are resolved. Make the smallest coherent change that realizes the approved
model, follow project conventions, and keep dependent callers consistent.

Run existing checks covering changed behavior and relevant integration risks.
If implementation invalidates a design assumption, revisit the affected design.

**Exit:** The approved change is implemented and checks pass, or blockers and
unverified behavior are explicit.

## References

Read sibling references only under these conditions; reading is not diagnosis:

| Condition | Reference |
| --- | --- |
| Module boundaries, dependency isolation, or consequential architectural choices | [design-principles.md](../module-deepening/references/design-principles.md) |
| Dependency substitution, seam changes, or test migration | [deepening.md](../module-deepening/references/deepening.md) |
| Alternatives requested, or designs differ consequentially in caller obligations, state ownership, or dependency direction | [alternatives.md](../module-deepening/references/alternatives.md) |

If a required reference cannot be read, report its path and mark affected
judgments provisional.
