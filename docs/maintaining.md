# Maintaining

[Back to README](../README.md)

This document covers repository organization, shared references, and authoring
sources. For skill behavior, responsibility boundaries, approval gates, and
handoffs, read the corresponding [SKILL.md](../README.md#choose-a-skill) rather
than maintaining a second specification here. For installation and upgrades,
see the [installation guide](installation.md).

## Repository Layout

```text
minimal_domain_modeling/
|-- README.md
|-- guide.md
|-- docs/
|   |-- installation.md
|   `-- maintaining.md
`-- skills/
    |-- communication-policy/
    |   `-- SKILL.md
    |-- minimal-domain-modeling/
    |   |-- SKILL.md
    |   |-- references/
    |   |   `-- information-and-retention.md
    |   `-- assets/
    |       `-- model-template.md
    |-- code-structure-review/
    |   |-- SKILL.md
    |   `-- references/
    |       |-- design-principles.md
    |       |-- dependencies-and-verification.md
    |       `-- alternatives.md
    |-- software-design/
    |   |-- SKILL.md
    |   `-- references/
    |       `-- state-and-transitions.md
    |-- implement/
    |   `-- SKILL.md
    `-- ui-design/
        `-- SKILL.md
```

Each subdirectory under `skills/` defines a skill. Skills may collaborate or share
reference material. Required dependencies must identify both the dependency and
when it must be read; installation must make the required skills and reference
files available.

The README, `guide.md`, and `docs/` are human-facing documentation, not runtime
skill content. Keep them out of the agent's skill context. Skill content and
maintainer documentation are written in English for use across tools and
audiences. This package uses only the standard `name` and `description` metadata
fields, with no platform-specific execution extensions.

## Content Ownership

The main [minimal-domain-modeling skill](../skills/minimal-domain-modeling/SKILL.md)
covers analysis, clarification, and scenario validation. Specialized guidance
for assessing retention requirements lives in its
[reference](../skills/minimal-domain-modeling/references/information-and-retention.md).
The [model template](../skills/minimal-domain-modeling/assets/model-template.md)
has four sections: problem and goals, concepts and responsibilities, lifecycles
and collaboration, and business invariants.

The model document describes the complete model within the selected business
scope, excluding the current task's scope, change list, and validation process.
Update existing models in place and state each fact only once. Integrate
information-retention obligations into the relevant responsibilities, lifecycles,
or invariants. Mark unknown facts where they arise rather than inventing global
rules. Resolve skill file references relative to the skill directory, without
depending on this repository's absolute paths, scripts, or network access.

`software-design` owns the choice of engineering structures under confirmed
business rules. Its main skill contains the problem-first selection guidance;
its local `references/state-and-transitions.md` is read when operation legality
depends on lifecycle state or transition rules are scattered. This reference
supports design decisions without requiring the State pattern or a framework.
Add further method references only when real tasks establish a need, and keep
their reading conditions in the main skill rather than loading a pattern catalog.

General writing rules are maintained only in
[communication-policy/SKILL.md](../skills/communication-policy/SKILL.md);
`Agent Writing Standard.md` is no longer maintained separately. Task-specific
skills retain their specialized principles, examples, workflows, and delivery
requirements without a mandatory reference to Communication Policy. The policy
is not a required dependency for installing other skills independently.

## Shared References

Shared references are distributed with `code-structure-review`, not as a
separate skill. Reading a reference does not start an independent review workflow.

| Reference | Purpose and Reading Conditions |
| --- | --- |
| [design-principles.md](../skills/code-structure-review/references/design-principles.md) | Shared terminology and architectural constraints. `code-structure-review` reads it before evaluating candidates. `software-design` reads it for module boundaries, dependency isolation, or consequential architectural choices, not simple local changes. |
| [dependencies-and-verification.md](../skills/code-structure-review/references/dependencies-and-verification.md) | Dependency semantics, seam placement, and test migration. Read for dependency substitution, seam changes, or test migration. |
| [alternatives.md](../skills/code-structure-review/references/alternatives.md) | Compare improvement directions during review and target solutions during design. Read when alternatives are explicitly requested or differ consequentially. |

Depth is one evaluation dimension, not an overriding priority over responsibility
cohesion, independent change, or migration cost.

Keep the reference paths in `software-design/SKILL.md` valid when distributing
skills together. When an approved design needs implementation, hand off the
existing plan to `implement` without generating duplicate documentation.
`implement` does not itself depend on other skills' reference files; redesign
returns to `software-design`. Report missing required skills or references
explicitly and never claim an unperformed workflow is complete.

## Authoring Sources

See [Principle-driven Skill Design](../guide.md) for the authoring approach.

- [Anthropic Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
- [mgechev/skills-best-practices](https://github.com/mgechev/skills-best-practices)

## Further References

- https://github.com/wonsukchoi/domain-experts/blob/main/roles/financial-analyst/
- https://www.multicians.org/thvv/proverbs.html
- https://www.thoughtworks.com/en-us/insights/blog/agile-project-management/domain-modeling-what-you-need-to-know-before-coding
- https://www.cosmicpython.com/book/part1
- https://www.cs.unc.edu/~stotts/COMP590-059-f24/robsrules.html
- https://mcpmarket.com/zh/tools/skills/rob-pike-code-philosophy
- https://ubos.tech/news/rob-pikes-five-programming-rules-timeless-guidance-for-modern-developers/
