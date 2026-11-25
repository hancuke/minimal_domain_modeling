# Minimal Domain Modeling

**Logical design first. If you haven't decided what to build, don't start
implementing it.**

## Core Philosophy

**The fundamental problem in software design is managing complexity.**

Complexity makes systems hard to understand and modify. The goal is to minimize complexity through:
- Strategic (not tactical) programming
- Deep modules with simple interfaces
- Information hiding
- Eliminating special cases and exceptions
- Good names and obvious code
- Clear abstractions


Use AI to clarify intent, expose tradeoffs, and produce information-dense outputs
that help you make decisions, not simply generate more code.

Establish the business concepts, responsibilities, rules, and intended behavior
before choosing technical structures. If these are already clear, proceed without
unnecessary modeling.

This repository provides portable Agent Skills for that work, from clarifying
business meaning to designing software and implementing agreed changes.

## Choose a Skill

| Your Need | Skill |
| --- | --- |
| Clarify business meaning and rules | [minimal-domain-modeling](skills/minimal-domain-modeling/SKILL.md) |
| Assess whether existing code structure needs improvement | [code-structure-review](skills/code-structure-review/SKILL.md) |
| Decide software boundaries and contracts | [software-design](skills/software-design/SKILL.md) |
| Implement an agreed change | [implement](skills/implement/SKILL.md) |
| Design, review, or implement an interface | [ui-design](skills/ui-design/SKILL.md) |
| Make outputs clearer and more useful for decisions | [communication-policy](skills/communication-policy/SKILL.md) |

Choose by need, not as a fixed pipeline. Local changes with clear behavior and
established contracts can go directly to `implement`.

When the goal or consequential design decisions are unresolved, clarify them
before implementation. Software designs require explicit approval before code
changes; design and review requests alone do not authorize implementation.

## Get Started

With Node.js, npm, and Git available, run this from your target project's root:

```sh
npx skills add hancuke/minimal_domain_modeling --skill minimal-domain-modeling
```

Select your target agent during installation. Load the installed skill using
that agent's invocation mechanism, then provide your requirement. For example:

> Help me clarify this requirement before implementation. Identify the essential
> concepts, responsibilities, business rules, and unresolved decisions. Do not
> write code yet.
>
> Requirement: [describe the problem and desired outcome]

Installation does not guarantee automatic activation. See the
[installation guide](docs/installation.md) for activation, other skills and their
dependencies, global installation, and upgrades.

## Further Reading

- [Installation and activation](docs/installation.md)
- [Repository maintenance and authoring sources](docs/maintaining.md)
- [Principle-driven skill design](guide.md)
