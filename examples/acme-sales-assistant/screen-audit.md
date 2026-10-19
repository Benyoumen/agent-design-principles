# Acme Field Sales Assistant: screen audit

Fictional example. The audited screen is a first draft of the "After the meeting" queue, written to show what an audit finds.

Screen: After the meeting queue
Primary user and decision: a field sales rep, between meetings, deciding which follow-ups to approve
Viewport widths tested: 1280 and 1440

## 1. Agentic heuristics

| # | Heuristic | Score | Evidence | Fix |
|---|---|---|---|---|
| 1 | Agent presence and state (star) | 0 | The assistant's name appears only in the logo. The top strip says "CRM snapshot, read-only" | Add `Acme Assistant · last run 07:12 · 31 meetings read · 9 drafts · 2 sent · 3 wait for you` |
| 2 | Autonomy regime visible (star) | 1 | A regime label exists on some cards and not others | Show the dot signal on every card and match it to the spec |
| 3 | Draft, review, approve (star) | 1 | The email card has a "Write email" button that opens an empty window | Show the draft in the card, with Approve, Edit and approve, Reject |
| 4 | Layered explanation (star) | 1 | Two cards share the same three-line reason with different numbers | One short reason per card, rule and data behind an expand |
| 5 | Calibrated confidence (star) | 0 | A "Beta" badge on every card | Per-item confidence with a reason when low |
| 6 | Cheap dismissal and correction (star) | 0 | Only a primary button and a "Details" link | Add Not relevant, Already done, Snooze, no modal |
| 7 | Activity trail | 0 | No view of what the assistant did | Add an activity view with Undo |
| 8 | Feedback that teaches | 0 | Thumbs up and down, no consequence | Ask for a reason, state the effect |
| 9 | Undo and reversibility | 1 | Undo exists for notes, not for tasks | Add Undo with a stated window to all writes |
| 10 | Real focus and batching | 1 | "Focus: 25" with eight cards open. The side panel repeats card one | Open only what needs attention now. Summarize the day in the panel |
| 11 | Escape to conversation | 2 | "Ask the assistant" opens a chat with the account loaded | Do not touch |
| 12 | Identity and limits | 1 | A footer line says it cannot read email | Move the limit next to the draft it affects |

## 2. Required states

| State | Present? | Where |
|---|---|---|
| Quiet day | No | |
| Low confidence | No | |
| Duplicate or already handled | No | |
| Agent running | Yes | A thin bar at the top |
| Data unavailable or stale | No | |
| Action executed with undo | Partial | Notes only |
| No access | No | |

## 3. Write contract

- Controls with no destination: Snooze suggestion, Dismiss with reason, Log blocker
- Responses about the recommendation stored outside the system of record: no, dismissals write to the deal notes (move to the agent store)
- Fields to create: `reviewed_on` (open)

## 4. Dense operations rules

| # | Rule | Result | Evidence |
|---|---|---|---|
| 1 | Single accent | Fail | The accent appears on the title, icon, border and button |
| 2 | Ink on accent | Fail | White on the bright accent measures below 4.5:1 |
| 3 | Alpha grays | Pass | |
| 4 | Nothing grayed out | Fail | "Account profile" is disabled with no account selected |
| 5 | Closed scale | Fail | Two sizes outside the scale on the badges |
| 6 | System titles | Pass | |
| 7 | Legible numbers | Fail | `9 drafts · 3 late · 2 stale` on one line |
| 8 | Label before number | Fail | Units live in hover titles |
| 9 | Single fact | Fail | The reason column repeats the date from the value column |
| 10 | Two buttons | Fail | Cards carry four buttons |
| 11 | One question per control | Fail | One select mixes "when" and "why" |
| 12 | Constant column is noise | Fail | A "Source" column says "CRM" on every row |
| 13 | Stacked bars | Pass | |
| 14 | Three ways to close | Fail | The draft dialog closes only by the button |

## 5. Summary

Top three adjustments by return for the primary user:
1. Bring the draft into the card and make Approve the primary action (heuristics 3, 6, 2).
2. Add a presence strip and per-item confidence (heuristics 1, 5).
3. Cut cards to two buttons and one link, and split the mixed select (rules 10, 11).

Do not touch: "Ask the assistant" (heuristic 11), the fixed header and sub-navigation (rule 13).
