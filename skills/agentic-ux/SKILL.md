---
name: agentic-ux
description: A UX model for systems where an AI agent proposes, executes or monitors actions on behalf of a human. Use when designing, critiquing or reshaping a screen for an agent, copilot or AI decision queue. Covers agent presence, autonomy regimes, draft-review-approve, layered explanations, per-item confidence, cheap dismissal, activity trail, undo and the states a mockup must show.
---

# Agentic UX

Visual craft skills cover hierarchy, type, color and accessibility. They do not describe what makes an interface agentic: where the agent is, what it does alone, how the human approves, corrects, undoes and teaches it. Without that model, an agent screen turns into a dashboard with buttons. It looks fine and the agent is invisible.

This skill gives you three parts: twelve heuristics, a matrix of required states and a scoring rubric. Scoring detail is in [reference/critique-rubric.md](reference/critique-rubric.md).

## Modes

| Mode | Use it when | Output |
|---|---|---|
| `critique <screen>` | Before a screen goes to review | A 0-2 score per heuristic, gaps ranked by impact on the primary user, and the missing states |
| `shape <surface>` | Before designing or rewriting a surface | An agent brief: autonomy regime per action, required components, states to show |
| `states <screen>` | The mockup only shows the happy path | The states that are missing, with the text and structure of each |

## Setup

1. Name the primary user and the decision they make on this screen. Everything is scored against that person, not against a generic user.
2. Read the autonomy section of the spec for the surface. If none exists, write the regime for each action first, using the three regimes below.
3. Read the list of critical scenarios in the spec. Those become the required states.

## The three autonomy regimes

Every action the agent can take sits in exactly one regime, and the screen shows which.

| Regime | Meaning | Signal |
|---|---|---|
| Acts alone | The agent does it and tells the human afterward | Filled dot |
| Proposes and waits | The agent prepares it, the human approves | Half dot |
| Alerts only | The agent flags it, the human does everything | Empty dot |

If every action on a screen is "alerts only", there is no agent, only a to-do list.

## The twelve heuristics

Stars mark the ones that are critical when the primary user decides under time pressure.

1. **Agent presence and state (star).** Within one second the user knows an agent exists, when it last ran, what it did and what it is doing now. One line is enough: name, last run, items read, actions proposed, actions executed, items waiting for you. Data provenance ("snapshot from Monday") is a different thing and both should be present.
2. **Autonomy regime visible (star).** Each action shows its regime as a standard signal on the item, not in running text. The regime matches the spec.
3. **Draft, review, approve (star).** The agent does not tell the human to create a task. It brings the task ready, with title, due date, owner and note, and the human approves, edits or rejects in one click. The primary button says Approve or Edit and approve, never Create.
4. **Layered explanation (star).** Layer 1 is one line, under about 90 characters, specific to the item. Layer 2 is expandable: the rule applied, the data used, the limitation. Layer 3 is a link to the source. The same explanation text on two different items is a bug: it shows a template and kills scanning.
5. **Calibrated confidence per item (star).** Each recommendation carries its own confidence, with the reason for doubt when it is low. A global "beta" badge on every card is a disclaimer, not confidence.
6. **Cheap dismissal, snooze and correction (star).** Every queue item has Not relevant, Already done and Snooze until, in one click and without a modal. Rejecting must cost less than approving. Without this the agent cannot learn and the queue rots.
7. **Activity trail.** A view of what the agent did, with timestamp, action, target, regime, who approved, result and an undo link. This is what a sponsor reads before agreeing to give the agent more autonomy.
8. **Feedback that teaches.** Dismissal asks for the reason in one choice (wrong rule, wrong data, wrong timing, already handled) and shows the effect ("I will not suggest this for this account for 30 days"). Feedback with no visible consequence is not believed.
9. **Undo and reversibility.** Anything the agent executed, or the human approved, can be undone inside a stated window. Irreversible actions, such as sending an email to a customer, show their consequence first and need explicit confirmation.
10. **Real focus and batching.** A focus list of 25 items is not focus. Open the items that need attention now and collapse the rest by type, with a batch action ("approve the 5 log-contact items"). The side panel summarizes the agent's day instead of repeating the first card.
11. **Escape to conversation.** On any item, "Ask the agent about this account" opens a conversation with the item's context loaded. A structured screen plus chat is a hybrid, not a choice.
12. **Identity and limits.** The agent has a name and a quiet, consistent mark, and it says what it does not do and what it did not see ("I cannot read your calendar, check before you conclude"). Limits sit next to the decision they affect, not in a footer.

## Required states

A screen that shows only the happy path is incomplete. Each surface delivers these in the same file, as tabs, a scenario toggle or a states section.

| State | What to show |
|---|---|
| Quiet day | The agent confirms it checked everything, nothing needs action, and suggests what to do with the time |
| Low confidence | The item with its reason, and the regime lowered to alerts only |
| Duplicate or already handled | The agent notices the human already acted and collapses the item, with the reason and a way to reopen |
| Agent running | A non-blocking progress indicator, with new items marked as new |
| Data unavailable or stale | What it could not read, since when, and what that invalidates |
| Action executed | A toast or trail line with Undo and the window |
| No access | A scope message that does not leak data |

## Scoring

For each heuristic: 0 absent, 1 present but generic, hidden or inconsistent, 2 present, specific and one click away. Report a 12-row table with score, evidence and fix, then the missing states, then the three adjustments with the highest return for the primary user, then what not to touch because it already scores 2. Do not score visual craft.

## Anti-patterns

- The agent is invisible: its name appears only in the logo.
- The regime is implied by the button verb.
- Confidence is a global disclaimer.
- The side panel repeats the card instead of summarizing the day.
- Explanation text built from a template with visible placeholders.
- The primary button sends the human to do work the agent should have drafted.

## Definition of done

- The regime of every action is visible and matches the spec.
- Heuristics marked with a star score 2.
- All seven states exist in the deliverable.
- A reviewer can name, for each action, what the agent did, what the human decides and how to undo it.

## References

Public work this model builds on: Microsoft HAX Toolkit (Guidelines for Human-AI Interaction), Google PAIR People + AI Guidebook, the Shape of AI pattern library.
