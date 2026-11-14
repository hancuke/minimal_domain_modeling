---
name: software-design
description: Design software structure and minimal interfaces from a domain model, confirmed requirements, or existing code and an improvement goal. Use for feature architecture, redesigning an implementation, or delivering a selected refactor. Diagnose existing code as needed for the target design. Standalone module-boundary diagnosis belongs to module-deepening. Present the design for approval before implementing requested code changes.
---

# Software Design

Turn established business behavior into coherent code objects, modules, and
minimal interfaces. Diagnose relevant problems before redesigning existing code.
Implement and verify only when requested and approved.

## Execution contract

- Treat confirmed requirements, business rules, and visual design as inputs.
  State contradictions and ask for clarification.
- A domain model is a business-semantic contract, not a class blueprint. Code
  boundaries may differ while preserving the confirmed business meaning.
- Accept a domain model, clear requirements without a separate model document,
  or existing code with an improvement goal. Domain modeling is not a mandatory
  prerequisite for a feature or a behavior-preserving technical refactor.
- Use `minimal-domain-modeling` to resolve missing or contradictory business meaning
  that affects the design. Revisit only the affected rules.
  Do not let unrelated business questions block established behavior.
- Reuse a supplied diagnosis and selected recommendation.
  Check assumptions relevant to the design. Otherwise inspect the relevant existing code.
  Use `module-deepening` for standalone module-boundary diagnosis without a request
  for a target design or implementation.
- **Approval gate:** Before editing code, present the design and stop for
  explicit user approval. A code-change request alone is not design approval.
  Implement only when both are present. Seek renewed approval for material
  changes to the approved software structure, interface, or behavior.
- Design-only requests end with the design. Assessment-only requests end with
  findings. Neither authorizes edits.

## Design principles

- **Whole before parts.** Optimize the software structure within the problem's
  scope before local details. Existing code is evidence, not a template.
  Reorganize or remove objects and responsibilities when that simplifies the structure.
- **Small interfaces, deep modules.** Minimize the concepts, operations, and
  constraints callers must understand. Count that knowledge, not just methods.
  Generic entry points and parameter bags can move complexity to callers.
- **Regular rules, clear flow.** Keep rules orthogonal.
  Use names that describe behavior. Make data flow easy to follow.
- **Only necessary structure.** Prefer simple functions, objects, or pipelines
  over speculative layers and options. Designing the complete structure does not require
  a whole-codebase rewrite.

## Workflow

### 1. Confirm inputs and constraints

State the main job in one sentence. Identify required scenarios, acceptance
criteria, scope, and compatibility needs. Inspect relevant code, terminology,
conventions, and tests. Distinguish business constraints from past implementation choices.

| Input | Starting path |
| --- | --- |
| Domain model or confirmed feature requirements | Extract behavior and constraints; inspect relevant integration points |
| Existing code and an improvement goal | Diagnose the bounded area before choosing the target structure |
| Selected refactoring recommendation | Reuse its evidence and direction; check only design-relevant assumptions |

For existing-code redesign without a supplied diagnosis, inspect implementations,
representative callers, and tests. Identify who owns state, effects, rules, and failures.
For each motivating problem, cite code locations and a caller or change scenario.
Compare credible directions by benefit, risk, and change cost.
Retaining the current structure is a valid outcome.
Expand inspection only when the target design needs evidence from another area.

Clarify unknowns that would materially change the software structure or behavior.
Separate business decisions from technical choices.
Use the domain-modeling handoff for business decisions.
If clarification is unavailable, mark the affected design as provisional.

**Exit:** The problem and significant constraints are explicit.
Unresolved questions limit the affected design.
Existing-code improvements have evidence and a justified direction.

### 2. Design the software structure and derive interfaces

Map established business concepts and behavior to code objects, modules, and
interfaces. Define software responsibilities and ownership of state and effects.
Preserve confirmed business invariants and lifecycles.
Compare coherent structures before deciding local details.
Existing code boundaries can change. A business concept does not require its own class.

Derive the minimum interface needed by callers: operations, inputs, results,
usage constraints, and failure semantics. Make dependency direction clear.
Use project terminology.

Treat compatibility as a migration obligation, not a limit on exploring better
abstractions. Identify proposed contract changes for explicit approval.

**Exit:** The structure and interfaces support required scenarios.
Ownership of business rules, state, and effects is clear.

### 3. Challenge the solution and plan migration

Trace normal, failure, and change scenarios through the proposed interfaces.
Apply the principles. Read the [references](#references) when their conditions apply.
Check whether the structure removes complexity or shifts it to callers.

Compare the target structure with the current implementation.
Plan caller migration, compatibility checks, and recovery for deployment and data risks.
Identify tests, types, and integration checks that protect behavior.
Map existing coverage when refactoring.

**Exit:** Added complexity is justified. Significant trade-offs are explicit.
Migration and verification cover important risks.

### 4. Present the design and await approval

Present the target structure and interfaces with representative usage.
Explain important differences from the current design.
Include the migration and verification plan.
Use signatures, a diagram, or code sketches only when they clarify the decision.
Explain significant trade-offs.

For existing-code improvements, include the motivating evidence and expected
benefit. These findings support the design. They do not claim a code change.
State unresolved assumptions separately from approval status. For implementation
requests, ask for approval and stop under the approval gate.

**Exit:** The design is submitted for review.
Submission completes the design phase. It does not authorize implementation.

### 5. Implement and verify after approval

Proceed only after the approval gate is satisfied and key uncertainties are resolved.
Make the smallest coherent change that implements the approved structure.
Follow project conventions. Keep dependent callers consistent.

Run existing checks covering changed behavior and relevant integration risks.
If implementation invalidates a design assumption, revisit the affected design.

**Exit:** The approved change is implemented and checks pass.
Otherwise, state blockers and unverified behavior.

## References

Read sibling references only under these conditions. Reading alone is not diagnosis.

| Condition | Reference |
| --- | --- |
| Module boundaries, dependency isolation, or consequential architectural choices | [design-principles.md](../module-deepening/references/design-principles.md) |
| Dependency substitution, seam changes, or test migration | [deepening.md](../module-deepening/references/deepening.md) |
| Alternatives requested, or designs differ consequentially in caller obligations, state ownership, or dependency direction | [alternatives.md](../module-deepening/references/alternatives.md) |

If a required reference cannot be read, report its path.
Mark affected judgments as provisional.
