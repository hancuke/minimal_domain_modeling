# Information and Retention

Use this reference when modeling information, history, or recovery. It describes
business retention requirements, not database schemas or storage technology.

## Evaluate two independent dimensions

| Dimension | Categories | Question |
|---|---|---|
| Information nature | Authoritative fact / Derived information / Operational intermediate / Undecided | Where does this information come from, and what does it mean? |
| Retention requirement | Retain / Reconstruct without retaining / No retention needed / Undecided | What must remain available beyond this operation, and why? |

A derived result may need retention. An operational intermediate may need to
survive an interruption. Entity identity does not imply retention, and value
semantics do not imply transience.

Classify meaningful information within a concept separately when necessary.
An approval can contain both authoritative decisions and a derived progress view;
assigning one category to the whole concept can hide that distinction.

## Make a retention decision

For each relevant information item:

1. Identify its source and whether it records something that actually occurred.
2. Determine whether business continuity, historical explanation, audit, or an
   external obligation requires it to remain available.
3. If reconstruction is proposed, identify the retained source facts, historical
   inputs, rule versions, and other context needed to reproduce the required
   meaning. Recomputing with today's rules is not historical reconstruction.
4. Check whether losing the item would erase a commitment, decision, assignment,
   or other business fact even if a current summary remains calculable.
5. Record the retention requirement and reason. Leave retention duration or other
   obligations undecided when the requirements do not establish them.

Materializing data for performance is a later implementation choice. Keep it
separate from the business requirement to preserve a record.

## Examples of distinctions

- **Approval progress:** Usually derived from decisions. A progress snapshot
  presented to an external party may itself need retention if that presentation
  has business significance.
- **Pending approval task:** Derived if it is only a view of unfinished work.
  Independent assignment, claiming, transfer, or deadlines can introduce facts
  and a lifecycle that the view alone cannot reconstruct.
- **Eligibility result:** Calculated, but a historical decision may require
  retained inputs, applicable policy version, or the issued result. Ask what
  must be explainable; do not assume today's calculation is sufficient.
- **Intermediate reservation:** May look temporary, but if it represents a
  business commitment that must survive interruption, it needs retention.

These examples illustrate questions, not default rules for every domain.

## Report only relevant decisions

| Information item | Nature | Retention requirement | Source or reconstruction basis | Business reason / Unknown |
|---|---|---|---|---|
| ... | ... | ... | ... | ... |

Verify that required history and commitments survive restart, and that any
reconstruction claim names sufficient preserved inputs. Surface missing inputs
or ambiguous obligations as unresolved decisions.
