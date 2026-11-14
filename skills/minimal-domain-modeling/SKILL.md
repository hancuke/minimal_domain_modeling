---
name: minimal-domain-modeling
description: Define business meaning through the fewest sufficient concepts, their responsibilities, collaboration, lifecycles, and invariants. Use when clarifying business requirements or reviewing business boundaries, including when existing code supplies evidence. Produce a concise system-level business model; software structure and implementation belong to software-design.
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
- Choose this skill when business meaning needs a decision. A technical change
  under established business rules belongs to `software-design`; business vocabulary
  in a request does not by itself make it a modeling task.
- Use the user's business vocabulary. Resolve overloaded terms before depending on them.
- Label confirmed requirements, provisional assumptions, and open questions.
- Read supplied requirements and relevant existing models or code when available.
  Surface contradictions between them; ask which behavior is intended.
- For a new model, follow the stages below. For a focused review, inspect the
  affected stages and their dependencies rather than rebuilding the entire model.
- Separate the modeling process from its document. Task framing and scenario
  checks guide the work; the document describes the system's business structure.
- Revisit earlier stages when scenarios, capabilities, or lifecycles expose a
  missing distinction. The stage order is a starting path, not a one-way gate.

## 1. Frame the problem

State the current situation, desired outcome, core business problem, and scope.
Separate a proposed implementation from the need it serves, while preserving
external constraints that actually affect the business.
Keep this task framing out of the system model document.

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

An invariant is a business constraint that every valid state or transition must
preserve. Distinguish it from a workflow step or implementation technique.

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
in the review response when relevant, rather than adding a task-local note to
the system model or inventing a persistence design.

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

Read [Model template](assets/model-template.md) when producing a model. Use its
three sections for both brief and larger models:

1. **Concepts and responsibilities:** The fewest sufficient business concepts,
   each with a short meaning and responsibility. Include identity, essential facts,
   ownership, and cardinality only where they explain rules or lifecycles.
2. **Collaboration and lifecycles:** Essential interactions and meaningful state
   transitions, including their triggers, conditions, and outcomes. Keep independent
   lifecycles distinct.
3. **Business invariants:** Constraints that must hold, each with its responsible
   concept or business collaboration.

Define each fact or rule once and reference it elsewhere. Fold necessary retention
obligations into the relevant responsibility, lifecycle, or invariant. Prefer short
sentences and compact tables; use diagrams only to resolve specific ambiguity.
Let complexity determine length, not a checklist of optional fields.

Describe the integrated system model, not an incremental change proposal. Update
an existing model in place, preserving unaffected business meaning. Keep task
scope, exclusions, change lists, scenario logs, validation status headings, and
implementation plans out of the document. Scenario checks remain mandatory work,
not extra document sections.

Use the available system context; if it is insufficient for a global model,
report the missing context separately rather than inventing the rest. Mark
provisional statements next to the affected concept or rule and raise blocking
questions in the response. Claim validation only for behavior whose scenario
checks passed and whose consequential business decisions are confirmed.

For a focused review without a model-document request, give compact findings
rather than reproducing the system model. Write a file only when requested or
required by the project's established workflow.

The model is a business-semantic contract for `software-design`, not a class or
module blueprint. Hand off when software design is requested; that skill may map
business concepts to different code boundaries while preserving their meaning.
