# Shared Design Principles

Use this reference to evaluate technical designs. It does not start a diagnostic
workflow. Preserve the project's business and technical names.
Use these terms to clarify analysis, not to require renaming.

## Vocabulary

- **Module:** a unit with an interface and implementation, from a function
  to a package or cross-layer slice.
- **Interface:** everything a caller must know to use a module correctly,
  including inputs, results, invariants, ordering, failures, configuration,
  and relevant performance constraints. It is broader than a type signature.
- **Implementation:** the code and internal structure behind that interface.
- **Depth:** useful behavior relative to the knowledge the interface demands.
  More methods or implementation lines do not measure depth.
- **Seam:** a place where behavior can be replaced without editing the code
  that uses it. Decide its location separately from the knowledge it hides.
- **Adapter:** a concrete implementation that satisfies a contract at a seam.
- **Leverage:** capability callers obtain for the interface they must learn.
- **Locality:** concentration of knowledge, changes, and verification in the
  places that own them.

Depth is one evaluation axis, not a goal that overrides cohesion, independent
change, dependency isolation, compatibility, or migration cost.

## Diagnose complexity through scenarios

Use three perspectives to identify costs, not as a mandatory scoring checklist:

- **Change amplification:** One logical change requires coordinated edits in
  several places. Trace which decision is repeated and who should own it.
- **Cognitive load:** A caller must understand details unrelated to its task.
  Identify the rules, ordering, configuration, or recovery knowledge it carries.
- **Hidden impact:** The affected code or required knowledge is hard to discover.
  Show a dependency or assumption that a plausible change could overlook.

Support each finding with code locations and a caller or change scenario.
Many files, a thin wrapper, or a difficult name can prompt investigation;
none alone proves that restructuring is worthwhile.

**Good:** A new provider error requires every business caller to understand its
transport code. Identify where translation can preserve meaningful outcomes.
**Avoid:** Merge a thin protocol adapter into business code because it does
little computation.

## Assess the complete interface

Evaluate complete caller scenarios, not just entry-point counts. A single
method with modes, ordering constraints, and hidden failure behavior can demand
more knowledge than several explicit operations.

Ask what happens if the module is removed.
Does knowledge spread into callers, or does unnecessary indirection disappear?
Consider contract translation, invariant protection, and dependency isolation,
not just computation. A thin adapter can have substantial value.

## Architectural constraints

Make important dependency directions and ownership clear.
Justify each seam by its role in the scenario.
Keep internal substitution points private when callers do not need them.

Where useful, separate pure decisions from effect execution. Necessary I/O and
mutation remain part of the design.

Expose the information callers need to handle failures correctly. Hiding
complexity must not hide errors or turn partial failure into apparent success.

## Verification

Types, runtime validation, and tests should protect the actual constraints.
Place durable knowledge near its owner. Use existing documentation conventions.
Do not create documents only to repeat information that is clear in the code.

For test surfaces and coverage migration, use
[dependencies-and-verification.md](dependencies-and-verification.md#preserve-behavior-and-migrate-verification) when those
decisions are in scope.
