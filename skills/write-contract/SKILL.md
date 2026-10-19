---
name: write-contract
description: The rule that decides what may exist on an agent screen. Only what can reach the system of record (CRM, ERP, service desk), today or after a field is created, stays. Use when deciding whether a feature enters or leaves a mockup, when answering "can this be written to the CRM?", when building the list of fields to create, or when specifying a write user story.
---

# Write contract

An agent that suggests is easy to design. An agent that writes is where the product succeeds or fails. The question that decides scope is not a UX question, it is a systems question:

> **Does this action have somewhere to land?**

The rule that follows is strict and saves months:

> **Only what can reach the system of record stays on the screen, today or after a field is created. What has no destination becomes a link to the system.**

A button that changes nothing is a false promise, and users find out in the first week.

## Modes

| Mode | Use it when | Output |
|---|---|---|
| `map <screen>` | Before scope is closed | Table: control, target object, field, field exists, verdict |
| `audit <screen>` | Review | What is on the screen without a destination, and what to do with each |
| `fields <specs>` | A decision meeting | What exists, what exists but is empty, what must be created |

## Two classes of control

Mixing these causes circular arguments. Separating them ends them.

| | Action on the business object | Response about the recommendation |
|---|---|---|
| Examples | create a task, change a category, confirm a date, assign an owner | dismiss a suggestion, snooze it, rate it |
| It is a fact about | the customer | the agent |
| Goes to the system of record? | Yes, always. With no destination it leaves the screen | Never. It would fill the customer history with tool noise |
| Lives in | a field in the CRM | the agent's own store |
| Without it | the product does not deliver | the agent cannot be measured |

The rule works in both directions. If the system of record is the source of truth and there are no parallel writes outside it, then what is not a fact about the customer does not go into it either.

Where this usually jams: someone wants to measure agent quality and proposes saving "suggestion dismissed" in a note on the deal. That note is read by sales, management and pre-sales. Filling it with tool noise makes the conversation history unreadable. Measure in the agent's store, or by proxy: items handled divided by items shown, and how often the screen sent the user to the system.

## The mapping method

Full method, with a worked example, in [reference/mapping-method.md](reference/mapping-method.md).

1. **Inventory the real controls** from the rendered output, not from memory. Buttons inherited from an older version show up in this sweep.
2. **For each control answer:** what object is created or changed, which field exactly, does the field exist.
3. **Confirm the field in the API**, not in the documentation. Pull the list of custom fields and a large sample of objects. This is how you find a field the sample export did not show, and the real values behind a dropdown.
4. **Assign a verdict:** stays (field exists), stays after creating a field, leaves, or lives in another product without writing back.
5. **The list of fields to create is a decision agenda**, not a technical backlog. A new field changes the system for every user.

## The minimum contract of every write

Without this set the write happens but cannot be contested, and trust drops at the first disagreement.

| Item | Content |
|---|---|
| Author | who approved, always identified |
| Previous value | what was there before |
| Proposed value | what the agent suggested, even if the human adjusted it |
| Written value | what actually went in |
| Origin | who produced it and the evidence (conversation excerpt, rule applied) |
| Acceptance channel | where the human approved |
| Timestamps | acceptance and write, kept separate |
| Limitation | what the agent did not see when it proposed |

## An MCP server is not the write point

An MCP server for a CRM solves a proof of concept in a day and often exposes dozens of write tools. But it is a client, not an audit point. It stores no author, no previous value and no origin.

> **MCP to prove, a single audited write point to ship.** Both talk to the same API underneath. What changes is who keeps the record.

## Mirror the system's vocabulary

If the action becomes an object in the system, the screen uses the system's word. If the CRM says Subject, Description, Owner, Type and Due date, the agent draft, the form and the write all say the same. One word from recommendation to record, so the user translates nothing on the way.

When the system has its own form for the object, mirror it: same field order, same required fields. The developer does not design, they reproduce, and the user recognizes it.

## Limits that always appear

- Sub-objects often do not accept custom fields. What must travel along (origin, evidence) has to fit in an existing text field or become a note on the parent.
- Creating a field changes the schema for the whole account. That decision belongs to the system owner, not the developer.
- A "reviewed on" stamp almost never exists. Confirming a value without a stamp is invisible: the field stays the same and nobody knows it was reviewed.
- Stage change history is rarely exposed. Without it, "how long in this stage" cannot be computed, and that is the sharpest cut there is.

## Checklist

- [ ] Every control on the screen is classified as an action on the record or a response about the recommendation.
- [ ] Every action on the record has a target object and a field, confirmed in the API.
- [ ] Responses about the recommendation are stored in the agent's store, not in the system of record.
- [ ] Controls with no destination are removed or replaced with "Open in the system".
- [ ] The write carries the minimum contract.
- [ ] Labels match the system's vocabulary.
- [ ] The price of each cut is stated, for example "we lose explicit feedback and measure by proxy".

## Good and bad

Good: `Create task` writes a task with subject, due date, owner and type, and the card shows the draft before approval.

Bad: `Snooze suggestion` on a record screen, with no field behind it. It changes nothing in the system, so it is either an agent-store response (move it out of the record flow) or it leaves.

Bad: `Log reason` writing "dismissed by user" into the deal notes.

## Definition of done

- The mapping table has one row per control and no empty verdict.
- Every field marked "exists" was confirmed against the live API.
- The list of fields to create is split into exists and filled, exists and empty, and does not exist.
- The system owner has seen the list of new fields.
