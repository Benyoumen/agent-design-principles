# Critique rubric for agentic screens

Companion to [SKILL.md](../SKILL.md). Use it when running `critique`. For each heuristic: what to look for, a passing example and the usual anti-pattern. Examples use Acme Field Sales Assistant, a fictional agent that drafts follow-up emails and updates the CRM after customer meetings.

## Scoring

- **0** absent. Nothing in the screen meets the heuristic.
- **1** present but weak: generic (same text on every card), hidden (footer, tooltip, second click), inconsistent (only some items) or not actionable.
- **2** present and strong: specific to the item, visible in the first fold or one click away, consistent across the screen.

Report order: (1) table of 12 rows with score, evidence (quote or selector) and fix, (2) missing states, (3) top three adjustments by return for the primary user, (4) what not to touch.

## 1. Agent presence and state

Look for: a line at the top with the agent name, time of the last run, counts (read, proposed, executed, waiting) and current state (idle, running, error).

Passes: `Acme Assistant · last run 07:12 · 31 meetings read · 9 drafts proposed · 2 sent · 3 wait for you`

Anti-pattern: `Snapshot from the CRM, read-only`. That is data provenance. It does not say an agent exists or what it did.

## 2. Autonomy regime visible

Look for: a standard signal per card showing one of three regimes.

| Regime | Label | Signal |
|---|---|---|
| Acts alone | Automatic | filled dot |
| Proposes and waits | Needs approval | half dot |
| Alerts only | Alert | empty dot |

The signal must match the autonomy section of the spec. If the spec says log-contact is automatic after two approvals, the card says so.

Anti-pattern: every card has a "Create task" button. The human executes everything and the agent is a checklist.

## 3. Draft, review, approve

Look for: the object of the action already filled in on the card or in a side panel, with approve, edit and reject controls.

Passes:

```
FOLLOW-UP EMAIL · needs approval
To: Dana Whitfield (Northgate Logistics)
Subject: Next steps after Tuesday's pricing review
Draft: 3 short paragraphs, mentions the security questionnaire
[Approve]  [Edit and approve]  [Reject v reason]
```

Anti-pattern: a "Write email" button that opens an empty compose window.

## 4. Layered explanation

Look for: a short layer 1 that is unique per item, layer 2 in an expandable block with rule, data and limitation, layer 3 as a source link.

Passes: layer 1 `Proposal sent 9 days ago, no reply` then expanded: `Rule: proposal sent and no customer activity for 7+ days. Data: sent on the 3rd, last CRM activity on the 4th. Limit: I cannot see your inbox.`

Anti-pattern: a three-line paragraph repeated on two cards with different numbers. It takes 40 percent of the card and forces a full read to find what changed.

## 5. Calibrated confidence

Look for: a confidence indicator that varies between items, with a reason when low.

Passes: `High confidence` (deterministic rule on complete data). `Low confidence: deal stage missing` (demotes the item to Alert).

Anti-pattern: a `Beta` badge on 100 percent of cards. It does not discriminate, so users learn to ignore it.

## 6. Cheap dismissal, snooze, correction

Look for: secondary actions on each card (`Not relevant`, `Already done`, `Snooze v tomorrow / next week / after the meeting`), no modal, with undo.

Anti-pattern: a card with only the primary action and a "Details" link. There is no way to clear it from the queue.

## 7. Activity trail

Look for: an "Agent activity" view with lines such as `07:12 · Logged call note · Northgate Logistics · automatic · [Undo]`, filterable by regime and result.

Anti-pattern: missing, or only a "Data sources" page, which is provenance and not activity.

## 8. Feedback that teaches

Look for: on dismissal, a one-choice reason and a stated effect.

Passes: `Why? ( ) wrong rule ( ) wrong data ( ) timing ( ) already handled` then `Understood. I will not suggest a follow-up for Northgate for 30 days. [Undo]`

Anti-pattern: thumbs up and down with no consequence.

## 9. Undo and reversibility

Look for: a toast or trail line with Undo and a window ("until 18:00"). Irreversible actions show a preview of the consequence and ask for explicit confirmation.

## 10. Real focus and batching

Look for: only "Needs attention now" open by default, other groups collapsed with counts, batch actions on homogeneous groups, and a side panel with a summary of the agent's day instead of a copy of card one.

Anti-pattern: "Focus: 25" with eight cards open and a side panel duplicating card one.

## 11. Escape to conversation

Look for: a per-item "Ask the assistant" that opens a conversation with the item's context, and a free input at the top.

## 12. Identity and limits

Look for: a consistent name and mark, limits placed next to the decision they affect, and a one-time statement of what the agent does not do.

## Reference text for states

- **Quiet day:** `I read 31 meetings at 07:12. Nothing needs action today. For the free time: 3 accounts with no contact in 20+ days.`
- **Low confidence:** card with `Low confidence`, the reason, regime `Alert` and a `Confirm data` button instead of `Approve`.
- **Duplicate:** `I collapsed "Log call: Northgate". You logged it at 09:40. [Reopen]`
- **Running:** a thin bar `Syncing with the CRM... 60%` and new items tagged `New`.
- **Stale:** `CRM unavailable since 06:50. The proposals below use yesterday's data and stage values may be out of date.`
- **Executed with undo:** `Note saved to the CRM. [Undo] until 18:00`
- **No access:** `This account belongs to another territory. Ask for access or see the public summary.`

## Cross-cutting anti-patterns

- The word "agent" or its name appears only in the logo.
- Explanation text with placeholders that show when read.
- Autonomy implied by the verb on the button.
- Confidence as a global disclaimer.
- Only the happy path in the file.
- The primary button hands the human work the agent should have drafted.
