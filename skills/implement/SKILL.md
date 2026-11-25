---
name: implement
description: Implement an approved software design or a bounded code change whose structure and behavior are already established. Use for coding, fixing, and refactoring with clear contracts. Keep business operations separate from technical mechanisms, place functions by responsibility, and verify behavior and module boundaries. Return material design decisions to software-design.
---
# Implement

Translate approved designs or bounded changes with established structure and
behavior into maintainable modules, files, and functions while preserving
design boundaries.

## Contract

- **Required inputs:** An approved software design, or a bounded change request
  backed by established contracts, compatibility constraints, and project
  conventions. (Local fixes within established contracts do not require a
  separate design phase or document.)
- **Non-goals (refuse):** Refuse code modifications without an implementation
  request or when required design approval is missing. Refuse redesigning module
  boundaries, altering public contracts, or inventing unconfirmed business rules
  during implementation. Refuse creating generic utility dumping grounds or
  speculative abstractions without a concrete responsibility.
- **Handoff targets:** Return material changes to responsibilities, module
  boundaries, public contracts, dependency direction, failure semantics, or
  behavior to `software-design` with evidence. Hand off missing or contradictory
  business rules to `minimal-domain-modeling`. Hand off standalone structural
  assessment to `code-structure-review`.
- **Mandatory approval gates (must stop):**
  - **Unapproved change gate:** Stop if implementation is not explicitly
    requested or if a required design lacks approval. An implementation request,
    document presence, or a "ready" label alone does not approve an unapproved
    design.
  - **Design drift gate:** Stop the affected work if repository drift, missing
    decisions, or implementation discoveries require material changes to
    approved boundaries, public contracts, dependency direction, or failure
    semantics. Return the specific discrepancy to `software-design` and seek
    renewed approval before proceeding. (Incidental file drift that does not
    invalidate contracts may be resolved locally.)
- **Missing dependencies:** Report any missing required skills or references and
  the affected work explicitly; never claim their workflows have been applied.

## Principles

- **Separate business intent from technical mechanism:** Business entry points
  express intent and coordinate capabilities; separate files own concrete
  storage, network protocols, serialization, and format handling. Distinguish
  format validation from business decisions, and keep business workflows
  independent of provider-specific APIs or formats. Private technical helpers
  inside a business entry-point file do not establish this boundary.
  - **Good:** `orders/place-order.ts` (`placeOrder`) coordinates
    `pricing-policy.ts` (`calculateOrderTotal`) and an `order-store.ts`
    capability contract, while `infrastructure/postgres-order-store.ts`
    (`insertOrderRow`) owns SQL, row mapping, and provider error translation.
  - **Avoid:** Put `placeOrder`, `calculateOrderTotal`, SQL queries, and row
    decoding together in `order-service.ts` and claim separation because the SQL
    helpers are private.
- **Boundary-first placement and cohesion:** Group code by business capability,
  then separate independently changing responsibilities inside it. Reuse sound
  project structure, but do not copy poor organization; avoid both God Files and
  one-function-per-file fragmentation.
  - **Good:** Group order operations, pricing policy, and order persistence
    under `orders/`, keeping cohesive pure helpers with their owning module.
  - **Avoid:** Scatter order-specific logic across project-wide `services/`,
    `repositories/`, and `utils/` directories without a project-specific reason,
    or create pass-through facades that protect no boundary.
- **Minimal caller burden and explicit seams:** Expose only the contracts
  required by collaborating components, keeping concrete wiring at the
  established composition point. Hide private helpers and technical sequences
  inside their owning module without hiding business failures callers must
  handle. Reuse existing seams rather than adding redundant interfaces.
  - **Good:** Expose `placeOrder` with meaningful business failure semantics,
    keeping transaction ordering and provider error translation behind the
    storage boundary.
  - **Avoid:** Expose low-level persistence steps or leak provider-specific
    recovery to callers, or add an abstraction interface with no collaboration
    or dependency-isolation need.
- **Names and comments reveal responsibility and missing knowledge:** Follow
  established project vocabulary without renaming unrelated code for stylistic
  uniformity. Name public business operations by intent, rules by decision or
  calculation, and technical implementations by mechanism. Use comments only for
  non-obvious contracts, constraints, or rationale near their owner.
  - **Good:** Distinguish `timeoutMs` from attempt counts, name files after
    their main responsibility, and document an exclusive range end or retry
    safety assumption not expressed by code.
  - **Avoid:** Use ambiguous `handle`, `process`, `data`, or `manager` names
    that obscure responsibility, or add comments that merely restate method
    names.

## Workflow

### 1. Inspect and validate contract
- Inspect the approved design or established behavior, relevant implementations,
  representative callers, and existing tests.
- **Cross-session design intake (when implementing from a design document):**
  Treat the document as a design contract, not proof of implementation or
  verification. Confirm goal, scope, contracts, approval status, and unresolved
  questions; compare assumptions, integration points, and recorded revisions
  against current code, tests, and tooling. Resolve local details by inspection;
  trigger the *Design drift gate* if drift or missing decisions invalidate the
  design. Update the canonical design only when requested or required, and never
  create a second handoff document.
- *Intermediate deliverable:* Verified implementation scope, contract
  boundaries, and any discrepancies requiring escalation.
- *Stopping condition:* Approval is confirmed, current code and tests match
  design assumptions, and no blocking design drift or missing decision remains.

### 2. Establish Code Placement Map
- Before writing code for a new feature with multiple responsibilities, or any
  change combining business decisions/workflows with technical I/O, present a
  concise Code Placement Map in the response. Reuse and verify an approved
  layout rather than regenerating it, filling only placement gaps; local fixes
  within sound existing placement may skip a new map.
- Include:
  - Business directory and files to create or reuse, with each file's
    responsibility and representative function names.
  - Distinction between public business entry points, internal rules, and
    technical implementations, identifying allowed external entry points.
  - Reused interfaces, allowed dependency directions, and concrete wiring at the
    established composition point.
- *Intermediate deliverable:* Code Placement Map aligned with confirmed
  responsibilities and contracts.
- *Stopping condition:* Every planned file has a clear owner, unidirectional
  dependencies, and no unapproved boundary changes.

### 3. Implement the smallest coherent change
- Implement within approved contracts and the placement map, adjusting only
  private helpers, internal naming, local algorithms, and file placement within
  agreed responsibilities.
- Keep dependent callers consistent and preserve business invariants, failure
  semantics, and backward compatibility.
- Add focused test coverage for changed behavior and important failure modes
  while preserving constraints protected by existing tests.
- *Intermediate deliverable:* Implemented code and focused tests matching the
  placement map.
- *Stopping condition:* All planned responsibilities are implemented without
  leaking technical mechanisms across boundaries or altering unapproved
  contracts.

## Verification

Do not consider implementation complete until both behavior and boundary checks
are satisfied. Passing tests alone does not establish sound boundaries.

### Observable evidence
- Run relevant project tests, type checks, linters, builds, and any available
  import-restriction or architecture checks. Do not treat a design document's
  verification plan as executed evidence.
- Inspect actual imports and implementations—not just file names or helper
  visibility—across changed files and representative callers.
- Trace one relevant business-rule change and one provider or storage change to
  verify that rule changes stay with the rule owner and technical substitutions
  do not require rewriting business rules.

### Validation criteria
- **Responsibility & naming alignment:** Placement matches confirmed
  responsibilities; file and function names distinguish business intent, rules,
  and technical mechanisms.
- **Entry-point discipline:** Outside callers use intended business entry points
  rather than internal technical implementations, except designated wiring or
  integration code.
- **Boundary integrity:** Business entry-point files contain no SQL, protocol
  parsing, or isolated technical mechanisms, including inside private helpers.
- **Dependency hygiene:** Imports follow agreed dependency directions without
  cycles, unnecessary public APIs, or pass-through layers that protect no
  responsibility.
- **Behavioral & contract preservation:** Checks pass, business invariants and
  failure semantics hold, and any necessary cross-boundary contract change is
  explicitly explained and approved.

### Delivery constraints
- Report: (1) implemented changes and file-level responsibility placement,
  (2) executed checks and observed results, and (3) blockers, discrepancies, or
  unverified behavior.
- Add project-specific mechanical enforcement (lint or import rules) only when
  explicitly in scope; textual guidance alone is not an enforcement mechanism.
- Claim completion only when both behavioral checks and structural boundary
  checks support it.
