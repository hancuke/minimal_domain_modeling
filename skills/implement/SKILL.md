---
name: implement
description: Implement an approved software design or a bounded code change whose structure and behavior are already established. Use for coding, fixing, and refactoring with clear contracts. Keep business operations separate from technical mechanisms, place functions by responsibility, and verify behavior and module boundaries. Return material design decisions to software-design.
---

# Implement

Implement responsibilities, not just signatures. Code should make business
operations easy to find without exposing the technical steps they rely on.
More files do not by themselves improve boundaries.

## Execution contract

- Change code only when implementation is requested. Honor any design approval
  gate; a request to implement an unapproved proposal does not bypass that gate.
- Use the approved design, established behavior, compatibility constraints,
  and project conventions as inputs. Inspect relevant implementations, callers,
  and tests before editing.
- Small changes within established contracts do not require a separate design
  document. Do not invent an architecture exercise for a local fix.
- If implementation requires a material change to agreed responsibilities,
  interfaces, dependency direction, or behavior, stop the affected work and
  return that decision to `software-design` for approval. Do not silently
  redesign while coding.
- If a required skill or reference is unavailable, report the missing dependency
  and the affected work. Do not claim its workflow has been applied.

## Placement principles

- **Separate reasons to change.** Business operations express intent and own
  business decisions. Technical modules own mechanisms such as filesystem access,
  encoding, serialization, and protocol handling. Do not interleave their details
  in the same operation.
- **Distinguish format from meaning.** Malformed JSON and missing format fields
  belong to the format boundary. Whether an order may be cancelled belongs to
  business rules. Place validation by the knowledge it needs, not its name.
- **Hide technical knowledge.** Moving calls to helpers is insufficient if
  business callers still coordinate decoding, format selection, and technical
  error translation. Put that sequence behind the capability that owns it.
  Preserve failures callers need to handle; never turn failure into apparent success.
- **Give functions a discoverable home.** Find the existing owner first.
  Add a focused module only when no suitable owner exists. Do not default to the
  current entry-point file or a generic `utils` module.
- **Keep cohesive details together.** Small private helpers serving the same
  responsibility may share a file. Separate independently changing business and
  technical responsibilities into focused modules using the project's conventions.
  Do not impose line limits, one-function-per-file rules, or a fixed layer tree.
- **Earn abstractions.** Plain functions and concrete modules are sufficient
  unless the design needs a substitution seam. Avoid pass-through layers,
  speculative interfaces, and circular dependencies.

## Workflow

### 1. Locate responsibilities

Map the requested operations to their owning modules or files and allowed
dependencies. Include business rules, state, effects, format handling, and
failure ownership where relevant. Reuse an existing design map when sufficient.

Check representative callers to see what knowledge the interface must hide.
Resolve material ambiguity before editing. Keep the map brief; do not create
a separate document merely to restate obvious code.

**Exit:** Each changed responsibility has a home. Technical details will not
accumulate in a business entry point merely because that is where coding starts.

### 2. Implement the smallest coherent change

Follow the responsibility map and approved contracts. Reuse existing capabilities.
Keep dependent callers consistent and preserve business invariants, failure
semantics, and compatibility. Put new functions at their owner as they are written,
not in one large file awaiting a hypothetical cleanup.

For example, importing orders might use:

```text
orders/
  import-orders.ts    # Coordinates the business operation and its outcomes
  order.ts            # Owns order rules
  order-file.ts       # Hides file reading, decoding, and format handling
  order-format.ts     # Owns format parsing when it warrants a separate module
```

The business operation calls `readOrderFile(path)`, applies order rules, and
saves orders. It does not open files, choose encodings, or decode JSON inline.
The file reader reports read and format failures; the business operation decides
their business consequences.

This is not a mandatory file tree. A short private parser can stay in
`order-file.ts` when it serves that responsibility and has no independent reason
to change. Do not move business eligibility checks there merely because they
inspect parsed fields.

**Exit:** Code and callers implement the agreed behavior and responsibility map.

### 3. Verify behavior and boundaries

Run the project's relevant tests, type checks, linters, and builds as applicable.
Add focused coverage for changed behavior and important failure paths.
Do not remove existing coverage without preserving the constraints it protects.

Inspect the changed code and representative callers:

- Do business operations still contain low-level I/O or format handling?
- Have technical modules acquired business decisions?
- Must callers still coordinate technical steps that should be hidden?
- Can a maintainer locate a changed function by its business or technical role?
- Did the split introduce cycles, pass-through wrappers, or unnecessary public APIs?

Correct implementation drift within the approved design. Return material design
changes for approval. Passing tests alone does not establish sound boundaries,
and file splitting alone does not establish correct behavior.

**Exit:** Report the implemented change, executed checks, and any blockers or
unverified behavior. Claim completion only when both behavior and boundary
checks support it.
