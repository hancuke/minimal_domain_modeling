# State and Transitions

Use this reference when operation legality depends on lifecycle state or
transition rules are scattered. Translate confirmed lifecycle rules into a
software contract; do not invent business policy to complete a diagram.
Return unresolved business meaning to `minimal-domain-modeling`.

## Model behavior before choosing machinery

A state machine describes states, events, and legal transitions. The State
pattern is one possible implementation, not a prerequisite. An enum and a
transition function can express a complete state machine.

Use explicit transitions when they clarify allowed operations, protect
invariants, or replace duplicated state checks. Keep a simple function or
conditional when behavior is local and already clear. A status field used only
for display does not by itself justify a state machine.

Keep independent lifecycles separate. Combining order fulfillment and refund
progress into one status can create artificial states and hide partial completion.
Use confirmed rules to define any coordination between them.

## Make the transition contract explicit

Describe only the decisions relevant to the required scenarios:

- The state owner and the facts that determine the current state.
- The triggering operation or event, permitted source states, guard conditions,
  and resulting state.
- The result and failure semantics, including what happens for an unlisted
  transition or a repeated request.
- The effects required for success and those that may complete later, with an
  owner for pending work and failure recovery.

A compact transition table is often enough:

| Current state | Trigger | Guard | Next state | Result or obligation |
| --- | --- | --- | --- | --- |
| Submitted | Approve | Actor may approve | Approved | Record the approval |
| Submitted | Reject | Actor may reject | Rejected | Record the rejection |
| Approved or Rejected | Approve or Reject | No further decisions allowed | Unchanged | Reject the operation |

This example assumes final decisions cannot be revised. Establish that rule for
the actual domain rather than copying it. Define repeated-request behavior
separately where delivery or caller retries are possible.

Distinguish a transition being accepted from an external effect completing.
If cancellation is accepted before a refund completes, the contract must not
silently equate cancellation success with refund success.

## Choose the smallest sufficient representation

| Representation | Appropriate when | Cost or limitation |
| --- | --- | --- |
| Explicit operations with conditionals | A small lifecycle has local, readable rules | Checks can drift if duplicated across callers |
| Transition function or table | Allowed transitions need one clear owner | Guards, effects, and failures still need explicit semantics |
| State pattern | Substantial behavior varies by state and benefits from cohesive implementations | More objects and indirection; shared rules can become scattered |
| State-machine library | Required hierarchical states, coordination, or execution features justify it | Library semantics, persistence, and integration become design obligations |

Compare the proposed mechanism with a credible simpler option using the same
scenarios. Do not introduce a framework merely to draw transitions, or require
one class per state. A transition table need not become a generic runtime engine.

## Protect transitions at their boundary

Expose meaningful operations rather than unrestricted status mutation. Keep
transition decisions with their owner, not repeated in controllers and callers.
Separate deciding the transition from technical I/O where useful, while making
the success boundary and effect ownership explicit.

When concurrent operations are possible, explain how checking the source state
and committing the transition avoid accepting incompatible decisions. A prior
in-memory check alone does not protect a later write.

When retries, duplicate events, or external effects are possible, define how
duplicates are recognized, what result callers receive, and what remains true
after interruption. Choose atomicity, idempotency, or recovery mechanisms only
as required by those guarantees. A state machine alone supplies none of them.
Do not infer event sourcing, durable history, or a workflow engine from the
presence of states.

## Exercise the contract

Trace a normal lifecycle and the relevant counterexamples: an operation in a
forbidden state, a failed guard, and, where possible, repeated or concurrent
requests and an effect failing after a transition is accepted.

Check the resulting state, caller-visible outcome, preserved invariants, and
owner of any unfinished work. If the scenario exposes missing business policy,
keep the affected design provisional rather than making up a transition.

Integrate the useful table, ownership decisions, and trade-offs into the
existing software design. Do not require a separate state-machine document or
diagrams that duplicate an already clear contract.
