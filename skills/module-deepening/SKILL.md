---
name: module-deepening
description: Independently diagnose module-deepening opportunities in existing code and judge whether a change is worthwhile. Use for assessment of repeated caller knowledge, changes rippling across modules, or existing boundaries and seams. Target software design and implementation belong to software-design.
---

# Module Deepening

Deeper modules reduce the knowledge callers need for useful behavior;
implementation size does not measure that benefit.

## Principles

### Hide knowledge, not just calls

Compare what callers must understand about rules, ordering, state, and failures
before and after a proposed change.

- **Good:** A module owns transaction coordination; callers express business intent.
- **Avoid:** Wrap five calls in one method while callers still manage their order
  and recover partial failures.

### Concentrate shared knowledge

Trace repeated decisions to a shared owner; similar syntax alone is not evidence.

- **Good:** Several callers enforce the same eligibility rule; its owner exposes
  an operation that enforces it consistently.
- **Avoid:** Combine similar-looking validation with different business meanings.

### Protect independent change

Treat independent rules, ownership, and lifecycles as evidence for retaining
a boundary.

- **Good:** Retain a thin adapter that isolates a provider's protocol and errors.
- **Avoid:** Merge modules because both are small or have no I/O.

### Earn the cost of change

Rank reduced caller burden against broader responsibility, coupling, compatibility
risk, and migration cost. Retaining the current structure is a valid outcome.

- **Good:** Centralize evidenced coordination while preserving failure semantics
  and identifying equivalent behavioral coverage.
- **Avoid:** Add an abstraction for hypothetical reuse, or remove internal tests
  without checking which important invariants they protect.

## Execution contract

Diagnose existing code and output recommendations, not code changes. New-feature
design and goal-driven redesign belong to `software-design`. When implementation
is also requested, resolve consequential candidate choices and hand off evidence,
constraints, interface direction, and verification needs to that skill.

Bound the inspected area and compatibility constraints. For codebase-wide requests,
scan entry points, tests, and dependencies to choose a concrete area; expand only
when evidence leads elsewhere. Inspect implementations, representative callers,
and tests to establish decision, state, effect, and failure ownership. Preserve
project terminology; ask about unknowns that could invalidate the recommendation.

Rank findings by benefit, risk, and change cost. Each finding needs code locations,
a concrete caller or change scenario, a deepen-or-retain recommendation, changed
caller obligations and ownership, major costs, and verification needs. For a
proposed change, sketch a representative caller interaction. State inspection
limits and distinguish evidence from assumptions; explain important exclusions.
If code or a required reference is missing, report it and keep affected claims
provisional.

## References

- Read [design-principles.md](references/design-principles.md) before evaluating
  candidates for shared definitions and design constraints.
- Read [deepening.md](references/deepening.md) for dependency substitution, seam
  changes, or test migration.
- Read [alternatives.md](references/alternatives.md) when credible designs differ
  in caller obligations, state ownership, or dependency direction, or when the
  user requests alternatives.
