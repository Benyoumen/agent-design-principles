# The rules, with reasoning and anti-pattern

Companion to [SKILL.md](../SKILL.md). The reasoning matters as much as the rule. Without it a rule becomes superstition and someone breaks it, for a good reason, without knowing. Examples use the fictional Acme Field Sales Assistant.

## 1. Single accent

One brand accent. If it appears on the title, the icon, the border and the button, the eye does not know where to go. The accent says "decide here". If it is in eight places, it says nothing.

When you open an exception, rewrite the rule instead of breaking it in silence. A rule that is broken and not updated becomes a documented lie.

## 2. Ink on accent

Compute the contrast ratio of white and of black on the accent and pick the winner. The answer differs by color: white wins on dark and mid-dark fills, black often wins on bright ones. Black on a bright accent looks odd for two days and then looks obvious.

Implementation note: changing the ink color breaks anything that was already that color. A chip with black text inside a button whose ink turns black disappears. Every ink change needs a sweep of the children.

## 3. Alpha grays

Use `rgba(ink, 0.7)` style values. A fixed gray is chosen against one background. When the text lands on a dark strip, a white panel or a hovered row, it fails. Alpha follows the surface.

## 4. Nothing grayed out

A gray item does not teach what unlocks it. The user clicks, nothing happens, and concludes the tool is broken.

The right form: "Account profile" with no account selected opens a page titled "Choose an account", with a searchable list and the accounts that need attention first.

## 5. Closed scale

An open scale is not freedom, it is entropy. In three weeks there are sizes 9, 9.5, 10 and 10.5 on one screen and nobody knows which is right. Illustrative scale of seven steps (values are an example, pick your own):

```
step 0   smallest label, caption
step 1   secondary text, panel description
step 2   body, row title
step 3   card title
step 4   page title (the ceiling)
step 5   identity name on a dark strip
step 6   KPI number
```

Enforce it with a script that finds font-size literals outside the scale. It catches a stray size on the day it is written.

## 6. System titles

Operations screens have no cover. A very large title eats the fold the content needs and makes the tool look like a marketing site. If the title looks like a document and not like a system, it is too big.

## 7. Legible numbers

Wrong: `43 deals · $26.9M · 12 uncategorized · 8 late` on one line. Right: each number in its own cell, number above, meaning below.

## 8. Label before number

```
OPEN PIPELINE            <- label, caps, small, bold
$870K                    <- number, large
6 deals · synced 07:05   <- the reading of the number, small
```

Without the third line the block is a scoreboard. With it, it is a record.

## 9. Single fact

The eye reads twice and learns nothing when a column repeats what another column already says. Each fact has one home.

## 10. Two buttons

A queue of 25 items with one extra button is 25 extra decisions a day. Corollary: rejecting must cost less than accepting. If accepting is one click and rejecting is three, the queue rots and the agent never learns.

## 11. One question per control

The pattern: one select that mixes answers to "when" with answers to "why". In the Acme queue, a single dropdown on each card offered "snooze to tomorrow", "snooze to next week", "not relevant", "already done" and "wrong data". The first two answer when. The last three answer why. The user compares things that do not compare, and what they choose stops meaning what it appears to mean: a snooze is read as a dismissal, a dismissal is read as a delay.

The fix is two controls. `Snooze` with the when-options, `Dismiss` with the why-options. Each answers one question, and the agent's feedback data becomes usable.

## 12. Constant column is noise

The pattern: a table built from a filter has a column that repeats the filter. In the Acme open-questions table, which lists only the gaps still to ask the customer, a Status column read "to ask" on every row. It is constant by construction.

Remove it and put the constant in the section title: "Open questions (all still to ask)". The question column gets the room it needed.

## 13. Stacked bars

Fixed header, context bar and sub-navigation, in that order. Put the scroll offset on the target section only.

The pattern: a document-level scroll offset and a section-level margin are both set, so they add. Clicking "Needs attention" in the Acme sub-navigation lands the viewport in the middle of the "Later" section, and nobody can say why because each offset looks right on its own. Keep the offset on the target only, assert in a regression check that the document-level offset is `auto`, and measure the landing position in pixels.

## 14. Three ways to close

With the native `<dialog>`, a backdrop click arrives with the dialog itself as the event target, so `if (ev.target === dialog) dialog.close()` works. It is safe only when the dialog has zero padding. With padding, a click on the padding also targets the dialog and closes it by accident.

## Two CSS traps

**Order wins when specificity ties.** A component rule that sets `display: grid`, declared after a hidden-state rule that sets `display: none`, makes the form always visible. Put `display` in state rules, never in the component. When two rules tie, raise the specificity instead of hoping for file order.

**Descendant selectors leak.** `.deal-card b { font-size: 18px }` also hits a `<b>` used for a status inside the same card, and the status becomes a title. Use the child combinator when the rule is about a direct child.

## Measure, do not look

```js
// text alignment (padding does not move the box)
const start = el.getBoundingClientRect().left
            + parseFloat(cs.paddingLeft) + parseFloat(cs.borderLeftWidth);

// control clipped by a scrolling container
rect.right > containerRect.right + 1
```

A control that is clipped at one viewport width is invisible to an audit run at another. On the Acme queue card, the Approve button and the "Open in CRM" link fit at 1440 px and the link is cut at 1280 px. Whoever measures at one width does not measure.
