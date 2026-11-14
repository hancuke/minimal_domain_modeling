---
name: minimal-domain-modeling
description: Model a business problem through the fewest sufficient concepts, responsibilities, lifecycles, collaborations, and invariants. Use when clarifying business requirements or reviewing business boundaries, including when existing code supplies evidence. Support whole-system and local models; software structure and implementation belong to software-design.
---

# Minimal Domain Modeling

## Goal and boundaries

Build an understandable business problem model. Preserve required meaning with
the fewest justified concepts. Favor clear rules and boundaries over concept count.

Support whole-system and local models. Do not expand a local problem into the
entire system.

Use this skill when business meaning needs a decision. Business vocabulary alone
does not make a technical change a modeling task. Leave architecture and
implementation under established business rules to `software-design`.
When software design is requested, hand off the model as its business contract.
Confirmed business meaning remains authoritative.

## Principles

- **Minimal sufficiency:** Merge or remove a concept only if the model can still
  express all required facts, rules, and boundaries.
- **Independent change:** Retain distinctions justified by independent identity,
  rules, or lifecycles, not hypothetical reuse.
- **Rule ownership:** Assign a decision to the concept that enforces its constraint,
  not one that only describes related facts. Define each fact or rule once.
  Reference that definition elsewhere.
- **Bounded uncertainty:** Unconfirmed business rules support provisional choices,
  not a confirmed model. Distinguish confirmed requirements, provisional
  assumptions, and open questions.
- **Evidence and clarification:** Use the user's business vocabulary. Resolve
  ambiguous terms before using them. Read supplied requirements and relevant
  models or code. State contradictions and ask which behavior is intended.
- **Local before collaborative:** Prefer explaining each important concept's
  responsibilities and lifecycle before its collaboration with other concepts.
  This is a default order, not a prerequisite for discussing collaboration.

**Good:** Keep an approval task when claiming, transfer, and expiry give it its own
facts and lifecycle. **Avoid:** Remove it because pending tasks can be calculated.

**Good:** Model cancellable quantities for partially fulfilled orders.
**Avoid:** Treat the entire order as cancelled when only some quantities are cancelled.

## Modeling process

For a new model, follow all six stages. For a focused review, inspect affected
stages and their dependencies. Do not rebuild the entire model.
Revisit earlier stages as new business distinctions emerge.

### 1. Frame the problem

Identify who faces the problem. State the current situation, desired outcome,
and success criteria. Define the business scope, external dependencies, and
constraints. Separate a proposed solution from the need it serves.

Identify unknowns before choosing concepts. Ask for clarification when an unknown
could change identity, a key rule, lifecycle, ownership, or retention obligations.
Ask only the questions needed to resolve the current blocker.

If clarification is unavailable, present alternatives or a provisional model
with explicit assumptions. Identify which decisions depend on those assumptions.

**Exit:** The problem, scope, and outcome are clear. Key unknowns are resolved
or explicitly limit the provisional model.

### 2. Discover and reduce concepts

For each candidate, identify its meaning, essential facts, relationships, and need
for individual identification. State relationship limits and business ownership
where they affect rules or lifecycles. Do not treat every association as ownership.

Business events, rules, policies, and processes can also be concepts when they
have a necessary business meaning or responsibility.

Apply minimal sufficiency and independent change. State why each concept is needed.
Explain merges and exclusions that affect business meaning.

**Exit:** Every retained concept is justified. Simplification loses no confirmed
in-scope requirement.

### 3. Assign responsibilities and rules

Assign responsibilities, business operations, and invariants to important concepts.
For consequential operations, state acceptance conditions, rejection cases,
and resulting facts or state changes.

An invariant is a business constraint that all valid states or transitions must
preserve. It is not a workflow step.

For a decision across concepts, name the business rule, policy, or process.
Identify its participants and who is responsible for the decision.

**Exit:** Important rules have clear owners or collaboration boundaries.
Operations have clear business effects.

### 4. Explain lifecycles and collaboration

For concepts with meaningful state changes, describe creation, relevant states,
transition triggers and conditions, rejected transitions, and completion or
termination. Keep independent lifecycles separate. Skip lifecycle descriptions
for concepts without meaningful state changes.

Use the relationships from stage 2 to explain essential collaboration.
Identify the initiating action, participants, decisions, and resulting facts
or state changes. Add newly discovered relationships. Revise lifecycles when
collaboration reveals missing conditions.

**Exit:** Essential operations are clear from start to finish.
Lifecycle restrictions agree with responsibilities and invariants.

### 5. Identify business facts and retention obligations

When the task introduces or changes facts, history, or commitments that must
remain known, read
[Business facts and retention obligations](references/information-and-retention.md).
For a focused review with unchanged retention obligations, mention this in the
response only when relevant. Do not add a task-local note to the model.

**Exit:** Retention obligations for required facts, decisions, commitments,
and history are justified or marked as open questions.

### 6. Validate against business scenarios

Walk through a representative success scenario and key exception paths from
the requirements. For a focused review, exercise the changed boundary.

Trace each scenario's input or action, rules, participants, accepted or rejected
transitions, and resulting facts. When retention is in scope, check that history
remains explainable and commitments remain known after interruption or completion.

Check every confirmed in-scope requirement against a rule, fact, or scenario.
Check concept necessity, rule ownership, and assumptions against the principles.

If a scenario fails, revise the model and run the scenario again.
Treat proposed edge-case behavior as unconfirmed until the business confirms it.
Scenario checks are required work, not extra model sections.

**Exit:** Confirmed scenarios have no contradictions. Claim validation only for
behavior with passed scenario checks and confirmed key business decisions.
Explicitly exclude unresolved behavior.

## Delivery requirements

- Read and use the four-section [Model template](assets/model-template.md).
  Let business complexity determine length, not optional fields.
- Describe an integrated model of the selected business scope, not a change proposal.
  Update existing models in place. Preserve unaffected business meaning.
- Include business boundaries. Exclude task-specific scope and exclusions, change
  lists, scenario logs, validation status headings, and implementation plans.
- Mark assumptions next to the affected concept or rule. Report missing context
  and blocking questions separately in the response. Do not invent missing context.
- For a focused review without a model-document request, give compact findings.
  Write files only when requested or required by the project's workflow.
