---
name: software-design
description: Design software structure and minimal interfaces from a domain model, confirmed requirements, or existing code and an improvement goal. Use for feature architecture, redesigning an implementation, or delivering a selected refactor. Diagnose existing code as needed for the target design; standalone module-boundary diagnosis belongs to module-deepening. Present the design for approval before implementing requested code changes.
---

# Software Design

Turn established business behavior into coherent code objects, modules, and
minimal interfaces. For existing code, diagnose the relevant problems before
designing improvements. Implement and verify only when requested and approved.

## Execution contract

- Treat confirmed requirements, business rules, and visual design as inputs;
  surface contradictions for clarification.
- A domain model is a business-semantic contract, not a class blueprint. Code
  boundaries may differ while preserving the confirmed business meaning.
- Accept a domain model, clear requirements without a separate model document,
  or existing code with an improvement goal. Domain modeling is not a mandatory
  prerequisite for a feature or a behavior-preserving technical refactor.
- Resolve missing or contradictory business meaning with `minimal-domain-modeling`
  when it affects the design. Revisit only the affected rules; unrelated open
  business questions need not block established behavior.
- Reuse a supplied diagnosis and selected recommendation, checking assumptions
  relevant to the design. Otherwise inspect existing code as needed to propose
  the target structure. Standalone evaluation of module-deepening opportunities
  without a target-design or delivery request belongs to `module-deepening`.
- **Approval gate:** Before editing code, present the design and stop for
  explicit user approval. A code-change request alone is not design approval.
  Implement only when both are present; seek renewed approval for material
  changes to the approved model, interface, or behavior.
- Keep detail proportionate to the decision. Design-only requests end with the
  design; assessment-only requests end with findings. Neither authorizes edits.

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

| Input | Starting path |
| --- | --- |
| Domain model or confirmed feature requirements | Extract behavior and constraints; inspect relevant integration points |
| Existing code and an improvement goal | Diagnose the bounded area before choosing the target structure |
| Selected refactoring recommendation | Reuse its evidence and direction; check only design-relevant assumptions |

For existing-code redesign without a supplied diagnosis, inspect implementations,
representative callers, and tests. Establish current state, effect, rule, and failure
ownership. Cite concrete code locations and a caller or change scenario for each
problem that motivates the design. Compare credible directions by benefit, risk,
and change cost; retaining the current structure is a valid outcome. Expand the
inspection only when evidence needed for the target design leads elsewhere.

Clarify unknowns that would materially change the software structure or behavior.
Separate business decisions from technical choices and use the domain-modeling
handoff for the former. If clarification is unavailable, mark the affected design
provisional.

**Exit:** The problem and significant constraints are explicit; unresolved
questions visibly limit the design. Existing-code improvements have evidence
and a justified direction, not just a preference for different structure.

### 2. Design the software structure and derive interfaces

Map established business concepts and behavior to code objects, modules, and
interfaces. Define software responsibilities and the ownership of state and
effects; preserve confirmed business invariants and lifecycles. Compare coherent
structures before settling local details; existing code divisions need not survive,
and business concepts do not require one class each.

Derive the minimum interface needed by callers: operations, inputs, results,
usage constraints, and failure semantics. Assign ownership of state and effects,
and make dependency direction clear. Use project terminology.

Treat compatibility as a migration obligation, not a limit on exploring better
abstractions. Identify proposed contract changes for explicit approval.

**Exit:** The target software structure and interfaces explain the required
scenarios and who enforces business rules and owns state and effects.

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

Present the target software structure, proposed interfaces with representative usage,
important differences from the current design, and the migration and verification
plan. Include signatures, a diagram, or code sketches where they clarify the
decision; explain only significant trade-offs.

For existing-code improvements, include the motivating evidence and expected
benefit. These findings support the design; they do not claim a code change.
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
