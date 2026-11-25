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
2. **One responsibility per module** — Organize files around cohesive responsibilities, not execution sequence.
3. **Separate business from technology** — Business workflows express intent; technical modules own I/O and format handling. Distinguish format validation from business decisions.
4. **Explicit interfaces** — Expose only the contracts required by collaborating components.
5. **Hide implementation details** — Keep private helpers and technical sequences inside their owning modules without hiding failures callers must handle.
6. **Cohesion over fragmentation** — Avoid both God Files and unnecessary one-function-per-file decomposition.
7. **Names support correct expectations** — Use established project vocabulary and conventions. Name the relevant role, outcome, or unit precisely enough for the scope; do not rename unrelated code for stylistic uniformity.
8. **Comments add missing knowledge** — Explain non-obvious contracts, constraints, or reasons near their owner. Do not restate code or require a comment before every function.

**Good:** Distinguish `timeoutMs` from an attempt count. Document a range's
exclusive end or a retry's safety assumption when the code does not express it.
**Avoid:** Replace established business terms with generic `data` or `manager`,
or add comments that merely repeat method names. Match existing conventions,
but do not copy defects or force different semantics into one pattern.

## Workflow

1. Inspect the approved design or established behavior, relevant implementations,
   representative callers, and tests.
2. Locate responsibilities with a concise Code Placement Map when useful:
   - Map confirmed responsibilities to concrete files and functions.
   - Identify existing interfaces to reuse and allowed dependencies.
   - Reuse the approved design; do not redefine contracts or responsibility boundaries.
   - Do not require a separate artifact for an obvious local fix.
3. Implement the smallest coherent change within those decisions. Keep dependent
   callers consistent and preserve business invariants, failure semantics, and compatibility.
4. Review changed code and representative callers for file boundaries, interface
   clarity, and dependency direction. Check for leaked technical coordination,
   misplaced business rules, cycles, and unnecessary public APIs or pass-through layers.
5. Run relevant project tests, type checks, linters, and builds as applicable.
   Add focused coverage for changed behavior and important failures. Preserve
   the constraints protected by existing tests when migrating coverage.

## Constraints

- Do not put unrelated business operations and technical operations in the same module.
- Do not introduce technical dependencies into business workflows without a clear reason.
- Do not create generic utility modules as dumping grounds.
- Do not create abstractions without a concrete responsibility or collaboration need.
- Do not blindly follow an existing poor file organization.
- Adjust private helpers, internal naming, local algorithms, and file placement
  within agreed responsibilities and contracts. Correct implementation drift,
  but return material design changes for approval rather than silently adopting
  better boundaries.

## Completion Criteria

Code structure reflects component responsibilities, interfaces are explicit, technical details are encapsulated, and business behavior can be located and tested independently.

Passing tests alone does not establish sound boundaries. Report the implemented
change, executed checks, blockers, and unverified behavior. Claim completion only
when both behavior and boundary checks support it.
