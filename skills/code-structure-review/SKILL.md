---
name: code-structure-review
description: Review existing code structure and judge which problems are worth addressing. Use for standalone assessment of responsibility boundaries, caller burden, coupling, and change locality. Report evidence, priorities, and improvement directions without prescribing refactoring. Target design belongs to software-design; code changes belong to implement.
---

# Code Structure Review

Good structure gives responsibilities, knowledge, and changes appropriate owners.
Review concrete costs and benefits, not module size or a preferred refactoring.

## Principles

### Establish problems through scenarios

Use caller interactions, failure handling, or change scenarios to demonstrate
structural costs. File length and similar syntax alone are not evidence.

- **Good:** Show that one policy change requires coordinated edits across callers.
- **Avoid:** Recommend splitting a large file without identifying distinct responsibilities.

### Give knowledge a clear owner

Identify repeated decisions, mixed responsibilities, and unclear ownership of
state and effects. Compare what callers must understand about rules, ordering,
and failures before and after a proposed change.

- **Good:** A module owns transaction coordination. Callers express business intent.
- **Avoid:** Wrap five calls in one method while callers still manage their order
  and recover partial failures.

### Protect independent change

Treat independent rules, ownership, and lifecycles as evidence for retaining
a boundary.

- **Good:** Retain a thin adapter that isolates a provider's protocol and errors.
- **Avoid:** Merge modules because both are small, or split cohesive logic into
  pass-through layers that make changes harder to locate.

### Earn the cost of change

Compare caller burden and change locality with coupling, compatibility risk,
and migration cost. Retaining the current structure is a valid outcome.

- **Good:** Centralize coordination supported by evidence. Preserve failure semantics
  and identify equivalent behavioral coverage.
- **Avoid:** Add an abstraction for hypothetical reuse, or remove internal tests
  without checking which important invariants they protect.

### Choose the direction from evidence

Concentrating knowledge, splitting responsibilities, moving ownership,
simplifying interfaces, and retaining boundaries are all possible outcomes.
Module deepening is one technique for reducing caller knowledge, not the review's goal.

- **Good:** Separate protocol parsing from business decisions when they change independently.
- **Avoid:** Assume every problem calls for a deeper or larger module.

## Execution contract

Review existing code. Output findings and recommendations, not code changes
or a finalized target design.
New-feature design and goal-driven redesign belong to `software-design`.
When implementation is also requested, resolve important candidate choices.
Hand off evidence, constraints, improvement directions, and verification needs
to that skill. Do not bypass its design approval gate.

Bound the inspected area and compatibility constraints. For codebase-wide requests,
scan entry points, tests, and dependencies to choose a concrete area.
Expand only when evidence leads elsewhere.
Inspect implementations, representative callers, and tests.
Identify who owns decisions, state, effects, and failures.
Preserve project terminology. Ask about unknowns that could invalidate the recommendation.

## Boundary checks

Inspect the following in the bounded area. Treat a mismatch as a candidate finding,
not automatic justification for restructuring; establish its cost through a caller
or change scenario.

- **Placement:** Business capabilities are discoverable in the directory structure;
  files have cohesive responsibilities rather than mixing independently changing
  rules and technical mechanisms. Respect sound project conventions.
- **Entry points:** Outside callers use intended business operations rather than
  internal technical implementations, except for designated wiring or integration
  code. Identify leaked protocol, ordering, or recovery knowledge.
- **Physical separation:** Public business workflow files coordinate capabilities,
  while separate implementation files own SQL, protocol parsing, serialization,
  and similar mechanisms. Private helpers in the same file are not this separation.
- **Names:** File names identify their main responsibility; function names distinguish
  business intent, decisions or calculations, and technical mechanisms. Demonstrate
  ambiguity through a caller or maintenance task, not naming preference alone.
- **Change locality:** Trace a relevant rule change and a provider or storage change.
  Check whether each stays with its owner or unnecessarily crosses the boundary.
  Inspect dependency direction, cycles, and existing mechanical boundary checks.

Retain cohesive pure helpers and useful adapters. Do not recommend one file per
function, empty facades, or project-wide technical directories as a universal fix.
Final file placement belongs to `implement` within an approved design; this review
reports evidence and improvement directions, not a mandatory target tree.

## Recommendations

Rank findings by benefit, risk, and change cost. For each finding, include:

- Code locations and a concrete caller or change scenario.
- The structural problem and who currently bears its knowledge or change cost.
- Whether a change is worthwhile and credible improvement directions.
- Changes to caller obligations and ownership, including conditions, outcomes,
  and failure semantics.
- Major costs and verification needs.

Sketch a representative caller interaction when it clarifies a recommendation.
Do not require finalized signatures, a file tree, or a full migration plan;
those belong to `software-design`.
State inspection limits and important exclusions. Distinguish evidence from assumptions.
If code or a required reference is missing, report it.
Keep affected claims provisional.

## References

- Read [design-principles.md](references/design-principles.md) before evaluating
  candidates for shared definitions and design constraints.
- Read [dependencies-and-verification.md](references/dependencies-and-verification.md) for dependency substitution, seam
  changes, or test migration.
- Read [error-semantics.md](references/error-semantics.md) when defaults,
  idempotency, fallback, retries, or error aggregation affect a contract.
- Read [alternatives.md](references/alternatives.md) when credible designs differ
  in caller obligations, state ownership, or dependency direction, or when the
  user requests alternatives.
