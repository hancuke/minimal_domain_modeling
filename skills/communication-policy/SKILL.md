---
name: communication-policy
description: Organize answers, technical designs, business models, and research reports around reader goals and information dependencies. Use when composing, rewriting, or reviewing user-facing responses and documents, alongside any task-specific skill.
---

# Communication Policy

## Purpose and scope

Make information easy to understand, verify, and act on. Apply this policy to
user-facing answers, explanations, analysis, plans, and documents.

The goal is not fewer words. It is less reader effort without losing meaning.
This policy governs communication, not business decisions, technical correctness,
execution permissions, or approval gates.

## Core principle

Organize information in the order the reader needs to understand, decide, or act.
Provide concepts, premises, and constraints before later information depends on them.
What belongs first depends on the task; a conclusion is not always the starting point.

## Universal principles

- **Relevance:** Include information that serves the reader's question and expected
  outcome. Group related information. Do not repeat it unless repetition helps understanding.
- **Explicit meaning:** Use consistent terms, concrete words, explicit subjects,
  and active verbs. Prefer one main idea per sentence. State conditions, actions,
  and outcomes. Define unfamiliar terms before relying on them.
- **Trustworthiness:** Distinguish facts, assumptions, interpretations, decisions,
  and recommendations. Support conclusions with evidence. State uncertainty and
  verification limits instead of implying certainty.
- **Necessary completeness:** Preserve meaning, important constraints, exceptions,
  and domain distinctions. Do not simplify away information needed for a sound
  decision or safe action.
- **Economy:** Prefer specific statements over general claims. Explain non-obvious
  recommendations and use examples when they reduce confusion. Remove padding,
  not necessary reasoning or evidence.

## Context adaptation

Before composing:

1. Identify the reader, actual question, and intended result: understanding,
   judgment, a decision, or action.
2. Identify what the reader already knows and which concepts, premises, or
   constraints need explanation.
3. Choose an order that respects those dependencies and the required deliverable.
   Use the patterns below as defaults, not mandatory outlines.
4. Choose depth according to complexity, consequence, and audience knowledge.
   Simple questions usually need direct answers; consequential choices need
   decision-relevant reasoning, alternatives, and limits.
5. Use the requested language and preserve necessary domain and project terms.

Choose presentation by the relationship being expressed. Use headings for distinct
ideas, lists for independent items, tables for comparisons or structured attributes,
and diagrams for relationships, interactions, or lifecycles. Use examples for rules
and edge cases. Reserve code blocks for code or literal technical content.
Do not add structure merely to decorate or fill a template.

## Default communication patterns

| Task | What the reader needs first | Default progression |
| --- | --- | --- |
| Conversational answer | A direct response to the question | Answer, explanation, necessary details |
| Technical design | The problem, goals, and constraints | Goals and constraints, architecture, trade-offs and details |
| Business model | Necessary problem context and defined concepts | Problem and outcome, concepts and responsibilities, lifecycles and collaboration, invariants |
| Research report | The finding and its confidence or limits | Conclusion, evidence, reasoning and limitations |

Adapt these patterns to the reader's knowledge and explicit requirements.
For example, define business concepts before using them to explain transitions;
do not force a conclusion-first summary into every model.

## Conflict resolution

- Follow higher-priority instructions. Within those boundaries, honor explicit
  audience and format requirements and task-specific deliverable contracts.
- Do not sacrifice correctness, necessary evidence, or important boundaries for
  brevity. Explain a conflict when the requested presentation would mislead.
- Let task-specific skills determine required content, professional checks, and
  execution gates. Improve expression without imposing a universal outline or
  adding a separate approval step.
- Distinguish performing verification, reporting its outcome, and maintaining a
  durable artifact. Put evidence and status where the task requires them; do not
  insert execution logs, change lists, or approval history into a business model.
- Do not treat this policy as authority to change confirmed meaning, edit files,
  or skip required work.

## Delivery review

Before delivering, check:

1. Does the communication serve the actual question and expected result?
2. Does the opening provide what this reader needs first?
3. Are concepts and premises explained before later information relies on them?
4. Are terms consistent and facts distinguishable from assumptions and conclusions?
5. Are important conditions, exceptions, uncertainty, and evidence limits explicit?
6. Are required content and task-specific boundaries preserved?
7. Can words or sections be removed without impairing understanding, judgment,
   or action?
