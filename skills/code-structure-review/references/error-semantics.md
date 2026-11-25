# Error Semantics and Caller Obligations

Use this reference when defaults, idempotency, fallback, retries, or error
aggregation affect a contract. It supports review and design without authorizing
behavior changes or starting a separate workflow.

## Simplify the contract, not the evidence of failure

Start with the promised outcome and confirmed business rules. Distinguish an
accepted variation, a business rejection, an invalid call, and a technical
failure. Choose representations that fit the project's conventions; an expected
rejection need not be an exception, but must remain distinguishable from success.

Remove an error case only when the contract gives it a valid meaning. Moving
validation into a helper does not eliminate the failure or its handling obligation.
Changing an existing outcome is a contract change, even if the new API is simpler.

**Good:** Under an "ensure absent" contract, an already absent temporary file
satisfies the requested outcome. Permission and I/O failures still need reporting.
**Avoid:** Clamp a negative order total to zero to reuse the successful checkout
path. That replaces a business decision rather than simplifying its implementation.

## Give recovery a responsible owner

Handle failures where enough context exists to fulfill the contract or choose a
valid recovery. Keep provider details internal, but expose outcomes callers need
to decide what happens next.

- Use a default only when absence has that confirmed meaning. Do not treat
  malformed data, denied access, or an unavailable dependency as ordinary absence.
- Allow fallback or degraded operation only when the promised result permits it.
  Make staleness or incompleteness visible when it affects caller decisions.
- Retry only when repeat execution is safe and the failure is eligible.
  Bound retries; report exhausted attempts and uncertain outcomes.
- Aggregate errors only when callers can still distinguish outcomes requiring
  different actions. Preserve diagnostic causes without leaking provider-specific
  recovery into every caller.

**Good:** A preferences contract defines defaults for an unset preference.
A failed read remains a failure unless an approved fallback covers it.
**Avoid:** Catch every storage error and return an empty collection that callers
interpret as "no records."

For dependency isolation, retry safety, and partial effects, use
[dependencies-and-verification.md](dependencies-and-verification.md).

## Challenge the promised outcome

Trace success, rejection, and relevant failure paths through representative
callers. For each proposed simplification, establish:

- Which condition no longer needs special handling, and why the outcome is valid.
- Who owns recovery and what callers must still know.
- What happens after partial completion, an uncertain response, or failed recovery.
- Which compatibility obligations and focused checks protect those distinctions.

An idempotent operation promises equivalent intended effects on repetition;
it does not promise that execution cannot fail. A timeout does not establish
whether an external effect occurred. Do not report either as unconditional success.
