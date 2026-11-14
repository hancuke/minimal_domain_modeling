# Dependencies, Seams, and Safe Deepening

Uses the vocabulary in [design-principles.md](design-principles.md).
Dependency categories guide feasibility and verification, not automatic merging
or mandatory architecture patterns.

## Classify the dependency

### In-process

Pure computation or in-memory state is usually easy to exercise directly.
Check input boundaries and state transitions directly; absence of I/O does not
establish shared responsibility.

### Local-substitutable

A local database or filesystem stand-in can make behavior tests practical.
Check its semantic differences from production: transactions, concurrency,
constraints, query behavior, persistence, and failures as applicable.

Use the stand-in for the behavior it faithfully models. Identify production
integration checks for the remaining risk. Availability of a stand-in alone
does not establish that the design is safe or that production behavior is verified.

### Remote but owned

An owned service still has independent deployment, latency, availability, and
versioning constraints. Consider a port when it isolates those concerns from
business decisions; use production transport and test adapters when justified.

Preserve deployment and ownership distinctions even if orchestration moves
behind one interface. In-memory tests cannot prove transport compatibility or
distributed transaction behavior.

### True external

Test doubles cover modeled responses, not the provider's actual contract.
Identify contract or sandbox checks and any unverified provider behavior.

## Choose seam placement

Distinguish caller-facing operations from internal substitution points. Inject
dependencies at the level that owns their lifecycle rather than exposing every
internal dependency as a caller parameter.

State how errors, ordering, retries, and partial effects cross the interface
where relevant. Confirm retry safety and idempotency instead of assuming them.

## Preserve behavior and migrate verification

Before recommending removal of old tests, map their important behavior and edge
cases to the proposed verification. Record uncovered behavior explicitly.

Prefer tests through stable interfaces for caller-observable outcomes. Retain
internal tests with independent algorithm or invariant value. Remove only tests
made redundant by equivalent coverage or tied exclusively to removed structure.

For implementation handoff, identify compatibility obligations, integration
risks, coverage gaps, and whether staged caller migration is necessary.
`software-design` turns these into an actionable migration and verification
plan; a diagnosis alone does not claim the migration has succeeded.
