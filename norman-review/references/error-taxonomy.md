# The error taxonomy, and the remedy each class demands

The reason to classify an error is that the classes need *different fixes*. Teams that
skip classification apply the same two remedies to everything — add a confirmation
dialog, add a tooltip — and both fail on most of the taxonomy.

Norman's frame: error is a mismatch between human capability and what the technology
demands. Treat it as a design deficit, not a moral failing. The useful question is
never "why did they do that" but "what made that the natural thing to do".

## The split

**Slip** — you intended the right thing and did something else. The plan was correct;
execution went astray. Slips come from the *lower* stages of the action cycle
(specify, perform, perceive, interpret) and are largely subconscious.

**Mistake** — you did exactly what you intended, and the intention was wrong. The plan
itself was bad. Mistakes come from the *upper* stages (goal, plan, compare) and are the
product of conscious deliberation.

The counterintuitive consequence: **experts make more slips, novices make more
mistakes.** Expertise moves behavior into the subconscious, which is fast, accurate,
and occasionally derailed. So a defect that mostly bites your power users is probably
a slip-inducer, and a defect that mostly bites new users is probably a model problem.

Diagnostic question for any reported error: *did the user get what they asked for?*
If yes but it was wrong — mistake. If they asked for something they didn't want to ask
for — slip.

## Slips: two subtypes

### Action-based slip

The right action performed on the wrong object, or the wrong action performed instead
of a similar one. Norman's own: pouring milk in the coffee and putting the coffee in
the fridge.

**Sub-causes worth naming separately:**

- **Description-similarity slip.** Two targets are described similarly enough in the
  mind that the wrong one gets selected. In software: identical rows, near-identical
  buttons, two files one character apart, adjacent items in a list where only a small
  label differs, `staging-db` and `staging-db-2`.
  *Fix:* increase distinctness (color, shape, icon, position), increase distance, or
  add a disambiguating detail to the label. Not "be careful".

- **Capture slip.** A frequently-performed sequence captures a similar but less common
  one — you start down the rare path and your hands finish the familiar one.
  *Fix:* make the initial steps of the two procedures as dissimilar as possible.
  Sharing the first three clicks between "duplicate" and "delete" is a capture-slip
  factory.

- **Mode error.** A correct action in the wrong context. Only exists because a mode
  exists.
  *Fix:* eliminate the mode; failing that, make it continuously and unmissably
  visible; failing that, make the action label carry the mode ("Deploy to
  **production**").

**General slip remedies, in order of power:**

1. Separate confusable things — visually, spatially, or by naming.
2. Make the object of the action prominent at the moment of acting.
3. Perceptible feedback on the action *and* on the resulting state.
4. Undo.

### Memory-lapse slip

You forgot a step. The intention was right, the plan was right, and one component
never happened. Leaving the original in the copier. Leaving the gas on.

In software: the file uploaded but never attached, the change made but never saved,
the filter left applied, the VPN left on, the feature flag left enabled, the branch
left unmerged, the temporary permission never revoked.

*Fix:* don't rely on memory — put the knowledge in the world.
- Show incomplete state persistently rather than expecting recall.
- Close the loop automatically where you can (auto-save, auto-attach, auto-expire).
- Make the unfinished thing visible at the point of exit, not buried in a settings page.
- Forcing functions where the cost of forgetting is high: the interlock that won't let
  the flow complete with a step outstanding.

The biggest driver is **interruption**, and the expensive part is resumption. Systems
that discard partial state on navigation manufacture memory-lapse slips at scale.

## Mistakes: three subtypes

### Rule-based mistake

The situation was diagnosed correctly, and the wrong rule was applied. The user knew
what state they were in and chose a bad response — often because the interface offered
a plausible-looking wrong option, or because the right rule was less discoverable than
the wrong one.

In software: using "reset" when they wanted "revert"; force-pushing to fix a conflict;
picking the export format that silently drops data; choosing the plan that doesn't
include the thing they came for.

*Fix:* this is an information and framing problem.
- Present the consequences at the point of choice, not in docs.
- Order and weight options by what's usually correct; make the dangerous one look
  dangerous.
- Name options by outcome, not mechanism ("Keep my local version" beats "ours").
- Provide a sensibility check when the chosen rule contradicts the evident situation.

### Knowledge-based mistake

The situation was *misdiagnosed*, because the user's knowledge was wrong or
incomplete. They were operating a model that doesn't match reality. Norman's example:
fuel computed in pounds instead of kilograms.

In software: believing the change was saved; believing "archive" is reversible and
"delete" isn't when it's the other way round; believing two settings are independent;
believing the number shown is live.

*Fix:* this is the conceptual-model problem, and it is the deepest class.
- Correct the model transmitted by the system image.
- Make the true state visible and continuously observable.
- Show units, scale, and provenance next to numbers that could be misread.
- Sensibility checks catch what the model missed: flag the value that is a thousand
  times the norm rather than executing it.

Note: **confirmation dialogs are useless against knowledge-based mistakes.** The user
is confident and will confirm. The only defenses are a better model, better visible
state, and reversibility.

### Memory-lapse mistake

Forgetting at the level of goals, plans, or evaluation — losing track of what you were
trying to achieve, or forgetting to check whether it worked. Distinct from a
memory-lapse slip, which forgets a step within a correct plan.

In software: abandoning a half-migrated state and forgetting the goal was migration;
running a fix, not verifying it, and moving on; a multi-day task where the original
objective drifts.

*Fix:* keep the goal in the world.
- Show progress against the stated objective, not just against the current step.
- Explicit completion criteria and a visible checklist for long or multi-session work.
- Make verification part of the flow rather than a thing the user must remember to do.

## Cross-cutting remedies

Ranked by how much error they actually remove.

1. **Make the wrong action impossible.** A constraint that eliminates the error class
   beats any amount of warning about it.
2. **Make actions reversible.** Undo is the highest-leverage single feature for error
   tolerance. It converts most slips into non-events.
3. **Sensibility checks.** Software can ask whether a request is plausible. Meek
   compliance with an absurd input is a design decision.
4. **Perceptible feedback on action and resulting state.** Errors you can see are
   errors you can fix.
5. **Separation and distinctness.** Cheap, structural, and removes whole slip families.
6. **Support interruption and resumption.** Removes the largest external error driver.
7. **Help them finish rather than rejecting them.** Treat input as an approximation of
   intent — parse the messy date, accept the spaced phone number, offer the correction
   inline.
8. **Confirmation dialogs.** Last, and only for slips, and only when the object is
   made prominent and the buttons name the action. Against mistakes they are theater.

## The Swiss cheese frame

Accidents don't have *a* cause. They happen when holes in several independent layers
line up. Two consequences for review work:

- **Stop hunting for the single root cause of a usability failure.** "The user clicked
  the wrong button" is one hole. So is "the buttons were adjacent", "there was no
  undo", "the confirmation was generic", and "the state wasn't visible". Report all of
  them; fixing one closes one hole.
- **Recommend layers, not a fix.** Redundancy and defense in depth: prevent, then
  detect, then recover. A design with only prevention fails hard the first time
  prevention fails.

Three moves the metaphor gives you: add a layer, shrink the holes in existing layers,
and alert when several holes line up.

## Five Whys, with a caveat

Keep asking why past the first plausible answer — the first answer is almost always
"the user did X", which is where analysis stops being useful. Push until you reach
something in the design that could be changed.

The caveat Norman himself adds: *why* is not guaranteed to converge, and it tends to
follow a single chain when accidents branch. Use it to get past the blame reflex, not
as a proof procedure. When you reach a design decision that could have gone the other
way, you've gone far enough.
