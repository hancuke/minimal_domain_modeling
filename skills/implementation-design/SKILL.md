---
name: implementation-design
description: Design technical solutions from established requirements or a selected refactoring recommendation. Use when defining project architecture, planning a feature's implementation, or turning a chosen technical change into a delivery plan. Implement and verify only when code changes are requested.
---

# Implementation Design

Technical design gives confirmed behavior clear responsibility, state, and
failure semantics; adding layers is not the goal.

## Principles 

### 1. Make the main path obvious

Make the main job of the code obvious without forcing readers through layers of mechanism.

### 2. Keep rules regular and orthogonal

Avoid designs where one feature subtly changes the meaning of another.

### 3. Prefer small sharp tools over framework accretion

Favor components that do one job cleanly and combine predictably with others.

### 4. Use plain words and plain structure

Choose names and structures that say exactly what they do, with as little ceremony as possible.

## Behavioral Constraints

- Remove options that do not serve the primary job.
- Prefer a regular rule over a pile of special cases.
- Do not invent framework structure when a simple tool, function, or pipeline is enough.
- Keep naming literal and flow visible.
- Minimize the number of concepts a reader must hold at once.



## Default Execution Mode

1. State the main job in one plain sentence.
2. Delete or fold away features that distract from that job.
3. Regularize the structure so similar things work the same way.
4. Make data flow and naming obvious.
5. Validate that the result is easier to explain without jargon.



## Execution contract


- Start from confirmed requirements or a selected diagnostic recommendation.
  Business rules and visual design are inputs; surface contradictions rather
  than silently changing them.
- New features do not require a prior module-deepening diagnosis. Open-ended
  investigation of existing structural problems belongs to `module-deepening`.
- For design or review requests, deliver the design and stop. For code-change
  requests, continue through implementation and verification.
- Scale the work to the decision: a small change may need a few sentences,
  not a module diagram or a checklist for every function.
- Read [design-principles.md](../module-deepening/references/design-principles.md)
  when evaluating module boundaries, dependency isolation, or consequential
  architectural choices. A local change with no such decision uses the principles
  above without loading references.
  Read [deepening.md](../module-deepening/references/deepening.md) when the change
  requires dependency substitution, seam changes, or test migration.
  Read [alternatives.md](../module-deepening/references/alternatives.md) when
  designs have consequential differences in caller obligations, state ownership,
  or dependency direction, or alternatives are explicitly requested.
- Install these references in the sibling `module-deepening` directory.
  Reading them does not invoke diagnosis. If a required read fails, report the
  path and keep affected judgments provisional.

## 1. Confirm inputs and constraints

Identify desired behavior, acceptance criteria, scope, and compatibility needs.
Inspect relevant code, conventions, and tests.

When a diagnosis is supplied, reuse its evidence and selected candidate. Check
the assumptions that affect the solution rather than rerunning the whole scan.
Ask when an unresolved rule or architectural choice would materially change the
design. If clarification is unavailable, label the affected design provisional.

**Exit:** Required behavior and significant constraints are stated; consequential
unknowns are resolved or explicitly limit the design.

## 2. Form the technical solution

Define responsibilities and interactions at the appropriate scale: project,
feature, module, or method. Specify the interface, dependency direction, relevant
state transitions, data flows, side effects, and failure handling.

Apply the principles to the solution. Preserve existing contracts unless changes
are authorized. Use project terminology and existing places for durable decisions.

**Exit:** The solution explains how the required scenarios work and who owns the
important decisions, state, and effects.

## 3. Challenge the solution

Trace actual caller and change scenarios. Challenge added concepts, caller
obligations, ownership, and compatibility using the principles and applicable
references.

Use the optional references under their reading conditions. Identify tests,
types, runtime checks, and integration checks that protect important behavior.
For refactors, map existing coverage using the test-migration reference.

**Exit:** Significant trade-offs and verification needs are explicit; added
complexity has a current justification and important compatibility risks are
accounted for.

## 4. Deliver the design

Present the approach and why it fits, the relevant responsibilities and
interactions, verification strategy, and only significant risks or alternatives.
Use a diagram or code sketch when it clarifies the decision.

For refactors, include caller migration, compatibility checks, and a recovery
or rollback approach proportionate to deployment and data risk. State whether
the design is confirmed or provisional.

**Exit:** The user can understand the proposed solution and how to validate and
safely introduce it. Design-only and review requests finish here.

## 5. Implement and verify when requested

Proceed only for a code-change request and after consequential uncertainties
are resolved. Make the smallest coherent change, follow project conventions,
and keep dependent callers consistent.

Run checks covering the changed behavior and relevant integration risks using
existing tooling. If implementation invalidates an assumption, revisit the
affected design rather than hiding the mismatch behind a workaround.

**Exit:** The requested change is implemented and appropriate checks have run,
or blockers and unverified behavior are stated explicitly. A completed design
is not evidence of a verified implementation.
