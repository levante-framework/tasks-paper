# Open queries for colleagues

Questions that can't be settled from the data or the repo, blocking or
qualifying specific statements in `ms.qmd`. Each gives the claim as it
currently stands and what needs confirming.

## 1. Task runtime behaviour: timing and encouragement prompts

These two were raised separately by the reviewer but are the same question —
what the task software actually does while a child is working. Findings below
come from a targeted search of `../core-tasks/task-launcher/src/tasks` on
2026-10-23; it is not an exhaustive audit, and the deployed build may differ
from this checkout. All of it needs confirming by someone who knows the
implementation.

### 1a. Are the fixed-form tasks really untimed?

Design principle 4 currently reads:

> **Accuracy over speed.** Because administration platforms vary, tasks impose
> no time limits and are mostly untimed, though reaction time is recorded
> throughout.

FO is not confident this is accurate. Three things bear on it:

- **The CAT versions do impose limits** — session caps of 4 minutes (Sentence
  Understanding, Shape Rotation, Vocabulary), 6 minutes (Pattern Matching,
  Same & Different) and 8 minutes (Math), per the Adaptive Task Construction
  section. A session cap is not a per-item limit, but slower children answer
  fewer items, so "accuracy over speed" is qualified rather than preserved.
- **Hearts and Flowers instructs children to hurry.** The instruction strings
  are "This time the game will go faster. It won't tell you if you are right or
  wrong." and "Try to answer as fast as you can without making mistakes."
  That is a speeded task by design, which sits awkwardly under this principle.
- **But no enforced per-trial timeout was found.** In
  `hearts-and-flowers/trials/stimulus.js` the `response_ends_trial: true` line
  is commented out, and `practice.js` carries a TODO to "handle stimulus
  presentation timeout and other parameters". So H&F appears to be
  speed-*emphasised* in its instructions but not hard-timed in code.

**Need to know:** which tasks impose limits of any kind, in which
administration mode; whether H&F enforces a response window in the deployed
build; and whether the CAT session limits were a considered trade-off against
this principle or simply a requirement of group testing. The wording of
principle 4 and of the CAT paragraph both depend on the answer.

### 1b. The encouragement phrases

The manuscript's translation workflow refers to "encouragement phrases that
appear throughout the core tasks". The reviewer asked for examples. Searching
for them raised a prior question about whether that description is accurate:

- Only **one** encouragement string appears to be active: `generalEncourage`,
  rendered as a persistent on-screen toast during the response phase of the
  Memory Game (`memory-game/trials/stimulus.ts:218`).
- Hearts and Flowers has two written — "Try to keep up!"
  (`heartsAndFlowersEncourage1`) and "If you make a mistake, just keep going!"
  (`heartsAndFlowersEncourage2`) — but **both are commented out** in
  `hearts-and-flowers/trials/instructions.js`.
- No other task in the repo references an encouragement string.

If that is right, "appear throughout the core tasks" overstates it, and the
translation-workflow paragraph needs correcting rather than merely
illustrating.

**Need to know:** whether encouragement phrases are in fact used beyond the
Memory Game toast; whether the H&F ones were disabled deliberately; and two or
three representative phrases to quote.

## 2. How were the nine core tasks selected?

The manuscript does not describe the selection process (reviewer, tranche 1,
L50). **Need to know:** what the criteria were and who decided.

## 3. Were there per-task length targets?

Related to query 1a.

The one-hour total for the battery is confirmed and is design principle 10.
**Need to know:** whether individual tasks also had target durations during
development, or whether only the total was budgeted.

## 4. Should the Scoring section document the guessing floor?

The IRT models carry a fixed lower asymptote (0.5 for two-option items, 0.25
for four-option, mixed in Math, Same & Different and Stories), and the
difficulty definition in the main text now refers to it. But the Scoring
section never mentions it, and describes the models as "Rasch" (4 mentions).
**Need to know:** whether "Rasch" is the label we want given the fixed
guessing parameter, and whether Scoring should state the floor explicitly.
