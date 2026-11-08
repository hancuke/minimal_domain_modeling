# Shared Design Principles

Use this reference to evaluate technical designs, not to start a diagnostic
workflow. Preserve the project's business and technical names; these terms
clarify the analysis rather than mandate renaming.

## Vocabulary

- **Module:** something with an interface and implementation, from a function
  to a package or cross-layer slice.
- **Interface:** everything a caller must know to use a module correctly,
  including inputs, results, invariants, ordering, failures, configuration,
  and relevant performance constraints. It is broader than a type signature.
- **Implementation:** the code and internal structure behind that interface.
- **Depth:** useful behavior relative to the knowledge the interface demands.
  More methods or implementation lines do not measure depth.
- **Seam:** a place where behavior can be substituted without editing the code
  that uses it. Decide where substitution belongs separately from what it hides.
- **Adapter:** a concrete implementation satisfying a contract at a seam.
- **Leverage:** capability callers obtain for the interface they must learn.
- **Locality:** concentration of knowledge, changes, and verification in the
  places that own them.

## Assess the complete interface

Evaluate complete caller scenarios, not just entry-point counts. A single
method with modes, ordering constraints, and hidden failure behavior can demand
more knowledge than several explicit operations.

Ask what happens if the module is removed: does knowledge spread into callers,
or does needless indirection disappear? Consider contract translation, invariant
protection, and dependency isolation as well as computation. A thin adapter can
have substantial value.

## Architectural constraints

Make important dependency directions and ownership visible. Justify a seam by
its role in the scenario being evaluated. Keep internal substitution points
private when callers do not need them.

Where useful, separate pure decisions from effect execution. Necessary I/O and
mutation remain part of the design.

Expose the information callers need to handle failures correctly. Hiding
complexity must not hide errors or turn partial failure into apparent success.

## Verification

Types, runtime validation, and tests should protect the actual constraints.
Place durable knowledge near its owner using existing documentation conventions;
do not create documents merely to restate discoverable code.

For test surfaces and coverage migration, use
[deepening.md](deepening.md#preserve-behavior-and-migrate-verification) when those
decisions are in scope.
