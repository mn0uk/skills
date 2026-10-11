---
name: norman-design
description: Design and build interfaces the way Don Norman would — discoverability, affordances vs signifiers, natural mapping, feedback, conceptual models, constraints, and forcing functions. Use this whenever you create or change anything a human will operate, including screens, components, forms, settings, dashboards, onboarding, navigation, modals, empty states, error states, CLI flags and output, config file shapes, and API ergonomics. Trigger it even when the request sounds trivial — "add a button", "build a page", "make a form", "improve the UX", "design a flow" — especially then, because the defaults you would otherwise reach for are exactly the ones Norman spent a book diagnosing. For auditing an interface that already exists, use norman-review. For deciding what to build in the first place, use norman-discover.
---

# Designing everyday things

## The one idea everything else hangs from

The person using your interface cannot see your intentions, your data model, or your
code. All they get is what you put in front of them. Norman calls that the **system
image** — the visible structure, the labels, the states, the docs, everything
perceivable. The designer's mental model and the user's mental model never touch
directly. The entire burden of communication rests on the system image.

So when someone can't work out how to use a thing, that is not a training problem, a
documentation problem, or a user problem. It is a signal that the system image failed
to carry the model. Norman's discipline is to keep re-asking: *what does this artifact
communicate on its own, to someone who has never seen it and won't read anything first?*

Two qualities matter above all:

- **Discoverability** — can a newcomer work out what actions are possible, and where
  and how to perform them?
- **Understanding** — can they work out what it all means, what the controls do, and
  what state the system is in?

## Start with the conceptual model, not the layout

Before writing any markup, component, or flag, state the conceptual model in one or
two plain sentences: *what the user should believe is happening.* It doesn't have to
be technically accurate — folders don't exist inside a computer either — it only has
to be simple, predictive, and true at the edges.

Then check the model against the implementation. Norman's refrigerator has two dials
labelled "freezer" and "fridge" but only one thermostat: the labels promise
independent control the machine cannot deliver, so no amount of care lets the user
succeed. **A false conceptual model is unrecoverable by the user.** If your UI implies
two independent things and the backend has one, either fix the backend so the promise
becomes true, or change the UI to stop making the promise.

A simplified model earns its keep only while its assumptions hold. Design the moment
it breaks, not just the happy path — the offline document that still looks local, the
optimistic update that silently failed, the cached number that is now stale. If the
model has no explanation for that state, the user is stranded.

## The six levers

These are the tools you actually reach for. Keep the vocabulary precise; the precision
is what makes them usable rather than decorative.

**Affordances** are relationships, not properties — what *this* agent can do with
*this* object, given both. A screen affords touching everywhere. A form field affords
typing. Affordances exist whether or not anyone can perceive them, which is exactly
why they are a weak design lever on their own. An **anti-affordance** prevents an
interaction (a disabled control, a permission wall); it also has to be perceivable, or
it just reads as broken.

**Signifiers** are the perceivable indicators that communicate where and how to act.
The arrow, the label, the caret, the underline, the peeking edge of the next card, the
cursor change, the placeholder. Norman coined the term after watching designers say
"I put an affordance there" about a circle on a touchscreen — the touch was already
afforded everywhere; the circle *signified where*.

**In design, signifiers do the work.** Affordances determine what is possible;
signifiers communicate what to do. Focus your effort on signifiers. A hidden gesture,
an unlabelled icon, a menu that only appears on hover, a keyboard shortcut with no
on-screen trace — these are affordances without signifiers, and they are invisible to
everyone who did not build them.

Digitizing something silently deletes its *accidental* signifiers. A physical book
tells you how much is left just by being held; an e-reader tells you nothing unless
someone deliberately builds it. Whenever you replace a physical or analog thing, list
what the original told the user for free, and re-supply it on purpose.

**Mapping** is the relationship between controls and their effects. Natural mapping
uses spatial or perceptual analogy so the relationship needs no learning: to move
something up, the control moves up; vertical position means amount; controls sit in
the same arrangement as the things they control. Group related controls; put a control
next to the thing it changes. Mercedes' seat control is shaped like the seat — the
gold standard. Watch out for mappings that feel natural but are only cultural; check
before assuming.

**Feedback** communicates the result of an action. See the discipline section below —
it has more failure modes than any other lever.

**Conceptual model** — covered above. It is the one that produces understanding rather
than mere operability.

**Constraints** limit what can be done, so the right action is often the only action.
Four kinds, all available in software:

| Kind | What it leans on | In an interface |
|---|---|---|
| Physical | Shape and size make wrong actions impossible | Input masks, steppers instead of free text, drop targets that only accept valid items, ranges enforced by a slider |
| Cultural | Learned conventions of a group | Red for destructive, ✕ closes, left-to-right progress, a shopping cart icon, platform conventions |
| Semantic | What the situation means | Only one sensible arrangement given the purpose — an end date cannot precede a start date; a "reply" belongs under the thing replied to |
| Logical | Elimination and spatial reasoning | One slot left so one item must go there; two switches for two panels, left controls left |

Physical constraints are strongest when visible *before* the user acts. A constraint
that only reveals itself by rejecting the submitted form is a validation error wearing
a constraint's clothes.

## Answer the seven questions

Norman's action cycle gives a checklist that catches most interface failures. Walk any
screen or flow you design and confirm the interface itself answers all seven — not
your docs, not your onboarding tooltip, the interface.

Bridging the **Gulf of Execution** — the effort to work out how to act. Answer these
with signifiers, constraints, mappings, and the conceptual model (Norman calls this
information *feedforward*):

1. What do I want to accomplish?
2. What are the alternatives?
3. What can I do right now?
4. How do I do it?

Bridging the **Gulf of Evaluation** — the effort to work out what happened. Answer
these with feedback and the conceptual model:

5. What happened?
6. What does it mean?
7. Is this okay — did I get what I wanted?

When a design feels wrong but you can't name why, find which of the seven has no
answer. That is the defect, and it usually names its own fix: a missing answer to #3
is a missing signifier; a missing answer to #5 is missing feedback; a missing answer to
#6 is a missing or false conceptual model.

## Feedback discipline

Feedback is the lever most often done badly, and bad feedback is worse than none —
it distracts, misleads, and trains people to ignore your signals.

- **Immediate.** Even a tenth of a second of unexplained silence reads as
  unresponsive. Acknowledge the action before you have the result: pressed state,
  spinner, optimistic row, "sending…".
- **Informative.** "Something happened" is not feedback. Say *what* changed, to
  *what*, and *what to do next* if anything. A beep, a toast that says "Error", a red
  border with no message — all of these tell the user only that they have a problem.
- **Attributable.** The user must be able to tell which thing is speaking. Five
  components that all emit an identical grey toast produce a UI where nothing is
  traceable.
- **Prioritized.** Routine confirmations should be unobtrusive; genuinely important
  signals should be hard to miss. If everything shouts, people disable all of it — and
  then miss the one that mattered. Norman's word for the over-notifier: a backseat
  driver.
- **Planned as a set.** Every action gets confirmed, but design the whole vocabulary
  at once so patterns stay consistent and codings don't contradict each other across
  the product.
- **Goal-aware.** His dishwasher beeped at 3 a.m. to announce it had finished quietly
  overnight, defeating the entire point. Feedback that ignores why the user chose this
  path destroys the value of the feature.

State is feedback too. Make the current mode, the pending change, the unsaved edit,
and the last sync visible without the user having to probe for them.

## Forcing functions — and their price

A forcing function makes failure at one step prevent the next. Three kinds:

- **Interlock** — forces correct sequence. You cannot deploy before the build passes;
  you cannot submit before the required field is filled *and visibly marked so*.
- **Lock-in** — keeps an operation active until it's properly concluded. The classic
  "save, don't save, cancel" on quit. Norman's note is the good part: the dialog
  offers the desired action directly, so it stops being an obstacle and becomes a
  shortcut. Copy that — never make a blocking prompt a dead end.
- **Lockout** — prevents entry into a dangerous space. Production-mode guards,
  irreversible-action gates, destructive commands that need an explicit flag.

Every forcing function is a tax on the normal path. If the tax is too high people
defeat it — they tape over the warning light, they alias past the confirmation, they
click through without reading. The craft is finding the version that makes people
*notice* they are crossing a line without making the ordinary case annoying. Norman's
example is a stairwell gate at the ground floor: enough friction to break your
momentum, not enough that anyone props it open.

## Design assuming errors

People will make errors. This is not pessimism, it is arithmetic — so design as though
every plausible mishap will occur.

- **Undo beats confirmation.** Confirmation dialogs are mistimed: by the time it
  appears the person has already decided, so they say yes reflexively. Norman's joke
  is a dialog asking twice and then deleting the file anyway. Prefer reversible
  actions, a trash rather than a delete, an "undo" toast with real duration.
- **When you must confirm, make the object salient, not the verb.** People focus on
  the action they just chose, not the thing it lands on. Show *which* item, prominently
  — highlight it, name it, show a preview — and label the buttons with the action
  ("Delete 3 invoices" / "Cancel"), never "OK".
- **Sensibility checks.** Software can ask whether the request is reasonable. A
  transfer a thousand times the user's normal, a dose beyond any clinical range, a
  retention period of 0 days. Silent compliance with an absurd input is a design
  choice, and a bad one.
- **Separate confusable things.** Rows of near-identical controls invite acting on the
  wrong one; adjacent destructive and routine buttons invite mis-clicks. Put distance
  between them, or make them visually distinct, or both.
- **Kill modes where you can.** A mode error is doing the right thing in the wrong
  context. If a mode is unavoidable, make it loud and continuously visible — not a
  two-letter indicator in a corner.
- **Support interruption.** Interruption is a leading cause of error, and the real
  cost is resuming, not pausing. Preserve drafts, keep partial state, show where the
  user left off, and never silently discard work because focus moved.
- **Don't call it an error — help them finish.** Treat what the user did as an
  approximation of what they meant. Accept the phone number with spaces. Parse the
  date they typed. Where you genuinely cannot, say what you expected and offer the
  correction inline rather than rejecting and clearing the field.
- **Layer defenses.** Accidents come from several holes lining up, not one cause. Add
  independent checks, make each hole smaller, and surface it when several line up.

## Anti-patterns to catch in your own output

If you find yourself producing any of these, stop and redesign:

- A control whose behavior you had to explain in an adjacent tooltip, helper text, or
  README line. Norman's tell: hand-lettered signs taped to a door mean the door failed.
- Two controls that look identical and do different things.
- An icon with no label, carrying an action that isn't already a universal convention.
- A gesture, hover, long-press, or shortcut as the *only* route to a capability.
- A setting whose effect the user cannot observe, or can only observe hours later.
- A destructive action with no undo and a generic "Are you sure?".
- A form that rejects input a human would consider obviously valid.
- A "success" state indistinguishable from "nothing happened".
- Beautiful and unusable — the flush glass door with no visible hinge. When aesthetics
  and communication conflict, find the version that does both; that is the job.
- Everything on one screen because each feature had an advocate. More features means
  more controls, less space per signifier, and a worse product for everyone. Norman
  calls the disease featuritis.

## How to deliver

When you produce an interface, lead with a short design rationale — three or four
lines, not an essay — then the implementation:

1. **Conceptual model:** the one sentence the user should end up believing.
2. **Signifiers and mapping:** how a newcomer knows what's actionable and what each
   control affects.
3. **Feedback:** what confirms each action, and how state is visible at rest.
4. **Error and recovery:** what can go wrong, and how it is prevented or reversed.

Then, before you finish, walk the seven questions once against what you actually
built. It routinely finds a missing empty state, a missing loading state, or a control
whose effect is invisible.

## References

- `references/ui-translation.md` — the concept-to-interface move set in depth, with
  worked examples across web, CLI, and config surfaces. Read when you want concrete
  patterns rather than principles.
- `references/conceptual-models.md` — deriving, testing, and expressing a conceptual
  model; system image; where simplified models break. Read when the model is the hard
  part, or when the UI and the backend disagree about what exists.
