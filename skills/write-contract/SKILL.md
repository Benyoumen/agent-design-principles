---
name: write-contract
description: The rule that decides what may exist on an agent screen. Only a control that can reach the system of record (CRM, ERP, service desk), today or after a field is created, stays; use it to map every control to its destination before scope closes, to audit a screen for buttons that change nothing, or to build the list of fields to create.
license: MIT
metadata:
  version: "1.1"
  author: Benjamin Tayar
  method: agent-design-principles
---

# Write contract

> A button that changes nothing is a false promise. Every control on an agent screen lands in the system of record or leaves.

An agent that suggests is easy to design. An agent that writes is where the product succeeds or fails. The question that decides scope is not a UX question, it is a systems question:

> **Does this action have somewhere to land?**

The rule that follows is strict and saves months:

> **Only what can reach the system of record stays on the screen, today or after a field is created. What has no destination becomes a link to the system.**

Users find a button that changes nothing in the first week, and trust does not come back.

## When to use

| Mode | Use it when | Output |
|---|---|---|
| `map <screen>` | Before scope is closed | A table: control, target object, field, field exists, verdict |
| `audit <screen>` | In review | What is on the screen without a destination, and what to do with each item |
| `fields <specs>` | A decision meeting with the system owner | What exists, what exists but is empty, what must be created |

Run it before `agentic-ux` settles the regime of an action. An action with no destination has no regime.

## Inputs

1. The rendered screen or mockup, to inventory the real controls. Not the generator, not memory.
2. Access to the API of the system of record, or at least its live list of objects and custom fields. Documentation and sample exports are not enough.
3. The spec's write stories, if any, and the vocabulary of the destination system: object and field names, the labels on its own forms.
4. The system owner, who decides whether a field is created.

## Procedure

1. **Inventory the real controls** from the rendered output. Buttons inherited from an older version show up in this sweep.
2. **For each control, answer three questions:** what object is created or changed, which field exactly, does the field exist.
3. **Confirm the field in the API**, not in the documentation. Pull the list of custom fields and a large sample of objects. This is how you find a field the sample export did not show, and the real values behind a dropdown.
4. **Classify the control** as an action on the business object or a response about the recommendation (see Rules). Only the first kind goes to the system of record.
5. **Assign a verdict:** stays (the field exists), stays after creating a field, leaves, or lives in another product without writing back.
6. **Turn the list of fields to create into a decision agenda** for the system owner. A new field changes the system for every user.
7. **State the price of every cut** out loud, for example "we lose the agent's only explicit feedback source and measure by proxy".

The full method, with code for the inventory and the API check and a worked table, is in [reference/mapping-method.md](reference/mapping-method.md).

## Rules

### Two classes of control

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

### The minimum contract of every write

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

### An MCP server is not the write point

An MCP server for a CRM solves a proof of concept in a day and often exposes dozens of write tools. But it is a client, not an audit point. It stores no author, no previous value and no origin.

> **MCP to prove, a single audited write point to ship.** Both talk to the same API underneath. What changes is who keeps the record.

### Mirror the system's vocabulary

If the action becomes an object in the system, the screen uses the system's word. If the CRM says Subject, Description, Owner, Type and Due date, the agent draft, the form and the write all say the same. One word from recommendation to record, so the user translates nothing on the way.

When the system has its own form for the object, mirror it: same field order, same required fields. The developer does not design, they reproduce, and the user recognizes it.

### Limits that always appear

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
- [ ] The price of each cut is stated.

## Example

Acme Field Sales Assistant, the controls on a deal card before the mapping.

Weak:

```
[Create task]  [Snooze suggestion]  [Mark as priority]  [Log reason]
```

`Log reason` writes "dismissed by user" into the deal notes, so sales and management read tool noise in the customer history. `Mark as priority` has no field behind it. `Snooze suggestion` is a fact about the agent, not about the customer.

Corrected, after `map`:

| Control | Class | Object and field | Exists | Verdict |
|---|---|---|---|---|
| Create task | Action on the record | Task: subject, due_date, owner, type | Yes | Stays; the card shows the draft before approval |
| Snooze suggestion | Response about the recommendation | Agent store | n/a | Moves out of the record flow, stored on the agent side |
| Mark as priority | Action on the record | Deal: priority | No, owner declined | Leaves, replaced by "Open in the CRM" |
| Log reason | Response about the recommendation | Agent store, one-choice reason | n/a | Kept as feedback, never written to the deal notes |

Two buttons left on the card, both with a destination, and the agent still learns.

## Definition of done

- The mapping table has one row per control and no empty verdict.
- Every field marked "exists" was confirmed against the live API.
- Responses about the recommendation are stored on the agent side, not in the system of record.
- The list of fields to create is split into exists and filled, exists and empty, does not exist, and the system owner has seen it.
- The write carries the minimum contract.

## Related

- [`agentic-ux`](../agentic-ux/SKILL.md): the regime of each action, once it has a destination. Cheap dismissal is a response about the recommendation and stays on the agent side.
- [`dense-operations-ui`](../dense-operations-ui/SKILL.md): the button verb names the effect in the system; this skill supplies the vocabulary.
- [`autonomy-tiers-and-evals`](../autonomy-tiers-and-evals/SKILL.md): the write trail (proposed value, written value, origin) is the data a shadow comparison runs on.
- [Write contract table template](../../templates/write-contract-table.md) and the [Acme example](../../examples/acme-sales-assistant/write-contract-table.md).
- In the backlog method, the write stories of an agent come from [`agent-brief`](https://github.com/Benyoumen/backlog-writing-system/tree/main/skills/agent-brief).
