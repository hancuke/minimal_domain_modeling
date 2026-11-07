---
name: implementation-design
description: Design technical modules, interfaces, dependencies, data flows, and failure handling from established requirements. Use when defining project architecture, planning a feature's implementation, or refactoring technical boundaries. Treat business rules and visual design as inputs; implement and verify when the task requests code changes.
---

# Design Before Code

## Goal

Before writing code, design the smallest solution that makes the system easy to understand, maintain, and extend.

Prioritize business intuition, clear boundaries, simple composition, and explicit behavior. Avoid unnecessary abstractions and documentation.

## Workflow

### 1. Understand the Problem

- Identify the problem, desired behavior, and acceptance criteria.
- Inspect relevant existing code, architecture, conventions, and tests.
- Identify business rules, constraints, and compatibility requirements.
- Do not propose architecture before understanding the actual problem.

### 2. Design the Solution

Determine the appropriate design scope based on the task.

- **Project architecture:** Define major modules, their responsibilities, dependency directions, and entry points.
- **Feature or module:** Define responsibilities, boundaries, interfaces, and interactions.
- **Class or method:** Define intent, inputs, outputs, side effects, and behavioral constraints.

Apply these design principles:

1. **Semantic clarity:** Names and interfaces express business intent, not merely implementation details.
2. **Clear responsibilities:** Each component has a coherent purpose and an explicit boundary.
3. **Explicit dependencies:** Important dependencies, data flows, and side effects are easy to trace.
4. **Predictable behavior:** Inputs, outputs, state transitions, and failure behavior are understandable.
5. **Verifiable constraints:** Important business rules and edge cases can be checked through tests, types, or runtime validation.
6. **Discoverable knowledge:** Architecture decisions, business rules, and usage guidance are documented where developers expect to find them.

### 3. Evaluate the Design

Before implementation, challenge the proposed solution:

- Can another developer understand the design without reading every implementation detail?
- Does each abstraction solve a real problem?
- Are responsibilities and dependency directions clear?
- Can likely changes be localized to the relevant components?
- Are there simpler alternatives with fewer concepts and smaller interfaces?
- Does the design preserve existing behavior and important business constraints?

Consider alternatives only when they represent meaningfully different trade-offs. Prefer the simplest design that satisfies the requirements.

Do not introduce speculative abstractions for hypothetical future requirements.

### 4. Present the Plan Before Coding

Present a concise design proposal containing:

- **Approach:** What will change and why.
- **Responsibilities and boundaries:** Which components are involved and what each owns.
- **Interactions:** How components collaborate; use a diagram when it clarifies important flows.
- **Verification:** How correctness and important edge cases will be tested.
- **Trade-offs:** Mention only significant alternatives, risks, or unresolved decisions.

Scale the proposal to the task. A small method change may need only a few sentences. A project-level change may require a module diagram and dependency rules.

If a critical ambiguity or architectural decision could materially change the solution, ask the user before proceeding. Otherwise, state reasonable assumptions and continue.

### 5. Implement and Verify

After the design is sufficiently clear:

- Implement according to the agreed design.
- Prefer the smallest coherent change.
- Follow existing project conventions unless there is a concrete reason to change them.
- Run relevant tests and other available checks.
- If implementation reveals a flawed assumption or boundary, revisit the design rather than patching around it blindly.

## Guardrails

- Do not start implementation before completing the design step.
- Do not turn the six principles into a mandatory checklist for every function or class.
- Do not create unnecessary layers, interfaces, classes, diagrams, or documents merely to demonstrate good design.
- Do not confuse descriptive names with clear responsibilities.
- Do not claim correctness without appropriate verification.
- Do not redesign unrelated parts of the system.

## Completion Criteria

The design is acceptable when:

1. The intended behavior and important constraints are clear.
2. Responsibilities and interfaces are understandable.
3. Dependencies and significant side effects are visible.
4. The solution is no more complex than necessary.
5. Important behavior can be verified.
6. The implementation can be changed without unnecessary ripple effects.

The objective is not perfect architecture. It is a design that developers can understand, verify, and safely change.
