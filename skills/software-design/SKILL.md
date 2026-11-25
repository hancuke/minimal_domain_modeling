---
name: software-design
description: Design software structure and minimal interfaces from a domain model, confirmed requirements, or existing code and an improvement goal. Use for feature architecture, redesigning an implementation, or designing a selected refactor. Diagnose existing code as needed for the target design. Standalone code-structure assessment belongs to code-structure-review. Present the design for approval before handing requested code changes to implement.
---

# Software Design

Turn established business behavior into coherent code objects, modules, and
minimal interfaces. Diagnose relevant problems before redesigning existing code.
Hand off implementation only when requested and approved.

## Execution contract

- Treat confirmed requirements, business rules, and visual design as inputs.
  State contradictions and ask for clarification.
- A domain model is a business-semantic contract, not a class blueprint. Code
  boundaries may differ while preserving the confirmed business meaning.
- Accept a domain model, clear requirements without a separate model document,
  or existing code with an improvement goal. Domain modeling is not a mandatory
  prerequisite for a feature or a behavior-preserving technical refactor.
- Use `minimal-domain-modeling` to resolve missing or contradictory business meaning
  that affects the design. Revisit only the affected rules.
  Do not let unrelated business questions block established behavior.
- Reuse a supplied diagnosis and selected recommendation.
  Check assumptions relevant to the design. Otherwise inspect the relevant existing code.
  Use `code-structure-review` for standalone assessment of existing code structure.
  A target-design request does not require a separate review phase or skill invocation.
- **Approval gate:** Before editing code, present the design and stop for
  explicit user approval. A code-change request alone is not design approval.
  Implement only when both are present. Seek renewed approval for material
  changes to the approved software structure, interface, or behavior.
- Design-only requests end with the design. Assessment-only requests end with
  findings. Neither authorizes implementation changes.
- Default to delivering the design in the conversation. Create or update a
  design file only when requested or required by an established project workflow.
  A request for a design document authorizes that artifact, not implementation.

## Design principles

- **Whole before parts.** Optimize the software structure within the problem's
  scope before local details. Existing code is evidence, not a template.
  Reorganize or remove objects and responsibilities when that simplifies the structure.
- **Small interfaces, deep modules.** Minimize the concepts, operations, and
  constraints callers must understand. Count that knowledge, not just methods.
  Generic entry points and parameter bags can move complexity to callers.
- **Regular rules, clear flow.** Keep rules orthogonal.
  Use names that describe behavior. Make data flow easy to follow.
- **Only necessary structure.** Prefer simple functions, objects, or pipelines
  over speculative layers and options. Designing the complete structure does not require
  a whole-codebase rewrite.
- **Problem before pattern.** Identify the rules, state, or variation that need
  explicit structure before choosing a design technique. Patterns are candidate
  solutions, not required deliverables. Sequential code is valid when it keeps
  the relevant rules and responsibilities clear.

## Delivery modes

Use the same design standards in both modes. Adapt the deliverable, not the
quality of the reasoning or the approval gate.

**In-session design (default):** Present the information needed for the current
decision in the response. Honor requests such as "only key interfaces and the
directory structure"; include consequential constraints, failure semantics, and
compatibility obligations alongside the affected interfaces. Do not expand a
bounded design into a full document or create a file by default.

**Cross-session implementation document (on request):** When the user requests a
durable design or a handoff for a new session, write or update one canonical
document using the requested location or established project convention. For an
implementation handoff, include enough context for an agent with the document
and the specified repository, but no chat history, to inspect and implement:

- Goal, scope, non-goals, and necessary confirmed business rules.
- Repository and relevant code or documentation paths, integration points, and
  runtime or tooling constraints; identify the inspected revision when useful.
- Target directory structure, file responsibilities, public business entry
  points, internal implementation owners, and dependency direction.
- Key interfaces, inputs, results, usage constraints, and failure semantics.
- Settled design decisions and the rationale needed to preserve their intent.
- Applicable compatibility and migration obligations, acceptance scenarios,
  and relevant verification commands with their working directories.
- Unresolved questions, assumptions, and explicit approval status, including
  which work remains blocked.

Include only applicable material, not empty template sections or a line-by-line
implementation script. Use durable repository references instead of copying
source code or relying on "as discussed above." A shorter durable design is valid
when the user is not requesting an independently actionable implementation handoff.
Do not label an incomplete or unresolved design ready for independent implementation.

Record approval only when actually given; document creation, a "ready" label,
or receipt in another session does not itself authorize implementation.
Keep an existing design current rather than creating duplicate handoff documents.

## Workflow

### 1. Confirm inputs and constraints

State the main job in one sentence. Identify required scenarios, acceptance
criteria, scope, and compatibility needs. Inspect relevant code, terminology,
conventions, and tests. Distinguish business constraints from past implementation choices.
Select the delivery mode from the request; use in-session delivery when unspecified.

| Input | Starting path |
| --- | --- |
| Domain model or confirmed feature requirements | Extract behavior and constraints; inspect relevant integration points |
| Existing code and an improvement goal | Diagnose the bounded area before choosing the target structure |
| Selected refactoring recommendation | Reuse its evidence and direction; check only design-relevant assumptions |

For existing-code redesign without a supplied diagnosis, inspect implementations,
representative callers, and tests. Identify who owns state, effects, rules, and failures.
For each motivating problem, cite code locations and a caller or change scenario.
Compare credible directions by benefit, risk, and change cost.
Retaining the current structure is a valid outcome.
Expand inspection only when the target design needs evidence from another area.

Clarify unknowns that would materially change the software structure or behavior.
Separate business decisions from technical choices.
Use the domain-modeling handoff for business decisions.
If clarification is unavailable, mark the affected design as provisional.

**Exit:** The problem and significant constraints are explicit.
Unresolved questions limit the affected design.
Existing-code improvements have evidence and a justified direction.

### 2. Design the software structure and derive interfaces

Map established business concepts and behavior to code objects, modules, and
interfaces. Define software responsibilities and ownership of state and effects.
Preserve confirmed business invariants and lifecycles.
Identify which complexity needs explicit representation:

- State-dependent behavior and legal transitions.
- Business decisions or algorithms that vary independently.
- External dependencies that need a stable boundary.
- Multi-step work with partial completion, retry, or recovery.

Use only the relevant perspectives, not a mandatory checklist of patterns.
Choose the simplest structure that makes the constraints explicit. A function,
decision table, or transition table may be sufficient; a class hierarchy,
framework, or generic engine needs additional justification.
Read the [state and transitions reference](references/state-and-transitions.md)
when operation legality depends on lifecycle state or transition rules are
scattered. Use existing shared references for consequential dependency boundaries
and alternative comparisons; do not load unrelated guidance.

Compare coherent structures before deciding local details.
Existing code boundaries can change. A business concept does not require its own class.

Own the target responsibilities, contracts, and dependency direction.
Show the target directory and key file layout when requested, when preparing an
independent implementation handoff, or when physical placement clarifies a
consequential boundary. Leave remaining file and private function placement to
`implement` within those decisions; do not require a complete file tree for every
in-session design.

Make the owners of business operations, business rules, technical I/O, and format
handling explicit where relevant. Show which capability hides technical sequences
from business callers. Identify public business entry points and which
implementations must remain internal. Business workflows coordinate capabilities;
concrete storage, protocols, and serialization have separate implementation owners,
even when they serve the same feature. Distinguish format validation from business
decisions. Group by business capability before separating internal technical
responsibilities, while respecting sound project conventions. Do not prescribe
one file per function or introduce layers only to separate names.

Derive the minimum interface needed by callers: operations, inputs, results,
usage constraints, and failure semantics. Make dependency direction clear.
Use project terminology.

Treat compatibility as a migration obligation, not a limit on exploring better
abstractions. Identify proposed contract changes for explicit approval.

**Exit:** The structure and interfaces support required scenarios.
Ownership of business rules, state, and effects is clear.

### 3. Challenge the solution and plan migration

Trace normal, failure, and change scenarios through the proposed interfaces.
Where relevant, exercise rejected transitions, changes to decision rules, and
partial failure or retry. Check that the design prevents invalid outcomes and
makes recovery responsibilities explicit.
Apply the principles. Read the [references](#references) when their conditions apply.
Check whether the structure removes complexity or shifts it to callers.
When proposing a pattern or mechanism, compare it with a credible simpler
implementation under the same constraints. Explain which rule it protects or
which required variation it isolates, and the cost it adds. Do not invent a
complex alternative for an already sufficient simple design.

Compare the target structure with the current implementation.
Plan caller migration, compatibility checks, and recovery for deployment and data risks.
Identify tests, types, and integration checks that protect behavior.
Map existing coverage when refactoring.

**Exit:** Added complexity is justified. Significant trade-offs are explicit.
Migration and verification cover important risks.

### 4. Present the design and await approval

Deliver the design in the selected mode and requested level of detail.
Present the target structure and interfaces with representative usage when useful.
Explain important differences from the current design.
Include applicable migration and verification needs; an independent implementation
handoff must make these actionable without the conversation history.
Use signatures, a diagram, or code sketches only when they clarify the decision.
Explain significant trade-offs, including consequential design-technique choices.
Integrate these decisions into the design; do not add a mandatory pattern report
or separate design-method document.

For existing-code improvements, include the motivating evidence and expected
benefit. These findings support the design. They do not claim a code change.
State unresolved assumptions separately from approval status. For implementation
requests, ask for approval and stop under the approval gate.

**Exit:** The design is submitted for review in the requested form.
Submission completes the design phase. It does not authorize implementation.

### 5. Hand off for implementation

Start implementation only after the approval gate is satisfied and key
uncertainties are resolved. For requested same-session implementation, load
`implement` and hand off the approved responsibilities, interfaces, dependency
direction, state and effect ownership, failure semantics, compatibility constraints,
and migration and verification needs. Explicitly identify public business entry
points, internal rule owners, technical implementation owners, and where concrete
dependencies are wired. Reuse the approved design; do not create a duplicate
handoff document.

For a future-session handoff, deliver the document under the delivery-mode
contract and stop; do not start implementation or require a same-session skill
invocation. A draft can be delivered before approval if its pending status and
blockers are explicit.

`implement` owns concrete file and function placement within the approved
responsibilities and contracts, including the pre-implementation Code Placement
Map for multi-responsibility features or mixed business and I/O changes.
Reuse an approved layout as that map rather than requiring it to be regenerated;
`implement` checks it against the current repository and fills only necessary gaps.
It also owns implementation and behavior and boundary checks.
If implementation invalidates a design assumption, revisit only the affected
design and seek renewed approval for material changes.

If `implement` is unavailable, report the missing dependency and stop before coding.

**Exit:** For same-session execution, implementation is handed to `implement`
with the approved contract. For future-session execution, the durable design is
delivered with its actual approval status and any blockers.
Implementation completion and verification are reported under `implement`'s
execution contract, not claimed by delivering a design.

## References

Read references only under these conditions. Reading alone is not diagnosis.

| Condition | Reference |
| --- | --- |
| Operation legality depends on lifecycle state, or transition rules are scattered | [state-and-transitions.md](references/state-and-transitions.md) |
| Module boundaries, dependency isolation, or consequential architectural choices | [design-principles.md](../code-structure-review/references/design-principles.md) |
| Dependency substitution, seam changes, or test migration | [dependencies-and-verification.md](../code-structure-review/references/dependencies-and-verification.md) |
| Defaults, idempotency, fallback, retries, or error aggregation affect a contract | [error-semantics.md](../code-structure-review/references/error-semantics.md) |
| Alternatives requested, or designs differ consequentially in caller obligations, state ownership, or dependency direction | [alternatives.md](../code-structure-review/references/alternatives.md) |

If a required reference cannot be read, report its path.
Mark affected judgments as provisional.
