---
name: dense-operations-ui
description: Fourteen named design rules for a dense operator screen that a professional opens every day, under time pressure, to decide, plus a button vocabulary and a measured audit. Use it to design, critique or rewrite any operator panel, decision queue, agent console or back-office tool, including when a screen "looks like AI slop" or "is too crowded".
license: MIT
metadata:
  version: "1.1"
  author: Benjamin Tayar
  method: agent-design-principles
---

# Dense operations UI

> A working screen is a decision tool: dense, legible, quiet, and measured rather than eyeballed.

An operations screen fails in two opposite ways. It is flat and generic, so it looks like an admin template and the user cannot remember where they were. Or it is baroque: every concept became a chip, every rule became a line of text, and the user stops reading. These rules avoid both at once. Each one is named because a name can be cited in review.

The audience is always the same: someone who opens this every day, in a hurry, to decide. Not the buyer, not an executive in a presentation, not the designer admiring the work.

Reasoning and anti-pattern per rule are in [reference/rules.md](reference/rules.md).

## When to use

| Mode | Use it when | Output |
|---|---|---|
| `rules` | Before designing or reviewing | The rules with their numbers, ready to paste into a brief |
| `critique <target>` | After rendering | Violation by violation: the rule broken, the evidence, the fix |
| `apply <target>` | When rewriting the generator or stylesheet | The proposed diff, rule by rule |

Recommended order: `agentic-ux` (what the screen must say about the agent), then this skill (how to say it without clutter), then a visual polish pass. Not for persuasion pages or marketing sites.

## Inputs

1. The primary user and the decision the screen serves.
2. The rendered screen at a narrow and a wide viewport, or the generator and stylesheet for `apply`.
3. The design tokens in use: the accent, the ink, the type scale.
4. The vocabulary of the destination system (object and field names), for the button verbs.

## Procedure

1. Name the primary user and the decision in one sentence.
2. Sweep the fourteen rules in order: color and brand, type and number, density and action, structure and behavior. Record each violation as rule number, evidence, fix.
3. Measure what cannot be judged by eye: contrast on every accent fill and every gray, text alignment, clipping inside scrolling containers. Measure at both viewport widths.
4. Read every button label against the vocabulary rules.
5. `rules`: paste the numbered rules into the brief. `critique`: report violation by violation. `apply`: propose the diff rule by rule, smallest change first.
6. Record any exception as a rewrite of the rule, never as a silent break.

## Rules

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

### Vocabulary

The text ruler matters as much as the pixel ruler, and it is where most mistakes happen.

- **The button verb says what will happen in the system.** `Create task`, `Update stage`, `Save to CRM`. Never `Done`, `Send`, `OK`: a generic verb makes users guess the effect and learn it by being wrong.
- **Labels use the destination system's word, not yours.** One word from draft to record.
- **When users ask twice what a button does, the label is the defect.** Do not explain better, rename.
- **Missing capture never becomes zero.** With no sensor, write `no data`. Zero is a claim about the world. Absence is a claim about the instrument.
- **A limitation sits next to the decision it affects,** not in a footer.

### Anti-patterns

| Symptom | What is happening | Fix |
|---|---|---|
| "Looks made by AI" | Thick colored card border, gradient with no function, decorative icons, shadow on everything | Flat surface at rest, shadow only where a layer floats |
| "Too wordy" | Every concept became a chip or a line | Move it to layer 2 (a popover), do not delete the concept |
| "I do not know what this button does" | Generic verb | The verb is the effect on the system |
| A signal that fires on most of the base | The cut is too wide and became an alarm | Replace with the hard, actionable fact |
| Two numbers that do not add up | The same item counted in two strips | One item, one count, each strip points to its own block |

### Check by measuring

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

## Example

Acme Field Sales Assistant, the pipeline header and the open-questions table.

Weak:

```
Pipeline: 6 deals · $870K · 2 stale · 1 late                 [Done]  [OK]

Open questions
question                      account        owner      status
Confirm the renewal date      Northgate      Dana       to ask
Who signs the order?          Harbor Foods   Luis       to ask
```

Four numbers compressed on one line is zero numbers read. The verbs say nothing about the effect. The status column says the same thing on every row, by construction.

Corrected:

```
OPEN PIPELINE              STALE DEALS               LATE TASKS
$870K                      2                         1
6 deals · synced 07:05     no activity for 14+ days  due before today

[Save to CRM]   Open in CRM

Open questions (all still to ask)
question                      account        owner
Confirm the renewal date      Northgate      Dana
Who signs the order?          Harbor Foods   Luis
```

Label above the number, the reading below it, the verb names the effect in the system, and the constant moved into the section title.

## Definition of done

- Each rule is checked and any violation has a recorded decision.
- Contrast and alignment were measured, not eyeballed.
- A new user can say what every button does without asking.
- The screen was checked at a narrow and a wide viewport.

## Related

- [`agentic-ux`](../agentic-ux/SKILL.md): agent presence, autonomy regimes, draft-to-approval and the activity trail. This skill is the ruler for density, legibility and action once those are settled.
- [`write-contract`](../write-contract/SKILL.md): the button verb names the effect in the system of record; the vocabulary comes from there.
- [`autonomy-tiers-and-evals`](../autonomy-tiers-and-evals/SKILL.md): the tier of each action type is published where users can see it, which is a density decision too.
- [reference/rules.md](reference/rules.md): reasoning and anti-pattern per rule, two CSS traps, the measurement snippets.
- [Screen audit template](../../templates/screen-audit.md).
