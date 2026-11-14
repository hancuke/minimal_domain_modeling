# Compare Meaningful Alternatives

Compare designs against the shared vocabulary in
[design-principles.md](design-principles.md).

## Hold constraints constant

Use the same required behavior, caller scenarios, compatibility requirements,
and dependency constraints for every design. Mark unresolved requirements as
assumptions or questions.

- **Good:** Compare caller-owned and module-owned coordination for the same workflow.
- **Avoid:** Favor one option by assuming different requirements for it.

## Vary a consequential decision

Usually two credible designs are enough. Include the current design when viable.
For each design, sketch a caller interaction.
Identify caller obligations, hidden knowledge, state and effect ownership,
and significant failure behavior.

- **Good:** Contrast caller-owned state with module-owned state and explain how
  recovery changes.
- **Avoid:** Present different names or method counts as distinct designs.

## Make the trade-off explicit

Compare caller knowledge, locality, dependency and failure isolation, compatibility,
and migration cost. Recommend an option and state its important disadvantage.
If no option is clearly better, identify the unresolved decision.

- **Good:** Prefer fewer caller obligations while acknowledging added module
  responsibility and migration cost.
- **Avoid:** Justify flexibility by hypothetical reuse.
  Do not combine options into a design with unclear ownership.
