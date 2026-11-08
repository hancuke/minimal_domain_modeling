---
name: minimal-domain-modeling
description: Model or simplify business concepts, responsibilities, rules, relationships, lifecycles, and retention requirements. Use when translating business requirements into a domain model or reviewing existing business boundaries. Leaves technical architecture and UI design to separate tasks.
---

# Minimal Domain Modeling

Preserve the required business meaning with the fewest justified concepts.
Optimize for understandable rules and boundaries, not object count.

## Principles

- **Minimal sufficiency:** Merge or remove a concept only when required facts,
  rules, and boundaries remain expressible.
- **Independent change:** Retain distinctions justified by independent identity,
  rules, or lifecycles, not hypothetical reuse.
- **Rule ownership:** Put a decision with the concept enforcing its constraint,
  not merely the concept holding related data. Define each rule once.
- **Bounded uncertainty:** Unconfirmed business rules support provisional choices,
  not a confirmed model.

**Good:** Keep an approval task when claiming, transfer, and expiry create its own
facts and lifecycle. **Avoid:** Remove it because a pending-task list is calculable.

**Good:** Model cancellable quantities for partially fulfilled orders.
**Avoid:** Replace that distinction with one order-level cancelled flag.

## Execution contract

- Model the business; leave implementation and architectural choices to a separate task.
- Use the user's business vocabulary. Resolve overloaded terms before depending on them.
- Label confirmed requirements, provisional assumptions, and open questions.
- Read supplied requirements and relevant existing models or code when available.
  Surface contradictions between them; ask which behavior is intended.
- For a new model, follow the stages below. For a focused review, inspect the
  affected stages and their dependencies rather than rebuilding the entire model.
- Revisit earlier stages when scenarios, capabilities, or lifecycles expose a
  missing distinction. The stage order is a starting path, not a one-way gate.

## 1. Frame the problem

State the current situation, desired outcome, core business problem, and scope.
Separate a proposed implementation from the need it serves, while preserving
external constraints that actually affect the business.

Identify unknowns before choosing concepts. Ask for clarification when an unknown
could change identity, a key rule, lifecycle, ownership, or retention. Ask only
the questions needed to resolve the current blocker.

If clarification is unavailable, present alternatives or a provisional model
with explicit assumptions and affected decisions.

**Exit:** The scope and intended outcome are stated; consequential unknowns are
resolved or explicitly bound the provisional work.

## 2. Discover and reduce concepts

For each candidate, establish its business meaning, essential facts, need for
identity, and relationships. Specify cardinality and ownership where they affect
rules or lifecycle; distinguish ownership from a simple association.

Use entities, value objects, and finite classifications when helpful. Business
events, rules, policies, and processes are also valid concepts when required by
the problem. These are analytical categories, not a mandate to create classes.

Apply minimal sufficiency and independent change to each candidate. Name the
required meaning it protects; explain consequential merges and exclusions.

**Exit:** Each retained concept has a business reason to exist; no confirmed
requirement has been removed in the name of simplicity.

## 3. Assign capabilities and rules

For important concepts, describe their responsibilities, supported business
operations, and invariants. For consequential operations, identify the conditions
for acceptance, resulting facts or state changes, and business rejection cases.

Apply rule ownership; refer to the authoritative rule from interactions and
scenarios.

For a decision spanning several concepts, describe a business-named rule, policy,
or process and its participants. A collaboration need does not by itself require
a Service, Repository, Controller, or technical layer.

**Exit:** Important rules have explicit owners or collaboration boundaries, and
operations have understandable business effects.

## 4. Explain collaboration and lifecycles

Use the relationships from stage 2 to describe essential business interactions:
the initiating action, participating concepts, decision points, and resulting
facts or state changes. Add only relationships newly revealed by this work.

For stateful concepts, identify creation, relevant states, transition triggers
and conditions, rejected transitions, and completion or termination. Keep related
but independent lifecycles separate.

Use concise text by default. Use a decision table for branching rules, a transition
table for state changes, or a diagram when it resolves a specific ambiguity.
Skip lifecycle artifacts for concepts without meaningful state transitions.

**Exit:** Essential operations can be followed end to end, and lifecycle
restrictions agree with the capabilities and invariants.

## 5. Classify information and retention

When the task introduces or changes business information, historical records,
or recovery expectations, read [Information and retention](references/information-and-retention.md).
For a focused review that does not affect them, state that retention is unchanged
or outside the reviewed scope instead of inventing a persistence design.

**Exit:** Affected information has a justified retention decision or an explicit
open question. Reconstructibility and retention are evaluated separately.

## 6. Validate against business scenarios

Walk through a representative successful scenario and the consequential exception
paths supported by the requirements. For a focused review, choose scenarios that
exercise the changed boundary.

For each scenario, trace the input or action, applicable rule, participating
concepts, accepted or rejected transitions, and resulting business facts.
When retention is in scope, check what remains recoverable after restart.

Check every confirmed in-scope requirement against a rule, fact, or scenario.
Use the principles to check concept necessity, rule ownership, and assumptions.

If a scenario fails, revise the relevant earlier stages and run it again.
Treat proposed edge-case behavior as a question until the business confirms it.

**Exit:** The model supports the confirmed scenarios without contradictions;
unresolved behavior is visibly excluded from any completion claim.

## Deliver the model

Default to a compact answer containing the problem and scope, retained concepts,
important rules and interactions, relevant lifecycle and retention decisions,
scenario results, and remaining assumptions or questions. Omit inapplicable detail.

For an explicitly requested full modeling document, or a model too complex for
that compact form, read [Full model template](assets/model-template.md).
Use only its applicable sections; it defines presentation, not extra modeling rules.
Write a file only when requested or required by the project's established workflow.

Label the result **Confirmed within scope** only when the scenario checks pass
and no consequential business decision remains unknown. Otherwise label it
**Provisional** and identify the blocking questions and affected decisions.
