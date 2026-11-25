# Skill Authoring Policy

Organize skill body content around executable behavioral constraints: establish
contracts upfront, guide decisions with principles, structure actions with
workflows, and verify observable outcomes.

## Body Structure: Contract - Principles - Workflow - Verification

| Section | Role | Key Content |
| --- | --- | --- |
| **Contract** | Boundaries & Gates | Required inputs, non-goals (what to refuse), handoff targets, and mandatory approval gates where the agent must stop. |
| **Principles** | Decision Rules | Domain invariants and orthogonal trade-offs, reinforced with matched Good / Avoid examples. |
| **Workflow** | Action & Artifacts | Sequenced steps (only when order matters), intermediate deliverables, and explicit stopping conditions. |
| **Verification** | Definition of Done | Observable evidence, concrete validation criteria, and delivery format constraints. |

Cross-cutting policy skills without a staged execution flow may omit
inapplicable sections (such as **Workflow**) and non-applicable contract clauses
rather than padding them.

## Writing rules

- **Invariants and gates over procedures**: Guide decisions with boundaries, invariants, and explicit approval gates. Use ordered steps only when sequence strictly matters.
- **Fair Good / Avoid contrasts**: Compare Good and Avoid under identical conditions and isolate the decisive difference.
- **Core in main, detail lazy**: Keep essential permissions and boundaries in the main skill; offload infrequent details to references with explicit reading conditions.
- **High signal density**: Omit filler and narrative. State each requirement once; every sentence must directly constrain a decision or verify an outcome.
