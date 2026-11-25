---
name: implement
description: Implement an approved software design or a bounded code change whose structure and behavior are already established. Use for coding, fixing, and refactoring with clear contracts. Keep business operations separate from technical mechanisms, place functions by responsibility, and verify behavior and module boundaries. Return material design decisions to software-design.
---
# Implement — Code Structure & Interface Discipline

## Goal

Translate approved designs or bounded changes with established structure and
behavior into maintainable code modules, files, and functions.

Preserve design boundaries in the actual codebase. Do not collapse business behavior and technical mechanisms into monolithic files.

## Execution contract

- Change code only when implementation is requested. Honor any design approval
  gate; an implementation request alone does not approve an unapproved design.
- Use the approved design or established contracts, compatibility constraints,
  and project conventions. Local fixes within those contracts do not require
  a separate review, design phase, or design document.
- If implementation requires material changes to responsibilities, module
  boundaries, public contracts, dependency direction, failure semantics, or
  behavior, pause the affected work. Return the decision to `software-design`
  with evidence and seek approval before proceeding.
- Report missing required skills or references and the affected work.
  Do not claim their workflows have been applied.

## Principles

1. **Confirm responsibilities before coding** — Inspect existing component responsibilities and contracts; implement them rather than redesigning them.
2. **Business directories, cohesive files** — Group code by business capability, then separate independently changing responsibilities inside it. Reuse sound project structure; do not scatter a feature across generic technical directories by default.
3. **Separate business from technology** — Business entry points express intent and coordinate capabilities; separate files own concrete I/O, protocols, and format handling. Distinguish format validation from business decisions. Private technical helpers in the business entry-point file do not establish this boundary.
4. **Explicit interfaces** — Expose only the contracts required by collaborating components.
5. **Hide implementation details** — Keep private helpers and technical sequences inside their owning modules without hiding failures callers must handle.
6. **Cohesion over fragmentation** — Avoid both God Files and unnecessary one-function-per-file decomposition.
7. **Names reveal responsibility and abstraction** — Use established project vocabulary and conventions. Name public business operations by intent, rules by the decision or calculation, and technical implementations by mechanism. File names identify their main responsibility. Avoid ambiguous `handle`, `process`, or `manager` names when they hide that distinction; do not rename unrelated code for stylistic uniformity.
8. **Comments add missing knowledge** — Explain non-obvious contracts, constraints, or reasons near their owner. Do not restate code or require a comment before every function.

**Good:** Distinguish `timeoutMs` from an attempt count. Document a range's
exclusive end or a retry's safety assumption when the code does not express it.
**Avoid:** Replace established business terms with generic `data` or `manager`,
or add comments that merely repeat method names. Match existing conventions,
but do not copy defects or force different semantics into one pattern.

## Workflow

1. Inspect the approved design or established behavior, relevant implementations,
   representative callers, and tests. For a cross-session design document, apply
   the intake checks below before coding.
2. Before implementation, establish concrete placement. For a new feature with
   multiple responsibilities, or a change involving both business decisions or
   workflows and technical I/O, present a concise Code Placement Map:
   - Show the business directory and files to create or reuse, with each file's
     responsibility and representative function names.
   - Distinguish public business entry points, internal rules, and technical
     implementations; identify which entry points outside callers may use.
   - Identify existing interfaces to reuse and allowed dependency directions.
     Business code depends on the capability it needs, not provider protocols
     or storage formats; keep concrete wiring at the established composition point.
   - Reuse the approved design; do not redefine contracts or responsibility
     boundaries. Establish placement before writing implementation, not afterward.
   - Keep the map in the response or existing design; no separate document is
     required. A local fix within established responsibilities can reuse sound
     placement without a new map. Return material boundary changes for approval.
     Reuse a supplied approved layout as the map after checking it against the
     current repository; fill necessary placement gaps rather than regenerating it.
3. Implement the smallest coherent change within those decisions. Keep dependent
   callers consistent and preserve business invariants, failure semantics, and compatibility.
4. Review changed code and representative callers against the placement map or
   established boundaries. Apply the structural acceptance checks below; inspect
   actual imports and implementations, not only file names or helper visibility.
5. Run relevant project tests, type checks, linters, and builds as applicable.
   Add focused coverage for changed behavior and important failures. Preserve
   the constraints protected by existing tests when migrating coverage.

## Cross-session design intake

Treat the supplied document as the design contract, not as evidence that code
has already been implemented or verified.

- Read its goal, scope, contracts, boundaries, approval status, and unresolved
  questions. Confirm that implementation is requested and approval is established;
  document presence or a "ready" label alone does not satisfy the approval gate.
- Resolve referenced repository files and compare the design's assumptions,
  integration points, and any recorded revision with the current code and tests.
  Verify relevant tooling and commands rather than assuming old checks still apply.
- Proceed without reconstructing the previous conversation when the document and
  repository establish the required context. Resolve local implementation details
  through inspection; ask only about missing decisions that materially affect the
  approved behavior, contracts, or boundaries.
- If repository drift or a missing decision invalidates the design, pause the
  affected work and return the specific discrepancy to `software-design`.
  Seek renewed approval for material changes, not for incidental file drift.

Keep the canonical design current when updates are requested or required by
project workflow. Do not create a second handoff document or treat a document's
verification plan as proof that verification has run.

## Placement example

For an order-placement capability, a possible structure is:

```text
orders/
  place-order.ts             # Public business operation: placeOrder
  pricing-policy.ts          # Business calculation: calculateOrderTotal
  order-store.ts             # Required storage contract, if a seam is needed
  infrastructure/
    postgres-order-store.ts  # SQL and row mapping: insertOrderRow
```

`placeOrder` coordinates pricing and saving through the required capability;
the storage implementation owns SQL, row mapping, and provider error translation.
Expose meaningful business failures without leaking provider-specific recovery
to callers. Define a contract only when collaboration or dependency isolation
requires it; reuse an existing seam rather than adding a redundant interface.

**Avoid:** Put `placeOrder`, `calculateOrderTotal`, SQL, and row decoding in
`order-service.ts`, then claim separation because the SQL helpers are private.
Also avoid scattering order-specific code into project-wide `services/`,
`repositories/`, and `utils/` directories without a project-specific reason.

This is a responsibility example, not a required language, file tree, or layer
template. Keep cohesive pure helpers with their owner; do not create one file
per function or a facade that only forwards calls without protecting a boundary.

## Constraints

- Do not implement concrete storage, network protocols, or serialization in the
  same source file as the public business workflow, even for the same feature.
  Business-level coordination of those capabilities belongs in the workflow;
  their technical sequences belong behind the boundary.
- Do not let business workflows depend on provider-specific APIs or formats.
  Preserve established compatibility through adapters or return material
  dependency changes for design approval; implementation convenience is not
  a reason to bypass the boundary.
- Do not create generic utility modules as dumping grounds.
- Do not create abstractions without a concrete responsibility or collaboration need.
- Do not blindly follow an existing poor file organization.
- Adjust private helpers, internal naming, local algorithms, and file placement
  within agreed responsibilities and contracts. Correct implementation drift,
  but return material design changes for approval rather than silently adopting
  better boundaries.

## Completion Criteria

Check the changed area for all of the following:

- Placement matches confirmed responsibilities; file and function names distinguish
  business intent, rules, and technical mechanisms.
- Outside callers use the intended business entry points rather than internal
  technical implementations, except for designated wiring or integration code.
- Business entry-point files do not implement SQL, protocol parsing, or other
  isolated technical mechanisms, including inside private helpers.
- Trace a relevant business-rule change and a relevant provider or storage change.
  Rule changes stay with the rule owner; technical substitutions do not require
  rewriting business rules. Explain any necessary cross-boundary contract change.
- Imports respect the agreed dependency direction without cycles, unnecessary
  public APIs, or pass-through layers that protect no responsibility.

Use existing import restrictions, lint rules, or architecture tests where
available to verify mechanical boundaries. Add project-specific enforcement
only when in scope; textual guidance alone is not an enforcement mechanism.

Passing tests alone does not establish sound boundaries. Report the implemented
change, executed checks, blockers, and unverified behavior. Claim completion only
when both behavior and boundary checks support it.
