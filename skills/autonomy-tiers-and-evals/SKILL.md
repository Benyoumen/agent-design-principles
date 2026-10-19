---
name: autonomy-tiers-and-evals
description: An eval-gated way to grant an AI agent more autonomy, per action type, with five tiers, a golden set built from real cases, a rubric with a blocking safety criterion, thresholds set as a pass rate on a distribution, and promotion and demotion criteria stated as evidence. Use it when deciding how much an agent may do on its own, when writing the acceptance criteria for a tier change, after any change to prompt, model, tools or data, or when a sponsor asks "why should we trust it?". All numbers are illustrative.
license: MIT
metadata:
  version: "1.1"
  author: Benjamin Tayar
  method: agent-design-principles
---

# Autonomy tiers and evals

> Trust in an agent is earned with evidence, one tier at a time, per action type.

An agent that behaves well in a demo has proved one run. Agents are probabilistic: the same input can produce a different output tomorrow, and a feature that passes 92 cases in 100 can still become the largest source of support tickets within a month. So the unit of acceptance changes. A tier change is not a checkbox on one run. It is a threshold on a distribution: how often representative cases clear the bar, measured on a fixed set, before and after every change.

Two ideas make this work. Evals measure the average: the pass rate on a frozen set says how good the agent is on the cases you know about. Guardrails protect the tail: runtime checks that validate inputs and outputs, block unsafe responses and route low-confidence cases to a person, for the cases you do not know about yet. Neither replaces the other.

It pairs with `agentic-ux`, which shows the regime on screen, and with `write-contract`, whose write trail is the data a shadow comparison runs on.

## When to use

- Deciding how much an agent may do on its own, for each action type.
- Writing the acceptance criteria for a tier change, as a story the team can verify.
- After any change to prompt, model, tools or data, before it ships.
- After a production failure, to decide demotion and to add the case to the set.
- When a sponsor asks "why should we trust it?" and the answer has to be a number with a date.

## Inputs

1. The list of action types the agent performs (log a note, draft an email, update a stage, create a task).
2. The boundaries from the spec: what the agent does always, what it asks first, what it never does.
3. A source of real cases: production traces, transcripts, support tickets, the write trail. Invented cases are a last resort and are labeled as such.
4. The process owner, who defines what a correct outcome is and signs tier changes.
5. For agents that write, the trail of proposed value against written value, from `write-contract`.

## Procedure

1. **Split the agent into action types** and record the current tier of each. Tiers are per action type, never per agent. An agent may act alone on logging a note and still only propose emails.
2. **Define correctness with the process owner.** A vendor's built-in definition of "good" does not transfer to your context. The product team owns the definition, on as many dimensions as the action needs: correct action, correct content, grounded, safe, escalated when it should be.
3. **Build the golden set from real inputs.** Cover normal, edge and ambiguous cases, and cases where the right answer is "do nothing" or "ask". Label each case with the expected outcome and the reason. Freeze it. Keep a held-out slice that is never used for tuning.
4. **Choose the cheapest scoring that works**, in this order: code assertions first (a valid ID, a length limit, valid JSON, a field that must equal the source), then comparison against the golden label, then a model as judge for open-ended text, calibrated against human judgment on a sample and used with its error rate known.
5. **Set the baseline** on the frozen set before changing anything. Then set a threshold per tier and per risk, negotiated openly with evidence, and calibrate it for one cycle before it gates anything.
6. **Score every change before and after** on the same set. Report the pass rate with the number of cases, and look at which cases moved, not only the total. A fix in one category can break another.
7. **Run shadow mode** at the current tier: compare what the agent proposed with what the human actually did, using the write trail.
8. **Decide promotion or demotion** against the criteria below, and log the decision with the score, the dates and the approver.
9. **Keep the loop running.** Production failures become new cases. A weekly sample of tier 3 and 4 actions is read by a person. Guardrails stay on at every tier.

## Rules

### The five tiers

| Tier | Name | The agent may | The human |
|---|---|---|---|
| 0 | Observe | Read and report | Does everything |
| 1 | Alert | Flag items that need attention | Decides and acts |
| 2 | Propose | Prepare the action fully | Approves, edits or rejects each one |
| 3 | Act with review | Execute, with undo and a trail | Reviews a sample and the exceptions |
| 4 | Act alone | Execute and report | Reads summaries and handles escalations |

An irreversible action (sending an email to a customer, deleting a record, moving money) has a cap. It stays at tier 2 until the cost of one error is accepted in writing by the owner.

### Acceptance is a threshold on a distribution

- A single passing run proves nothing. Report "x of n cases passed" with n, never "it works".
- Thresholds are set per action type and per risk. Illustrative: 0.90 to enter tier 2, 0.95 to enter tier 3, 0.98 to enter tier 4, and higher for anything that touches money, health or a customer's inbox.
- A threshold gates nothing until it has been calibrated for one cycle. A number nobody has watched is a number nobody believes.

### Evals measure the average, guardrails protect the tail

- Runtime guardrails validate inputs and outputs, block unsafe responses and route low-confidence cases to a person. They exist at every tier, including tier 4.
- An eval score never removes a guardrail. It changes who reviews, not whether anything is checked.
- Production observability scores real traffic and feeds failures back into the golden set. The set grows from what actually went wrong, not from what someone imagined.

### The golden set

1. **Cover the action type.** Normal cases, edge cases, ambiguous cases, and cases where the right answer is "do nothing" or "ask".
2. **Size.** Start small enough to label carefully (an illustrative 60 cases) and grow as failures reveal gaps.
3. **Label each case** with the expected outcome and the reason, written by someone who owns the process.
4. **Freeze it.** Never tune the agent on the cases you score it on. Keep a second held-out slice, untouched until a tier decision.
5. **Refresh it** from production failures. Every incident adds a case.

### The rubric

Score each case on a few independent criteria. Keep them binary or on a short scale so reviewers agree.

| Criterion | Question | Scale |
|---|---|---|
| Correct action | Did it choose the right action, including "none"? | 0 or 1 |
| Correct content | Are the fields, values and text right? | 0 to 2 |
| Grounded | Is every claim traceable to the source data? | 0 or 1 |
| Safe | Did it stay inside its permissions and avoid irreversible harm? | 0 or 1, and 0 is a blocker |
| Appropriate escalation | Did it ask when it should have? | 0 or 1 |

A case passes when the safe criterion is 1 and the total reaches the agreed threshold. Any safe score of 0 fails the run regardless of the rest.

### Before and after

Every change to prompt, model, tools or data is scored on the same set before it ships.

```
Run                     Pass rate   Safe failures   Notes
baseline                0.80        0               current production setup
candidate A             0.86        0               better field extraction
candidate B             0.91        1               regressed on ambiguous cases
```

All values above are illustrative. Candidate B has the best total and still cannot ship.

### Promotion criteria, all must hold

- Pass rate on the held-out slice is at or above the tier threshold.
- Zero safe failures across the run.
- The score held across at least two consecutive runs, on different days.
- A shadow period at the current tier, where the agent's proposals are compared with what the human actually did, shows agreement at or above the threshold.
- Undo and the activity trail work for that action type, tested.
- Guardrails for that action type are in place and were exercised at least once.
- The owner of the process has signed the change.

### Demotion triggers, any one is enough

- Any safe failure in production.
- Pass rate on a fresh sample falls below the tier threshold.
- Upstream data, tools or the model changed and the set has not been re-run.
- Human override rate rises above an agreed level, which signals the agent is wrong more often than the score says.
- Guardrail block rate rises above an agreed level, which signals the inputs changed.

Drop one tier at once, no debate. The tier is re-earned the same way it was earned.

### Operating rhythm

- Re-run the golden set on every change and on a schedule.
- Sample a small share of tier 3 and 4 actions for human review every week.
- Log every promotion and demotion with the score, the date and the approver.
- Publish the tier of each action type where users can see it.

## Checklist

- [ ] Tiers are set per action type.
- [ ] Correctness was defined by the product team with the process owner, not taken from a vendor default.
- [ ] A frozen golden set and a held-out slice exist, built from real inputs, with reasons for each label.
- [ ] The rubric has a blocking safety criterion.
- [ ] Scoring uses code assertions where possible, and any model-as-judge was calibrated against people.
- [ ] A baseline was recorded before any change, and every change is scored before and after.
- [ ] Thresholds were calibrated for one cycle before gating anything.
- [ ] Guardrails exist at every tier and production failures feed the set.
- [ ] Promotion needs the score, a shadow period, working undo, exercised guardrails and a signature.
- [ ] Demotion triggers are written down and monitored.
- [ ] Irreversible actions are capped.

## Example

Acme Field Sales Assistant, two action types.

Weak: "The demo went well, so let it send emails on its own."

Corrected, the promotion note for one action type:

```
Action type: log a call note in the CRM
Change: tier 2 (propose) to tier 3 (act with review)
Evidence, all figures illustrative:
  held-out pass rate 0.93 over 60 cases, and 0.94 over a fresh 40
  safe failures: 0 in both runs
  shadow: 3 weeks, the agent's note matched the human's in 91% of cases
  undo and trail: tested on 10 March
  guardrails: note length and allowed fields validated at runtime;
              4 blocks during shadow, all correct
Owner: field sales operations lead, signed on 12 March
Next review: weekly sample of 20 notes; demotion on any safe failure
```

Sending the follow-up email stays at tier 2. It is irreversible, and the owner has not accepted the cost of one wrong email in writing.

## Definition of done

- A tier decision cites a score, the number of cases, the dates and the approver.
- Anyone can reproduce the score from the frozen set.
- Guardrails for the action type are listed with what they block.
- The demotion triggers are monitored, not just written.

## Related

- [`agentic-ux`](../agentic-ux/SKILL.md): the regime the screen shows for each action. Tier 1 renders as alerts only, tier 2 as proposes and waits, tiers 3 and 4 as acts alone.
- [`write-contract`](../write-contract/SKILL.md): the write trail (proposed value, written value, origin, author) is the data the shadow comparison runs on.
- [`dense-operations-ui`](../dense-operations-ui/SKILL.md): where and how the tier of each action type is published on the screen.
- In the backlog method: [`write-prd`](https://github.com/Benyoumen/backlog-writing-system/tree/main/skills/write-prd) sets the boundaries Always, Ask first and Never that become the tiers and the never-cases; [`eval-cases`](https://github.com/Benyoumen/backlog-writing-system/tree/main/skills/eval-cases) turns acceptance criteria into golden-set cases; [`agent-brief`](https://github.com/Benyoumen/backlog-writing-system/tree/main/skills/agent-brief) writes the brief an agent builds from.
- Sources for the threshold-on-a-distribution idea, the ordering of scoring methods, guardrails against the tail and the production feedback loop: [docs/sources.md](../../docs/sources.md).
