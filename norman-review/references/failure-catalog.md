# The failure catalogue

Named patterns, their Norman diagnosis, and the standard fix. Scan this during a
review to catch structural defects that a sequential walkthrough misses. Each entry is
written so you can lift the diagnosis language straight into a finding.

## Contents

1. [The Norman door](#1-the-norman-door)
2. [The taped-on sign](#2-the-taped-on-sign)
3. [Beautiful and inoperable](#3-beautiful-and-inoperable)
4. [The false conceptual model](#4-the-false-conceptual-model)
5. [The hidden gesture](#5-the-hidden-gesture)
6. [The silent action](#6-the-silent-action)
7. [Confirmation theater](#7-confirmation-theater)
8. [Alarm fatigue](#8-alarm-fatigue)
9. [The uninterpretable signal](#9-the-uninterpretable-signal)
10. [The invisible mode](#10-the-invisible-mode)
11. [The unmapped control](#11-the-unmapped-control)
12. [Description-similarity trap](#12-description-similarity-trap)
13. [The disabled control that won't say why](#13-the-disabled-control-that-wont-say-why)
14. [The memory test](#14-the-memory-test)
15. [Featuritis](#15-featuritis)
16. [The lost accidental signifier](#16-the-lost-accidental-signifier)
17. [The one-setting user](#17-the-one-setting-user)
18. [The forcing function people defeat](#18-the-forcing-function-people-defeat)
19. [Device-centered instead of activity-centered](#19-device-centered-instead-of-activity-centered)
20. [The unrecoverable interruption](#20-the-unrecoverable-interruption)

---

## 1. The Norman door

**Looks like:** A control that invites the wrong action. A push-plate on a pull door.
In software: a card that looks clickable and isn't; text styled like a link that's
inert; something inert styled like a button; a row where clicking the row does one
thing and clicking anywhere inside it does another.

**Diagnosis:** The perceived affordance contradicts the actual one. The user acts
correctly on the information given and fails anyway.

**Fix:** Make the appearance match the behavior. Anything that looks pressable must be
pressable; anything pressable must look it. Where two behaviors coexist in one region,
give each a visually distinct target.

---

## 2. The taped-on sign

**Looks like:** Helper text, a tooltip, an asterisk, an onboarding coach-mark, a pinned
Slack message, or a README line explaining how a control works.

**Diagnosis:** When you have to add an external signifier to something this simple, the
design has failed. The sign is evidence, not a solution.

**Fix:** Change the control so the explanation is unnecessary — a clearer label, a
different control type, a better default, a visible constraint. Keep the helper text
only for genuinely domain-specific information the interface can't carry.

**How to spot it fast:** ask what the team says out loud when demoing. Every sentence
of demo narration is a signifier missing from the product.

---

## 3. Beautiful and inoperable

**Looks like:** Minimal, flat, borderless, unlabelled. Uniform grey text where some of
it is interactive. Whitespace where the boundaries used to be. Norman's version was an
all-glass door with no visible hinge or hardware.

**Diagnosis:** Aesthetics were optimized at the expense of communication. It is not
that beauty is the problem — it is that the signifiers were treated as decoration and
removed.

**Fix:** Reintroduce the minimum signifier that restores discoverability. Norman's
own remedy for the glass door was a vertical plate on the push side — a small,
unobtrusive element that carries the whole message. Look for the equivalent: a subtle
border, a weight change, a caret, a hover-independent affordance cue. The goal is both,
not a trade.

---

## 4. The false conceptual model

**Looks like:** Two controls labelled as independent that share one mechanism. A
"delete" that archives. A flat list over a hidden hierarchy. A toggle whose effect
depends on a state shown elsewhere. A percentage that isn't of what you'd assume.

**Diagnosis:** The interface promises a structure the system does not have. This is
the most severe class of defect, because care doesn't help — a diligent user forms a
confident wrong model and acts on it.

**Symptoms in the field:** users running controlled experiments; support tickets
saying "I changed X and Y also changed"; folk theories in the community forum;
documentation that says "allow 24 hours" instead of explaining.

**Fix, in order of preference:** make the promise true in the implementation; or change
the promise to reflect the real mechanism; or expose the coupling live in the UI so
the dependency is learnable. Never leave the false model and explain it in docs.

---

## 5. The hidden gesture

**Looks like:** Swipe, long-press, right-click, drag, hover-reveal, keyboard shortcut,
or an edge gesture as the only route to a capability.

**Diagnosis:** An affordance with no signifier. The action was always possible; nothing
communicates that it is. Norman's own example was exactly this — an app where users
found the left/right swipes and never found up/down.

**Fix:** Add a perceivable indicator. The strongest is a partial reveal (the peeking
card, the clipped list). Others: a visible handle, a caret, an explicit button that
does the same thing, a first-run hint that leaves a permanent trace behind. Gestures
are fine as accelerators; they are never acceptable as the sole path.

---

## 6. The silent action

**Looks like:** A button that produces no visible change. A form that submits into
nothing. A setting saved without acknowledgement. An operation that succeeds
invisibly because "it just worked".

**Diagnosis:** Missing feedback — question 5 unanswered. Indistinguishable from a
broken control, and the user's next move is to press it again. This is how duplicate
records get created.

**Fix:** Acknowledge every action, on the element acted upon, within 100 ms — even if
the result isn't known yet. Then confirm the outcome when it arrives.

---

## 7. Confirmation theater

**Looks like:** "Are you sure?" with OK / Cancel, guarding something irreversible.

**Diagnosis:** Mistimed and uninformative. By the time it appears the user has already
decided, so they confirm reflexively; and it names the verb, not the object, while
the user's attention is on the verb they just chose. It stops slips occasionally and
stops mistakes never.

**Fix:** Prefer undo — soft delete, trash, a real undo window. Where confirmation must
exist, make the *object* prominent (name it, show it, preview it), label buttons with
the actual action and count ("Delete 3 invoices"), and offer the safe resolution
inside the dialog. For genuinely irreversible high-blast-radius operations, require
typing the target's name.

---

## 8. Alarm fatigue

**Looks like:** Every notification styled as important. Constant toasts. A dashboard
where several things are always red. Badge counts that never reach zero.

**Diagnosis:** Feedback that isn't prioritized. When everything demands attention,
people disable all of it — and then miss the one that mattered. Norman's control room
where operators spend the emergency silencing alarms.

**Fix:** Tier the signals. Reserve interruptive treatment for what is both urgent and
actionable. Make routine confirmations unobtrusive and self-dismissing. Anything that
is always on is not a signal — demote it to ambient state.

---

## 9. The uninterpretable signal

**Looks like:** A red dot. A toast that says "Error". A beep. A status light with four
meanings. An exit code with no message.

**Diagnosis:** Feedback that reports *that* something happened without reporting
*what* — the cheapest possible signal, carrying almost no information. Worse than
silence, because it creates anxiety without enabling action.

**Fix:** Say what happened, to what, and what to do about it. If several conditions
share one indicator, split them or make the indicator carry the distinction.

---

## 10. The invisible mode

**Looks like:** Edit vs view. Staging vs production. A selected tenant, branch,
environment, or account shown only in a small corner label. Offline. Recording.

**Diagnosis:** Mode error waiting to happen — correct action, wrong context. Norman's
strongest recommendation is simply to eliminate modes; where they must exist, they
must be conspicuous.

**Fix:** Remove the mode if the action can be made explicit per-invocation. Otherwise
make it change the whole visual field — chrome color, background, prompt — not a
badge. And put the mode in the action label itself.

---

## 11. The unmapped control

**Looks like:** A settings page whose entries don't correspond to anything visible. An
"edit" button far from what it edits. Controls in a different order than the things
they affect. Two switches for two panels, wired the wrong way round.

**Diagnosis:** Broken mapping — the relationship between control and effect must be
learned and re-learned because nothing in the layout carries it.

**Fix:** Spatial correspondence, proximity, and grouping. Move the control next to what
it changes; arrange controls to mirror their targets; add a live preview where no
spatial analogy exists.

---

## 12. Description-similarity trap

**Looks like:** Rows that differ by one character. Two buttons with near-identical
labels. Adjacent destructive and routine actions. `prod-db-1` next to `prod-db-2`.
Repeated identical controls down a list.

**Diagnosis:** Manufactures action slips. Users select the wrong target not from
carelessness but because the mental description of both is identical.

**Fix:** Increase distinctness — color, icon, shape, a differentiating detail in the
label. Increase distance between confusable controls. Never place a destructive action
adjacent to a frequent routine one.

---

## 13. The disabled control that won't say why

**Looks like:** A greyed-out button with no explanation of what would enable it.

**Diagnosis:** An anti-affordance with no signifier — the software equivalent of clean
glass. The user can perceive that they can't, but not why, so they can't act on it.

**Fix:** State the unmet condition next to the control, or link to it. Better: leave
the control enabled and, on activation, explain and offer to resolve the blocker.

---

## 14. The memory test

**Looks like:** Placeholder text used as the only label, vanishing on focus. A wizard
that doesn't show earlier answers. A confirmation page that doesn't show what you're
confirming. A code sent in one place that must be typed in another with no way back.

**Diagnosis:** Knowledge that should be in the world has been pushed into the user's
head — and specifically into short-term memory, which is small, fragile, and destroyed
by interruption.

**Fix:** Put it back in the world. Persistent labels. Visible summary of prior steps.
Show what's being acted upon at the moment of acting. Allow going back without loss.

---

## 15. Featuritis

**Looks like:** Every release adds; nothing is removed. Controls competing for space.
Menus nested to hide the count. Settings pages that need their own search. Each
feature has an internal advocate; none has an opponent.

**Diagnosis:** More functions in the same space means less room per signifier and more
to understand — the paradox of technology. The disease is structural, driven by the
need for each release to differentiate.

**Fix:** Defend the removal. Design for the activity rather than accumulating tasks —
the coherent thing people are trying to do bounds the feature set. Where capability
must exist but isn't common, layer it: progressive disclosure with a discoverable
entry point, not a flat surface where everything competes equally.

---

## 16. The lost accidental signifier

**Looks like:** A digital replacement for a physical thing that no longer tells you
what the physical thing told you for free. Norman's example: a paper book shows how
much is left just by being held; an e-reader shows nothing unless someone builds it.

**Diagnosis:** Physical artifacts carry unintended information. Digitizing deletes it
silently, and nobody notices because nobody ever designed it.

**Fix:** When replacing anything analog or physical, enumerate what the original
communicated incidentally — quantity, progress, position, wear, recency, presence of
others — and re-supply it deliberately. This is also where scroll position, item
counts, "last updated", and presence indicators come from.

---

## 17. The one-setting user

**Looks like:** Everyone at the company uses the same three of your forty features.
Users have a memorized incantation they don't understand and won't deviate from.
Copy-pasted config nobody can explain.

**Diagnosis:** Rote operation — the signature of a missing conceptual model. Faced
with an incomprehensible array of controls, people memorize one path that works and
ignore everything else. Norman's washing machine, and his own watch with five
unlabelled buttons.

**Fix:** This is a model problem, not a discoverability problem. Establish what the
thing *is* before adding more ways to operate it. Then the existing controls become
predictable rather than needing individual memorization.

---

## 18. The forcing function people defeat

**Looks like:** A confirmation everyone clicks through. A required field filled with
"n/a". A guard aliased away. A check disabled in CI "temporarily". Notifications
globally muted.

**Diagnosis:** The safety mechanism's cost on the normal path exceeds its perceived
value, so people route around it — and the protection is then absent when it was
actually needed.

**Fix:** Reduce the tax rather than adding enforcement. Move the guard to the rare
dangerous path only. Make the barrier carry the resolution so it becomes useful rather
than obstructive. Norman's stairwell gate: enough friction to break momentum, not
enough that anyone props it open.

---

## 19. Device-centered instead of activity-centered

**Looks like:** Navigation that mirrors your architecture — a page per service, a tab
per table, settings grouped by subsystem. Doing one real-world thing requires visiting
four screens and carrying state between them in your head.

**Diagnosis:** The structure reflects the implementation rather than the activity.
Norman's auditorium controls: separate screens for lights, sound, and projection, so
the speaker interrupts their own talk to move between them.

**Fix:** Group by activity. Put everything needed for one coherent goal in one place,
even when it spans subsystems. And keep manual overrides available without dropping
the user out of the activity — his own complaint about the activity-based controller
was that raising the lights cancelled the whole presentation mode.

---

## 20. The unrecoverable interruption

**Looks like:** Navigating away loses the draft. Reload resets filters, scroll, and
expansion. A multi-step flow restarts from step one. A dismissed dialog is gone
forever. Session timeout discards the form.

**Diagnosis:** Interruption is a leading cause of error, and the expensive part is
resumption. The system discards exactly the state needed to resume, converting a
30-second interruption into lost work.

**Fix:** Persist continuously rather than on submit. Preserve scroll, filters,
selection, and expansion across navigation and reload. On re-entry to a flow, say
where they were and what remains. Make dismissed things retrievable.
