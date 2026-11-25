---
name: communication-policy
description: Organize answers, technical designs, business models, and research reports around reader goals and information dependencies. Use when composing, rewriting, or reviewing user-facing responses and documents, alongside any task-specific skill.
---

# Communication Policy

Make information easy to understand, verify, and act on—reducing reader effort
without losing meaning.

## Contract

- **Scope:** Governs clarity, ordering, and presentation of user-facing answers,
  explanations, analysis, plans, and documents, alongside any active task skill.
- **Non-goals (refuse):** Do not override higher-priority instructions or
  task-specific deliverable contracts. Do not treat this policy as authority to
  make business decisions, alter confirmed meaning, judge technical correctness,
  edit files, skip required work, or impose a universal outline.
- **Gates:** This policy adds no standalone approval gate; defer all execution
  and approval gates to the active task skill. Never sacrifice correctness,
  required evidence, or important boundaries for brevity—when a requested format
  would mislead or omit decision-critical constraints, explain the conflict
  before complying.
- **Artifact separation:** Distinguish performing verification, reporting its
  outcome, and maintaining a durable artifact. Place evidence and status where
  the task requires them; never insert execution logs, change lists, or approval
  history into a business model or similar durable artifact.

## Principles

- **Dependency order first:** Organize information in the order the reader needs
  to understand, decide, or act. Define concepts, premises, and constraints
  before later content relies on them, and match depth to complexity,
  consequence, and audience knowledge.
  - **Good:** Define `Reservation` and `HoldExpiry` before explaining the
    timeout transition rules that depend on them.
  - **Avoid:** Lead with a summary of five timeout exceptions before defining
    what `HoldExpiry` means in the model.
- **Clarity without loss:** Use consistent terms, concrete words, explicit
  subjects, and active verbs; prefer one main idea per sentence. Preserve
  requested language, domain terms, conditions, exceptions, and domain
  distinctions. Remove padding—never decision-relevant reasoning or evidence.
  - **Good:** "Retry failed webhook deliveries up to 3 times with exponential
    backoff, except on `4xx` responses (which fail immediately)."
  - **Avoid:** "Webhook retries are handled gracefully in most failure cases."
- **Epistemic explicitness:** Distinguish facts, assumptions, interpretations,
  decisions, and recommendations. Support conclusions with evidence, and state
  uncertainty or verification limits instead of implying certainty.
  - **Good:** "Load test logs show P99 at 820ms (observed); lock contention is
    the leading hypothesis, but CPU and lock profiles have not been captured."
  - **Avoid:** "Lock contention causes P99 latency to reach 820ms."
- **Structure matches relationship:** Choose presentation by the relationship
  being expressed—headings for distinct ideas, lists for independent items,
  tables for comparisons or structured attributes, diagrams for relationships,
  interactions, or lifecycles, examples for rules and edge cases, and code
  blocks only for code or literal technical content. Do not add structure merely
  to decorate or fill a template.
  - **Good:** Use a 3-column comparison table when contrasting three caching
    strategies across latency, consistency, and memory cost.
  - **Avoid:** Write three separate narrative paragraphs repeating the same
    attribute labels, or force a table for a single-sentence answer.

### Default communication patterns

Adapt these defaults to the reader's knowledge and explicit requirements rather
than treating them as mandatory outlines.

| Task | What the reader needs first | Default progression |
| --- | --- | --- |
| Conversational answer | A direct response to the question | Answer, explanation, necessary details |
| Technical design | The problem, goals, and constraints | Goals and constraints, architecture, trade-offs and details |
| Business model | Necessary problem context and defined concepts | Problem and outcome, concepts and responsibilities, lifecycles and collaboration, invariants |
| Research report | The finding and its confidence or limits | Conclusion, evidence, reasoning and limitations |

## Verification

Before delivering, verify all criteria below:

- **Reader-first opening:** The opening directly addresses the actual question
  and supplies what this reader and task need first.
- **No forward dependencies:** Every concept, premise, and constraint relied on
  later is defined or established beforehand.
- **Explicit epistemology:** Facts, assumptions, interpretations, and
  recommendations are visibly distinct; uncertainty and verification limits are
  stated.
- **Intact boundaries:** Required content, conditions, exceptions, and
  task-specific deliverable boundaries are preserved without loss.
- **Delivery format constraints:** Requested language and project/domain terms
  are preserved; durable artifacts contain no execution logs, change lists, or
  approval history.
- **Zero filler:** No sentence, heading, or section remains that can be removed
  without impairing understanding, judgment, or action.
