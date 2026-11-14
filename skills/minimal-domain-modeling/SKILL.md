---
name: minimal-domain-modeling
description: Model a business problem through the fewest sufficient concepts, facts, rules, responsibilities, lifecycles, and collaborations. Use when clarifying business requirements or reviewing business boundaries, including when existing code supplies evidence. Support whole-system and local models; software structure and implementation belong to software-design.
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
  express all required facts, rules, and boundaries. Retain distinctions justified
  by independent identity, rules, or lifecycles, not hypothetical reuse.
- **Semantic clarity:** Model rules as well as facts and concepts. Distinguish
  business commitments from the mechanisms used to fulfill them.
- **Clear responsibility:** Identify who decides, maintains constraints, and
  fulfills commitments; these responsibilities may differ. Define each fact or
  rule once and reference it elsewhere.
- **Evidence and uncertainty:** Use the user's vocabulary, supplied requirements,
  and relevant models or code. Resolve ambiguous terms and contradictions.
  Distinguish confirmed requirements, provisional assumptions, and open questions.
- **Scenario validation:** Test the model with concrete scenarios and counterexamples.
  Classification alone does not establish correct business boundaries.

**Good:** Keep an approval task when claiming, transfer, and expiry give it its own
facts and lifecycle. **Avoid:** Remove it because pending tasks can be calculated.

**Good:** Model cancellable quantities for partially fulfilled orders.
**Avoid:** Treat the entire order as cancelled when only some quantities are cancelled.

## Modeling process

For a new model, follow all four stages. For a focused review, inspect only affected
stages and their dependencies. Revisit earlier stages as rules reveal missing
concepts or collaboration reveals missing boundaries.

### 1. Frame the problem

Identify who faces the problem. State the current situation, desired outcome,
and success criteria. Define the business scope, external dependencies, and
constraints. Separate a proposed solution from the need it serves.

Ask for clarification when an unknown could change identity, a key rule, lifecycle,
ownership, or retention obligations. Ask only what resolves the current blocker.
If clarification is unavailable, present alternatives or explicit assumptions
and identify which decisions depend on them.

**Exit:** The problem, scope, and outcome are clear. Key unknowns are resolved
or explicitly limit the provisional model.

### 2. Discover concepts and clarify rules

For each candidate, identify its meaning, essential facts, relationships, and need
for individual identification. State relationship limits and business ownership
where they affect rules or lifecycles. Do not treat every association as ownership.

Use four perspectives to clarify business statements:

| Perspective | Business question |
|---|---|
| Invariant | Which observable business states are invalid? |
| Precondition | Who may perform this operation, under what conditions, and when must it be rejected? |
| Decision Rule | Which facts and rules determine the conclusion or path? |
| Postcondition | When an operation succeeds, what results must already hold? |

Ask in business language before applying labels. These perspectives overlap and
are not exhaustive; use only those relevant to the requirement. An invariant must
hold at its defined business boundary, not after every internal step. A
postcondition describes a guaranteed result, not an implementation step.

Rules may reveal missing concepts. Business events, policies, and processes can
also be concepts when they have a necessary meaning or responsibility.
State why each retained concept is needed and explain simplifications that
affect business meaning.

**Exit:** Necessary concepts are justified and rule meanings are clear.
Simplification loses no confirmed in-scope requirement.

### 3. Organize responsibilities, lifecycles, and collaboration

Assign business operations and responsibility for decisions, constraints, and
results. For cross-concept rules, name the policy or process and its participants.
Do not duplicate a rule because several perspectives or participants refer to it.

Where relevant, clarify:

- **Scope and authority:** Where the rule applies, its exceptions, and who may decide.
- **Time and change:** Which facts apply at which time, and whether changes require
  a new decision.
- **Completion and consistency:** What must hold together, what may complete later
  and by when, and which commitments survive rejection or interruption.

These are business boundaries, not prescribed aggregates, transactions, or services.

For concepts with meaningful state changes, describe creation, states, transition
triggers and conditions, rejected transitions, and completion or termination.
Keep independent lifecycles separate. Explain essential collaboration through
its initiating action, participants, decisions, and resulting facts or state changes.
Distinguish an operation's success from completion of the wider business process.

When facts, decisions, history, or commitments must remain known, read
[Business facts and retention obligations](references/information-and-retention.md).
Identify what must remain available and why; leave unconfirmed obligations open.

**Exit:** Rules have clear responsibility and boundaries. Essential operations
have explicit conditions and results, including relevant retention obligations.

### 4. Validate scenarios and minimal sufficiency

Walk through a representative success scenario and key exception paths from
the requirements. For a focused review, exercise the changed boundary.

Trace each scenario's input or action, rules, participants, accepted or rejected
transitions, and resulting facts. When retention is in scope, check that history
remains explainable and commitments remain known after interruption or completion.

Check that successful operations preserve invariants, decision results govern
the relevant conditions or paths, and every promised result has a responsible
participant. Exercise relevant changes in facts or rules. For example, if an amount
crosses an approval threshold after approval, does that approval remain valid?
Ask rather than inventing the answer.

Trace every confirmed in-scope requirement to a fact, rule, or behavior and verify
it in a relevant scenario. Check concept necessity, responsibility, and assumptions
against the principles.

If a scenario fails, revise the model and run the scenario again.
Treat proposed edge-case behavior as unconfirmed until the business confirms it.
Scenario checks are required work, not extra model sections.

**Exit:** Confirmed scenarios have no contradictions. Claim validation only for
behavior with passed scenario checks and confirmed key business decisions.
Explicitly exclude unresolved behavior.

## Delivery requirements

- Read and use the four-section [Model template](assets/model-template.md).
  Let business complexity determine length, not optional fields.
- Put operation conditions, decisions, and success guarantees with the relevant
  responsibilities or collaboration; put invariants in `Invariants`. Do not
  force every rule into an invariant or add mandatory sections for each perspective.
- Describe an integrated model of the selected business scope, not a change proposal.
  Update existing models in place. Preserve unaffected business meaning.
- Include business boundaries. Exclude task-specific scope and exclusions, change
  lists, scenario logs, validation status headings, and implementation plans.
- Mark assumptions next to the affected concept or rule. Report missing context
  and blocking questions separately in the response. Do not invent missing context.
- For a focused review without a model-document request, give compact findings.
  Write files only when requested or required by the project's workflow.
