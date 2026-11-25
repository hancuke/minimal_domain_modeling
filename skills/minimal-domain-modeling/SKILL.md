---
name: minimal-domain-modeling
description: Model business problems with the fewest sufficient concepts, facts, rules, responsibilities, lifecycles, and collaborations. Use to clarify business requirements or review business boundaries, using existing code as evidence when relevant. Cover whole systems or local problems. Leave software structure to software-design and approved code changes to implement.
---

# Minimal Domain Modeling

Build a clear business problem model using the fewest sufficient concepts, facts,
rules, responsibilities, lifecycles, and collaborations. Clear rules and boundaries
matter more than concept count.

## Contract

- **Required inputs:** Business problem statement, desired outcomes, and requirements
  or scenarios. Relevant existing models or code serve as behavioral evidence, not
  architectural templates.
- **Non-goals (refuse):** Refuse requests to design software architecture, class
  hierarchies, database schemas/DDL, API contracts, or code implementations. Refuse
  purely technical refactorings that carry no business rule changes.
- **Handoff targets:** Hand off the confirmed model as a business contract to
  `software-design` for software structure and interface design. Hand off approved
  code changes to `implement`. Neither handoff alters confirmed business meaning.
- **Mandatory approval gates (must stop):**
  - **Unknowns and contradictions gate:** When missing facts or conflicting rules
    affect identity, key rules, lifecycles, ownership, or legal/retention obligations,
    stop and ask for user clarification. Do not invent business assumptions or default rules.
  - **Model confirmation gate:** Before finalizing a durable model artifact or handing
    off to design, present provisional assumptions and scenario validation results,
    then stop for explicit user confirmation.

## Principles

- **Minimal sufficiency:** Merge or remove a concept only if all required facts,
  rules, and boundaries remain expressible. Retain distinctions justified by
  independent identity, rules, or lifecycles, not hypothetical reuse.
  - **Good:** Keep an approval task when claiming, transfer, and expiry give it its own rules and lifecycle.
  - **Avoid:** Remove that task because pending work can be calculated from status flags; calculation does not replace independent lifecycle and facts.
- **Semantic clarity:** Model rules alongside facts and concepts. Separate business commitments from fulfillment mechanisms.
  - **Good:** Record cancelled quantities when part of an order is cancelled to preserve the remaining commitment.
  - **Avoid:** Mark the whole order cancelled with a note; partial cancellation must preserve the remaining commitment.
- **Clear responsibility:** Assign each rule, decision, and invariant to exactly one owning concept or collaboration policy.
  - **Good:** Have the loan policy evaluate credit limits and publish an eligibility decision referenced by other participants.
  - **Avoid:** Duplicate credit limit checks across the applicant, loan application, and disbursement process.
- **Evidence over assumption:** Ground concepts and rules in verified user requirements and actual code behavior. Explicitly isolate provisional assumptions.
  - **Good:** Mark edge-case timeout behavior as an open question when requirements are silent.
  - **Avoid:** Assume a 30-day expiration default to fill a specification gap without confirmation.
- **Scenario validation:** Validate boundaries and rules against concrete success paths and adversarial counterexamples.
  - **Good:** Test whether an approved request remains valid when its amount is edited past the threshold.
  - **Avoid:** Validate the model by checking only whether entity attribute names match domain terms.

## Workflow

Follow sequenced steps when creating a new model. For a focused review, execute only
the steps affected by the change.

### 1. Frame scope and boundaries
- Identify problem owners, current state, desired outcome, exclusions, and external dependencies.
- Separate the business problem from any proposed technical solution.
- *Intermediate deliverable:* Scope statement with explicit inclusions, exclusions, and blocking unknowns.
- *Stopping condition:* Scope and outcomes are bounded; all blocking ambiguities are either resolved or framed as explicit approval gates.

### 2. Discover concepts and clarify rules
- Identify candidate concepts, identity needs, essential facts, and ownership boundaries.
- Clarify business rules through four perspectives:
  - *Invariant:* Which observable business states are invalid?
  - *Precondition:* Who may perform this operation, under what conditions, and when must it be rejected?
  - *Decision rule:* Which facts and policies determine the conclusion or branch?
  - *Postcondition:* What business facts must hold immediately upon operation success?
- *Intermediate deliverable:* Candidate concept catalog with justified concepts and rule perspectives.
- *Stopping condition:* Every retained concept is justified; no confirmed business requirement is lost through simplification.

### 3. Organize responsibilities, lifecycles, and collaboration
- Assign operations and decision authority to owning concepts or cross-concept policies.
- Model lifecycles (creation, triggers, allowed and rejected transitions, terminal states).
- Define collaboration sequences: initiating action, participants, decisions, and resulting facts.
- When business facts, decisions, history, or commitments must remain available over time, read [Business facts and retention obligations](references/information-and-retention.md).
- *Intermediate deliverable:* Lifecycle state transitions and collaboration flows with explicit ownership.
- *Stopping condition:* Every business rule has an owner; cross-concept collaborations and retention commitments have explicit participants.

## Verification

Do not consider modeling complete until all criteria below are satisfied.

### Observable evidence
- Walk through at least one end-to-end success scenario and at least two distinct failure, rejection, or edge scenarios against the model.
- Trace inputs, rules applied, state transitions accepted or rejected, and resulting recorded facts.

### Validation criteria
- **Requirement coverage:** Every confirmed in-scope requirement maps directly to an owning concept, rule, or behavior, verified by a scenario.
- **Invariant preservation:** Every operation and state transition demonstrably preserves declared invariants.
- **Zero unconfirmed assumptions:** No provisional assumptions or default behaviors are presented as confirmed domain facts.

### Delivery constraints
- Use the four-section [Model template](assets/model-template.md) for full model documents.
- Present an integrated model of the business scope, not a change proposal or implementation plan. Update existing models in place.
- For focused reviews without a document request, provide compact findings directly in the response.
- Exclude task-specific scaffolding, scenario test logs, and technical implementation details from the model document.
