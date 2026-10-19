# The mapping method, step by step

Companion to [SKILL.md](../SKILL.md). Examples use the fictional Acme Field Sales Assistant and a fictional CRM.

## 1. Inventory the real controls

Extract from the rendered HTML, not from memory and not from the generator. This is where buttons left over from older versions appear.

```python
import re, collections
body = re.sub(r'<(script|style)[^>]*>.*?</\1>', ' ', HTML, flags=re.S)
buttons = collections.Counter()
for m in re.finditer(r'<button\b([^>]*)>(.*?)</button>', body, re.S):
    label = re.sub(r'\s+', ' ', re.sub(r'<[^>]+>', '', m.group(2))).strip()
    data = " ".join(sorted(set(re.findall(r'\bdata-([a-z-]+)', m.group(1)))))
    if label:
        buttons[(label[:44], data)] += 1
```

Expect to find controls nobody remembers adding, often with no destination.

## 2. Confirm the field in the API

Two findings only the API gives, and both change scope:

```python
# a) the custom fields that exist today in the account
fields = crm.call("custom_fields_list", {"page": {"size": 200}})

# b) the real values behind a dropdown, and their distribution
tasks = crm.paginate("tasks_list", {}, size=200, max_pages=6)
collections.Counter(t.get("type") for t in tasks)
```

The second pays off most. The label the user sees ("Call") and the slug the API stores (`call`) are not the same, and the developer needs both.

A field can exist in the API and be missing from a sample export simply because it was empty in that sample. Check the field list, not only the sample.

## 3. The table

One row per control. Six columns are enough. Eight squeeze the comment column, which is where the decision lives. A blank template is in [templates/write-contract-table.md](../../../templates/write-contract-table.md) and a filled one in [examples/acme-sales-assistant](../../../examples/acme-sales-assistant/write-contract-table.md).

### Verdicts

- **Stays.** The field exists today. Shortest path to value.
- **Stays after creating a field.** Worth it, and the system owner approves the new field.
- **Leaves.** No destination and no reason to exist.
- **Lives in another product.** Belongs to a neighboring product and does not write back to the system of record.

## 4. The list of fields to create is an agenda

Split it in three, because each conversation is different:

| Situation | Conversation |
|---|---|
| Exists and is filled | Nothing to do |
| Exists and is empty | Adoption, not engineering: why does nobody fill it in? |
| Does not exist | Decision for the system owner: each new field shows up for every user |

The middle group is usually the most valuable and the most ignored. A field that exists and is empty across the whole base is a zero-cost write waiting for someone to decide to use it.

## 5. When the rule cuts deep

Applied for real, the rule removes features that looked right.

| What left | Why | What is lost |
|---|---|---|
| Snooze suggestion | Changes nothing in the system | Nothing: not acting already snoozes |
| Dismiss with reason | It is an opinion about the agent | The only explicit feedback source, now measured by proxy |
| Log a pause | Needs a new field the owner declined | One of the risk signals |
| Log a blocker | Same | The signal that best explains a stalled deal |

State the price out loud. When the rule removes the agent's only explicit feedback source, record that as a consequence, not a detail. The way back, if precision is ever needed, is not to return the button. It is to compare what the agent proposed with what the human actually wrote, which needs the write trail anyway.

## 6. Mirror the system's form

If the target system already has a screen to create that object, copy its field order and required fields. You get three things free: the developer does not design, the user recognizes the form, and the field list is already the API list.

```
Deal               (locked; switching deals is another item)
Subject *          -> name
Description        -> description
Owner *            -> owner_ids
Type *             -> type   (closed list, with the API slug)
Due date *         -> due_date
[Cancel]  [Save to the CRM]
```

The footer button says where the data lands. The verb on the card said what will be born. Together they close the sentence.
