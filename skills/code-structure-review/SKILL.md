---
name: code-structure-review
description: Review existing code structure and judge which problems are worth addressing. Use for standalone assessment of responsibility boundaries, caller burden, coupling, and change locality. Report evidence, priorities, and improvement directions without prescribing refactoring. Target design belongs to software-design; code changes belong to implement.
---

# Code Structure Review

Good structure gives responsibilities, knowledge, and changes appropriate owners.
Review concrete costs and benefits, not module size or a preferred refactoring.

## Contract

- **Required inputs:** Existing code (specific modules, paths, or codebase area)
  and known compatibility constraints.
- **Non-goals (refuse):** Refuse modifying code, prescribing finalized interface
  signatures, mandating a target file tree, or writing full migration plans.
  Refuse new-feature design and goal-driven redesign (refer to `software-design`).
- **Handoff targets:** Hand off target design, finalized interfaces, and
  migration plans—together with review evidence, constraints, improvement
  directions, and verification needs—to `software-design`. Hand off approved
  code changes to `implement`. Hand off missing or contradictory business rules
  to `minimal-domain-modeling`.
- **Mandatory approval gates (must stop):**
  - **Review-only handoff gate:** Stop after presenting ranked findings and
    improvement directions. Even when implementation is also requested, resolve
    important candidate choices and hand off to `software-design` without
    modifying code or bypassing its design approval gate.
  - **Blocking unknowns gate:** When missing code, context, or business
    constraints could invalidate a recommendation, ask the user before
    finalizing or explicitly mark affected judgments as provisional.

## Principles

- **Scenario-grounded evidence:** Use caller interactions, failure handling, or
  change scenarios to demonstrate structural costs and choose the improvement
  direction. File length, similar syntax, or a preference for deeper modules
  alone is not evidence; concentrating knowledge, splitting responsibilities,
  moving ownership, simplifying interfaces, and retaining boundaries are all
  valid outcomes.
  - **Good:** Show that one policy change requires coordinated edits across
    callers, or separate protocol parsing from business decisions because they
    change independently.
  - **Avoid:** Recommend splitting a large file without identifying distinct
    responsibilities, or assume every problem calls for a deeper, larger module.
- **Clear knowledge ownership:** Identify repeated decisions, mixed
  responsibilities, and unclear ownership of state and effects. Compare what
  callers must understand about rules, ordering, and failures before and after a
  proposed direction.
  - **Good:** Consolidate transaction coordination behind one module so callers
    express business intent without managing step order or partial rollback.
  - **Avoid:** Wrap five calls behind one method while callers still manage call
    order and recover partial failures.
- **Protect independent change:** Treat independent rules, ownership, and
  lifecycles as evidence for retaining a boundary. Keep cohesive pure helpers
  and useful adapters; do not recommend one file per function, empty facades, or
  project-wide technical directories as a universal fix.
  - **Good:** Retain a thin adapter that isolates a provider's protocol and
    error translation from business code.
  - **Avoid:** Merge that adapter into business code because it is small, or
    split cohesive logic into pass-through layers that make changes harder to
    locate.
- **Earn the cost of change:** Weigh caller burden and change locality against
  coupling, compatibility risk, and migration cost under the same requirements.
  Retaining the current structure is a valid outcome.
  - **Good:** Recommend centralizing coordination only when scenario evidence
    justifies the migration cost, preserving failure semantics and mapping
    equivalent behavioral coverage before retiring internal tests.
  - **Avoid:** Recommend adding an abstraction for hypothetical reuse, or
    dropping internal tests without checking which invariants they protect.

## Workflow

### 1. Bound scope and gather evidence
- Bound the inspected area and compatibility constraints. For codebase-wide
  requests, scan entry points, tests, and dependencies to choose a concrete
  area; expand only when evidence leads elsewhere.
- Inspect implementations, representative callers, and tests. Identify who owns
  decisions, state, effects, and failures. Preserve project terminology.
- *Intermediate deliverable:* Bounded inspection scope, compatibility
  constraints, and ownership map grounded in code locations.
- *Stopping condition:* Target area, representative callers, and current
  ownership of rules, state, effects, and failures are established.

### 2. Run boundary checks and evaluate costs
- Read [design-principles.md](references/design-principles.md) before evaluating
  candidates; consult other references only when their conditions apply.
- Inspect the bounded area across five boundary checks. Treat a mismatch as a
  candidate finding, not automatic justification for restructuring; establish
  its cost through a caller or change scenario:
  - **Placement:** Business capabilities are discoverable in the directory
    structure; files have cohesive responsibilities rather than mixing
    independently changing rules and technical mechanisms. Respect sound project
    conventions.
  - **Entry points:** Outside callers use intended business operations rather
    than internal technical implementations (except designated wiring or
    integration code). Identify leaked protocol, ordering, or recovery knowledge.
  - **Physical separation:** Public business workflow files coordinate
    capabilities, while separate files own SQL, protocol parsing, serialization,
    and similar mechanisms. Private helpers in the same file do not establish
    this separation.
  - **Names:** File names identify their main responsibility; function names
    distinguish business intent, decisions or calculations, and technical
    mechanisms. Demonstrate ambiguity through a caller or maintenance task, not
    naming preference alone.
  - **Change locality:** Trace a relevant business-rule change and a provider or
    storage change to check whether each stays with its owner or crosses
    boundaries unnecessarily—including whether frequently changing rules or
    policies force edits to stable workflow orchestration. Inspect dependency
    direction, cycles, and existing mechanical boundary checks.
- *Intermediate deliverable:* Candidate findings with scenario-backed cost
  analysis and verified evidence.
- *Stopping condition:* Every retained candidate has concrete code locations and
  a demonstrated caller, failure, or change cost.

### 3. Prioritize findings and frame directions
- Rank findings by benefit, risk, and change cost, including whether retaining
  the current structure is preferable.
- Frame credible improvement directions and changes to caller obligations,
  ownership, and failure semantics without prescribing finalized signatures, a
  mandatory file tree, or a full migration plan.
- Sketch a representative caller interaction only when it clarifies a trade-off.
- *Intermediate deliverable:* Ranked findings with improvement directions,
  trade-offs, and verification needs.
- *Stopping condition:* Priorities and directions are grounded in evidence,
  major costs and verification needs are explicit, and out-of-scope design or
  implementation work is deferred.

## Verification

Do not consider the review complete until all criteria below are satisfied.

### Observable evidence
- Every finding cites specific code locations and at least one concrete caller,
  failure, or change scenario.
- Structural claims are verified against actual imports, callers, and
  implementations—not file length, file names, or helper visibility alone.

### Validation criteria
- **Evidence over assumption:** Verified observations, assumptions, inspection
  limits, and exclusions are explicitly distinguished.
- **Proportionality:** Each recommendation weighs benefit against compatibility
  risk, migration cost, and test-coverage impact; retaining current structure is
  treated as a valid outcome.
- **Boundary discipline:** No finalized signatures, mandatory target file trees,
  full migration plans, or code edits are produced.
- **Actionable handoff:** Caller obligations, failure semantics, and
  verification needs are clear enough for `software-design` to act on when
  redesign is chosen.

### Delivery constraints
- For each ranked finding, include:
  1. Code locations and a concrete caller or change scenario.
  2. The structural problem and who currently bears its knowledge or change cost.
  3. Whether a change is worthwhile and credible improvement directions.
  4. Changes to caller obligations and ownership (conditions, outcomes, failure
     semantics).
  5. Major costs and verification needs.
- State inspection limits, important exclusions, and any missing code or
  unreadable references; keep affected claims explicitly provisional.
- Stop after delivering the assessment; do not proceed to target design or code
  changes.

## References

Read references only under these conditions. Reading alone is not diagnosis.

| Condition | Reference |
| --- | --- |
| Before evaluating candidate findings for shared definitions and design constraints | [design-principles.md](references/design-principles.md) |
| Dependency substitution, seam changes, or test migration | [dependencies-and-verification.md](references/dependencies-and-verification.md) |
| Defaults, idempotency, fallback, retries, or error aggregation affect a contract | [error-semantics.md](references/error-semantics.md) |
| Credible designs differ in caller obligations, state ownership, or dependency direction, or alternatives are requested | [alternatives.md](references/alternatives.md) |

If a required reference cannot be read, report its path and mark affected
judgments as provisional.
