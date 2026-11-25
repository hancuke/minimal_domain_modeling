---
name: software-design
description: Design software structure and minimal interfaces from a domain model, confirmed requirements, or existing code and an improvement goal. Use for feature architecture, redesigning an implementation, or designing a selected refactor. Diagnose existing code as needed for the target design. Standalone code-structure assessment belongs to code-structure-review. Present the design for approval before handing requested code changes to implement.
---

# Software Design

Turn established business behavior into coherent code objects, modules, and
minimal interfaces. Diagnose relevant problems before redesigning existing code.
Hand off implementation only when requested and approved.

## Contract

- **Required inputs:** Domain model, confirmed feature requirements, or existing
  code with an improvement goal. (A domain model is a business-semantic contract,
  not a class blueprint; code boundaries may differ while preserving confirmed
  meaning. Domain modeling is not a mandatory prerequisite for technical refactoring.)
- **Non-goals (refuse):** Refuse requests to directly modify or implement code
  without an approved design. Refuse standalone code-structure assessment without
  target-design intent (refer to `code-structure-review`). Refuse inventing or
  deciding unconfirmed business meaning (refer to `minimal-domain-modeling`).
- **Handoff targets:** Hand off missing or contradictory business rules to
  `minimal-domain-modeling`. Hand off standalone code-structure assessment to
  `code-structure-review`. Hand off approved design, responsibilities, interfaces,
  and migration plans to `implement` for code changes.
- **Mandatory approval gates (must stop):**
  - **Business ambiguity gate:** When missing, contradictory, or unconfirmed
    business rules materially affect software structure, stop and ask for
    clarification or hand off to `minimal-domain-modeling`. Do not proceed with
    unverified assumptions.
  - **Design approval gate:** Before editing code or starting implementation,
    present the design and stop for explicit user approval. A code-change or
    design-document request alone is not design approval. Implement only when
    both approval and an implementation request are present. Renew approval for
    material changes to approved structure, interfaces, or behavior.

## Principles

- **Whole before parts:** Optimize software structure within the problem's scope
  before local details. Existing code is evidence, not a template. Reorganize or
  remove objects and responsibilities when that simplifies the structure.
  - **Good:** Consolidate fragmented state and transition logic across three helpers into a single coordinating component before refining method signatures.
  - **Avoid:** Polish local method signatures within an existing tangled class hierarchy without evaluating whether the hierarchy itself should be dissolved.
- **Small interfaces, deep modules:** Minimize the concepts, operations, and
  constraints callers must understand. Count that knowledge, not just methods.
  Generic entry points and parameter bags can move complexity to callers.
  - **Good:** Expose a single `processOrder(orderId)` entry point that internally coordinates inventory, pricing, and persistence.
  - **Avoid:** Expose `reserveInventory`, `calculateTax`, `applyDiscount`, and `saveOrder`, forcing callers to orchestrate ordering, transactions, and partial rollback.
- **Regular rules, clear flow:** Keep rules orthogonal. Use names that describe
  behavior. Make data flow easy to follow.
  - **Good:** Separate format parsing, business eligibility checks, and storage operations into distinct, sequentially composed steps with descriptive names.
  - **Avoid:** Interleave database queries and error logging inside an eligibility evaluation rule, or use ambiguous names like `processData`.
- **Only necessary structure:** Prefer simple functions, objects, or pipelines
  over speculative layers and options. Designing the complete structure does not
  require a whole-codebase rewrite.
  - **Good:** Use a straightforward pattern-matching function or decision table to handle three known notification types.
  - **Avoid:** Build an extensible notification plugin registry, abstract strategy factory, and dynamic configuration engine for a feature with three fixed types.
- **Problem before pattern:** Identify the rules, state, or variation that need
  explicit structure before choosing a design technique. Patterns are candidate
  solutions, not required deliverables. Sequential code is valid when it keeps
  the relevant rules and responsibilities clear.
  - **Good:** Introduce a state machine only after identifying multiple scattered transition rules and illegal state combinations.
  - **Avoid:** Apply the Command pattern simply because user actions exist, adding boilerplate commands that only forward calls to an existing service.

## Workflow

### 1. Confirm inputs and diagnose
- State the main job in one sentence. Identify required scenarios, acceptance criteria,
  scope, and compatibility needs.
- Distinguish business constraints from past implementation choices.
- When working with existing code without a supplied diagnosis, inspect relevant code,
  representative callers, and tests to identify who owns state, effects, rules, and
  failures. Cite code locations and change scenarios.
- When working with a supplied diagnosis or recommendation, reuse evidence and check
  only design-relevant assumptions.
- Clarify unknowns that materially change structure; isolate provisional assumptions.
- *Intermediate deliverable:* Problem and scope statement with concrete constraints,
  verified code evidence, and explicit provisional assumptions.
- *Stopping condition:* Scope and significant constraints are explicit; blocking
  business questions are resolved or isolated; existing-code improvements have
  concrete evidence and justified direction.

### 2. Design software structure and derive interfaces
- Map confirmed business concepts and behavior to code objects, modules, and minimal
  interfaces. Define explicit ownership of business rules, state, technical I/O,
  and side effects.
- Group by business capability before separating internal technical mechanisms.
- Identify complexity needing explicit representation: state-dependent lifecycles,
  independently varying rules, external boundaries, and multi-step retries/recovery.
- Read [state-and-transitions.md](references/state-and-transitions.md) when legality
  depends on lifecycle state or transition rules are scattered. Consult relevant
  shared references when applicable.
- Derive the minimal caller interfaces: operations, inputs, results, usage constraints,
  and failure semantics. Make dependency directions strictly unidirectional.
- Show directory and key file layout when requested, when preparing cross-session
  handoff, or when physical placement clarifies a boundary (leaving internal details
  to `implement`).
- *Intermediate deliverable:* Structural model with component responsibilities,
  dependency directions, state/effect owners, and minimal caller interfaces.
- *Stopping condition:* Target structure supports all required scenarios; ownership
  of rules and effects is unambiguous; public business entry points and internal
  mechanisms are cleanly partitioned.

### 3. Challenge solution and plan migration
- Trace normal, failure, and change scenarios through the proposed interfaces (e.g.,
  rejected transitions, rule changes, partial failure recovery).
- Verify that the design removes complexity rather than shifting it to callers.
- Justify any proposed pattern against a credible simpler baseline under identical constraints.
- Plan caller migration, backward compatibility, and recovery for deployment and data risks.
  Identify behavior-protecting tests, types, and integration checks.
- *Intermediate deliverable:* Risk and scenario analysis, migration plan, verification
  expectations, and identified trade-offs.
- *Stopping condition:* Added complexity is justified against a simple baseline; failure
  and edge scenarios are addressed; migration and verification paths are actionable.

## Verification

Do not consider design complete until all criteria below are satisfied.

### Observable evidence
- Trace at least one end-to-end success path and at least two failure, rejection, or edge
  scenarios (e.g., illegal state transitions, downstream failure, rule evolution) through
  the derived interfaces.
- Demonstrate that business callers depend only on business capabilities, with technical
  protocols, SQL, and serialization isolated behind internal owners.

### Validation criteria
- **Caller burden minimization:** Callers need only minimal operations and inputs;
  orchestration of internal steps and partial recovery is hidden within deep modules.
- **Responsibility & ownership clarity:** Every business rule, state mutation, and side
  effect has a single explicit owner.
- **Dependency discipline:** Dependencies flow inward toward business concepts or downward
  along capability abstractions without cycles or pass-through layers.
- **Explicit failure semantics:** Failure modes, idempotency, retries, and error types
  are clearly bounded and actionable for callers.
- **Zero ungrounded assumptions:** All provisional technical choices are highlighted;
  business questions are resolved.

### Delivery constraints
- **In-session design (default):** Present the target structure, minimal interfaces,
  key trade-offs, and migration needs concisely in the response. Honor specific scope
  constraints (e.g., "only key interfaces"). Do not create or edit files unless explicitly requested.
- **Cross-session implementation document (on request):** When a durable handoff or
  document is requested, create or update a single canonical document containing: goal,
  scope, non-goals, confirmed business rules, code paths/integration points, target directory
  and file responsibilities, key interfaces with failure semantics, trade-offs/rationale,
  migration/verification steps, and explicit approval status. Do not mark an incomplete or
  unapproved design ready for independent implementation.
- Stop for user review after presenting the design. Do not begin code modification or
  invoke `implement` until explicit user approval is granted.

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
