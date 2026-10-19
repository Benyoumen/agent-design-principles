# Acme Field Sales Assistant: write contract table

Fictional example. Acme is a made-up company, and so is its CRM. The assistant drafts follow-up emails and updates the CRM after customer meetings. Screen: the "After the meeting" queue.

## Control mapping

| Control | Where it appears | Class | Target in system of record | Field exists? | Verdict | Comment |
|---|---|---|---|---|---|---|
| Create follow-up task | Queue card, meeting detail | Action on record | Task: `subject`, `deal_id`, `due_date`, `type`, `owner_id` | Exists and filled | Stays | The most direct path to value. Mirrors the CRM's own task form |
| Save meeting note | Meeting detail | Action on record | Note on deal: `body` | Exists and filled | Stays | Origin and evidence go in the note body, since notes take no custom fields |
| Update next step | Queue card | Action on record | Deal: `next_step` | Exists and empty | Stays | A zero-cost write: the field exists and nobody fills it in. Adoption question for the CRM owner |
| Confirm close date | Queue card | Action on record | Deal: `expected_close_date` plus a "reviewed on" stamp | Pair only | Stays after creating a field | The date exists, the stamp does not. Without it a confirmation is invisible in the CRM |
| Send follow-up email | Draft panel | Action on record | Email activity logged on the deal, sent from the rep's mailbox | Exists and filled | Stays, capped at tier 2 | Irreversible, so the rep always approves |
| Snooze suggestion | Queue card | Response about recommendation | Agent store only | No | Leaves the record flow | Not a fact about the customer. Not acting already snoozes |
| Dismiss with reason | Queue card | Response about recommendation | Agent store only | No | Leaves the record flow | Becomes instrumentation, not a CRM control |
| Rate the draft | Draft panel | Response about recommendation | Agent store only | No | Leaves the record flow | Used for evals, never written to the deal |
| Log a blocker | Queue card | Action on record | Deal: `blocker_reason` | No | Leaves | Needs a new field the owner declined for now |
| Open in CRM | Every card | Link | None | n/a | Stays | The default exit when there is no field |

## Fields to create (decision agenda)

| Field | Object | Why | Who decides | Decision |
|---|---|---|---|---|
| `reviewed_on` | Deal | Makes a confirmation visible in the CRM | CRM owner | Open |
| `blocker_reason` | Deal | Best explanation for a stalled deal | CRM owner | Declined for now |

## Exists and empty (adoption, not engineering)

| Field | Object | Observation |
|---|---|---|
| `next_step` | Deal | Empty on the whole base. Using it needs no engineering |

## Cuts and their price

| What left | Why | What is lost | How we cope |
|---|---|---|---|
| Snooze suggestion | Changes nothing in the CRM | Nothing | Leaving an item untouched already snoozes it |
| Dismiss with reason | It is an opinion about the agent | The only explicit feedback source | Measure by proxy: drafts approved divided by drafts shown, and edits made before approval |
| Log a blocker | Needs a field the owner declined | The signal that best explains a stall | Revisit when `blocker_reason` is approved |

If precision is needed later, the way back is not to return the dismiss button. It is to compare what the assistant proposed with what the rep actually wrote.

## Minimum write contract (task and note writes)

| Item | Value |
|---|---|
| Author | The approving rep |
| Previous value | The field before the write (empty for new tasks) |
| Proposed value | The assistant's draft |
| Written value | What the rep approved, after edits |
| Origin | Assistant, with the meeting excerpt and the rule that fired |
| Acceptance channel | The "After the meeting" queue |
| Timestamps | Approved at, written at |
| Limitation | "Did not see the rep's inbox or calendar" |
