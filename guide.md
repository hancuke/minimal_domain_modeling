
# Principle-driven Skill Design

Distill complex knowledge, experience, or discussion into the smallest set of insights, principles, and examples sufficient to guide behavior. The goal is not the shortest document, but the highest information density and decision value.

# Skill Types
Principles are effective at guiding judgment, but not necessarily execution.

Consider three types of skills:

| Skill Type | Primary Description Format |
| --- | --- |
| Thinking method | Insight + Principle + Examples |
| Design standard | Principle + Good / Avoid |
| Workflow | Steps + Decision Rules + Verification |


## A Leaner Model for Describing Skills

Structure the description in three layers:

Layer 1 · Understanding

Insight

Explain the core insight behind the skill: why is it needed, and what fundamental problem does it address?

Layer 2 · Judgment

Principles

Define the agent's criteria for judgment: what is good, what is not, and what takes priority when criteria conflict?

Layer 3 · Execution

Examples / Workflow

Use positive and negative examples to show how principles apply in practice. For procedural tasks, define the necessary steps, decision branches, and verification conditions.

The three layers have an important logical relationship:

- Insights provide understanding.
- Principles guide judgment.
- Examples and workflows constrain behavior.

Not every skill needs all three layers. Some may need only one insight and three principles.


| Situation | Best Format |
| --- | --- |
| The agent is likely to misunderstand the problem | Insight |
| The agent is likely to make poor choices | Principle |
| The agent does not understand how to apply a principle | Examples |
| The agent is likely to miss steps | Workflow |
| The agent is likely to cross boundaries | Constraints |
| The agent must ensure the result is correct | Verification |

Five design guidelines:

1. Insight over Introduction: State the core insight directly instead of spending space on background.
2. Principles over Generic Instructions: Replace vague behavioral demands with principles that guide decisions.
3. Examples over Repetition: Demonstrate principles with positive and negative examples instead of repeatedly explaining them.
4. Workflow when Necessary: Define detailed workflows only for tasks that need repeatable execution.
5. Evidence over Completeness: Test skills on real tasks rather than judging quality by document length.


Every principle must guide a real decision. Brevity does not guarantee clarity, and abstraction does not imply sophistication.

If a principle requires the agent to guess its concrete meaning, it risks becoming another vague instruction. Good examples should remove that ambiguity, not merely decorate the document.

A weak principle:
> Write clean and maintainable code.

A better principle:
> Prefer explicit domain types over generic dictionaries when the data has stable business meaning.

The former offers little decision value; the latter can directly influence the agent's code design.


<template_schema>

---
name: minimal-domain-model
description: ...
---

# Minimal Domain Model

## Insight

State the core insight in one sentence.

## Principles

- Principle 1
- Principle 2
- Principle 3

## Good Examples
Show concrete practices that follow the principles.

## Avoid
Show common mistakes and explain why they are mistakes.

## Execution
Define this section only for workflows that require repeatable execution.

</template_schema>
