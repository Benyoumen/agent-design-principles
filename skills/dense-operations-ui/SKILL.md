---
name: dense-operations-ui
description: Design rules for a dense operator screen that a professional opens every day, under time pressure, to decide rather than to admire. Portable to any operator panel, decision queue, agent console, internal CRM or back-office tool. Fourteen named rules on color, type, numbers, density, action and behavior, plus a button vocabulary and a list of anti-patterns. Use when designing, critiquing or rewriting an operations screen, or when a screen "looks like AI slop" or "is too crowded".
---

# Dense operations UI

An operations screen fails in two opposite ways. It is flat and generic, so it looks like an admin template and the user cannot remember where they were. Or it is baroque: every concept became a chip, every rule became a line of text, and the user stops reading. These rules avoid both at once. Each one is named because a name can be cited in review.

The audience is always the same: someone who opens this every day, in a hurry, to decide. Not the buyer, not an executive in a presentation, not the designer admiring the work.

Detail, reasoning and anti-pattern per rule are in [reference/rules.md](reference/rules.md).

## Modes

| Mode | Use it when | Output |
|---|---|---|
| `rules` | Before designing or reviewing | The rules with their numbers, ready to paste into a brief |
| `critique <target>` | After rendering | Violation by violation: the rule broken, the evidence, the fix |
| `apply <target>` | When rewriting the generator or stylesheet | The proposed diff, rule by rule |

Recommended order: `agentic-ux` (what the screen must say about the agent), then this skill (how to say it without clutter), then a visual polish pass.

## The fourteen rules

### Color and brand

1. **Single accent.** One brand accent and only one. It does not compete with content, info badges or surfaces. If two accent elements fight for the same fold, one is wrong. One declared exception is allowed: a "commit" color for the button that writes to the system of record, because it never shares a fold with the accent.
2. **Ink on accent.** Text on an accent fill uses whichever ink measures at or above the contrast threshold (4.5:1 for body text under WCAG), not the one that looks obvious. White fails on many bright colors. Measure both.
3. **Alpha grays.** Every gray text is the main ink with transparency, not a hex of its own. A solid gray is chosen against one background and fails on the next.
4. **Nothing grayed out.** Navigation is never disabled for lack of a selection. A destination that needs context opens a picker. Never leave an item gray waiting for the user to guess what unlocks it.

### Type and number

5. **Closed scale.** A fixed set of type steps, about seven, and no literal size outside it. A stray size is debt: it becomes a documented step or goes back to the scale.
6. **System titles.** Titles stay small. Hierarchy comes from weight, tracking and space, not from font size. A tool has no cover page.
7. **Legible numbers.** A number that carries a decision is never smaller than the label that explains it. The pair is number above, meaning below. Never a sentence with three numbers squeezed on one line.
8. **Label before number.** In a highlight strip or KPI, the label is text above the number, never a hover title. A number whose unit exists only in a tooltip is a guess, and hover does not exist on touch.

### Density and action

9. **Single fact.** A fact appears in one column only. A "why now" column that repeats the due date, category and days already shown in the value column informs nothing.
10. **Two buttons.** At most two buttons and one link per queue item. The third button adds reading, not power. Rejecting must cost less than accepting.
11. **One question per control.** A control answers one question. Mixing "when" and "why" in one select forces users to compare options that cannot be compared.
12. **Constant column is noise.** A column where every row says the same thing is removed. If it is constant by construction, it belongs in the section title.

### Structure and behavior

13. **Stacked bars.** Header, context and sub-navigation are fixed and in that order. The scroll offset lives on the target only, never added in two places. Adding it twice parks every section below where it should be and nobody can say why.
14. **Three ways to close.** Every dialog closes by the button, by Escape and by clicking outside.

## Vocabulary

The text ruler matters as much as the pixel ruler, and it is where most mistakes happen.

- **The button verb says what will happen in the system.** `Create task`, `Update stage`, `Save to CRM`. Never `Done`, `Send`, `OK`: a generic verb makes users guess the effect and learn it by being wrong.
- **Labels use the destination system's word, not yours.** One word from draft to record.
- **When users ask twice what a button does, the label is the defect.** Do not explain better, rename.
- **Missing capture never becomes zero.** With no sensor, write `no data`. Zero is a claim about the world. Absence is a claim about the instrument.
- **A limitation sits next to the decision it affects,** not in a footer.

## Anti-patterns

| Symptom | What is happening | Fix |
|---|---|---|
| "Looks made by AI" | Thick colored card border, gradient with no function, decorative icons, shadow on everything | Flat surface at rest, shadow only where a layer floats |
| "Too wordy" | Every concept became a chip or a line | Move it to layer 2 (a popover), do not delete the concept |
| "I do not know what this button does" | Generic verb | The verb is the effect on the system |
| A signal that fires on most of the base | The cut is too wide and became an alarm | Replace with the hard, actionable fact |
| Two numbers that do not add up | The same item counted in two strips | One item, one count, each strip points to its own block |

## Check by measuring

Alignment and clipping cannot be judged by eye. Two measurements settle most cases:

```js
// text alignment (padding does not move the box)
const start = el.getBoundingClientRect().left
            + parseFloat(cs.paddingLeft) + parseFloat(cs.borderLeftWidth);

// a control clipped by a scrolling container
rect.right > containerRect.right + 1
```

Run audits at more than one viewport width. A clipping bug that only exists at one width is invisible if you test at another.

## Checklist

- [ ] One accent, with any exception written into the rule.
- [ ] Contrast measured on every accent fill and every gray.
- [ ] No size outside the type scale.
- [ ] Every KPI is label, number, reading.
- [ ] No fact appears twice in a row.
- [ ] No queue item has more than two buttons and one link.
- [ ] No column is constant.
- [ ] No navigation is disabled.
- [ ] Every dialog closes three ways.
- [ ] Every button verb names its effect.

## Good and bad

Good: a KPI block with label `OPEN PIPELINE`, number `$870K`, reading `6 deals, synced 07:05`.

Bad: `6 deals · $870K · 2 stale · 1 late` on one line. Four numbers compressed is zero numbers read.

## Definition of done

- Each rule is checked and any violation has a recorded decision.
- Contrast and alignment were measured, not eyeballed.
- A new user can say what every button does without asking.
- The screen was checked at a narrow and a wide viewport.

## What this skill does not cover

Agent presence, autonomy regimes, draft-to-approval and the activity trail belong to `agentic-ux`. Visual craft for persuasion pages belongs to a design polish pass. This skill is the ruler for density, legibility and action on a working screen.
