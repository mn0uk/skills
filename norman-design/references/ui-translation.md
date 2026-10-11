# Norman's concepts → concrete interface moves

Norman wrote about doors, stoves, and refrigerators. This file does the translation
work so you don't have to re-derive it every time. Use it when you know the principle
and want the move.

## Contents

- [Signifiers: the working catalogue](#signifiers-the-working-catalogue)
- [Mapping in software](#mapping-in-software)
- [Feedback by latency band](#feedback-by-latency-band)
- [Constraints, concretely](#constraints-concretely)
- [Forcing functions in software](#forcing-functions-in-software)
- [Modes](#modes)
- [Destructive actions and undo](#destructive-actions-and-undo)
- [Error messages that teach](#error-messages-that-teach)
- [Interruption and resumption](#interruption-and-resumption)
- [Surface-specific notes: CLI, config, API](#surface-specific-notes)
- [The activity, not the task](#the-activity-not-the-task)

---

## Signifiers: the working catalogue

A signifier is any perceivable indicator of where and how to act. The question to ask
of every interactive element: *if I showed this to someone with no context and no
documentation, would they know it was actionable, and what it would do?*

Reliable signifiers, roughly in order of strength:

1. **A visible label saying what happens.** "Export CSV" beats a download glyph.
2. **A control shaped like its function.** A slider for a range, a stepper for a
   count, a toggle for a binary. Picking the right control type is a signifier
   decision, not a styling decision.
3. **Position within a learned convention.** Primary action bottom-right of a dialog,
   destructive action separated, navigation along the top or left.
4. **Affordance-mimicking styling.** Raised or bordered = pressable. Underlined and
   colored = link. An input with a visible border and a caret = type here. Flat
   design's cost is exactly this: it strips the signifier and keeps the affordance.
5. **Peeking / partial reveal.** The half-visible next card that signifies a
   horizontal scroll. The clipped list that signifies "more below". This is the single
   best fix for hidden scroll and hidden swipe.
6. **State change on approach.** Cursor change, hover highlight, focus ring. Useful,
   but never the *only* signifier — they don't exist on touch, and they don't exist
   for someone scanning a screenshot.
7. **An empty state that teaches.** The most under-used signifier in software. A list
   with nothing in it is the one moment you have the user's full attention and no
   competing content: use it to say what this is, what to do first, and where.

Anti-signifiers to fix on sight:

- Icon-only toolbars for anything beyond ✕ / ▶ / 🔍.
- Text that is clickable but styled identically to text that isn't.
- Disabled controls with no explanation of what would enable them. A disabled button
  is an anti-affordance; like glass, it needs a signifier or people will keep walking
  into it. Put the unmet condition next to it, or make it enabled-and-explaining
  instead of disabled-and-silent.
- Capabilities reachable only by right-click, long-press, hover, drag, or keyboard
  shortcut with nothing on screen hinting they exist.
- Placeholder text used as the only label — it disappears exactly when the user needs
  it, which converts a label into a memory test.

## Mapping in software

Natural mapping means the relationship between control and effect needs no learning.

- **Spatial correspondence.** Controls arranged as the things they control. A settings
  panel whose sections mirror the on-screen regions they affect. A diff view where the
  action for a hunk sits on that hunk.
- **Proximity.** Put the control next to what it changes. An "edit" button at the top
  of a page that edits a field halfway down is an unmapped control. Inline editing is
  a mapping fix, not a fashion.
- **Grouping.** Related controls cluster; unrelated controls separate. Gestalt
  proximity is doing real cognitive work — whitespace is a mapping tool.
- **Direction conventions.** Up/right = more, later, forward. Down/left = less,
  earlier, back. Violating this to be clever costs the user every single time.
- **One-to-one honesty.** If two controls exist, they should have two independent
  effects. Two controls sharing one underlying knob is the refrigerator failure — the
  user runs experiments and never converges.

Where natural mapping is impossible (abstract settings with no spatial analogue),
substitute a strong *conceptual* mapping instead: consistent naming, a visible
hierarchy that matches the domain, and a live preview so cause and effect are at least
observable.

## Feedback by latency band

Match the feedback mechanism to how long the operation takes. The bands matter more
than the specific widget.

| Duration | What the user needs | Typical move |
|---|---|---|
| < 100 ms | Nothing extra — it feels instant | Just render the result |
| 100 ms – 1 s | Acknowledgement that input registered | Pressed/active state, immediate optimistic render |
| 1 s – 10 s | Something is working, and it's still working | Inline spinner *on the element acted upon*, skeleton rows, disabled-with-reason |
| > 10 s | Progress, an estimate, and an escape | Determinate progress, step labels, cancel, and the ability to leave and come back |
| Backgrounded | Where to look later, and closure when done | Persistent status affordance + a completion signal that respects context |

Rules that cut across all bands:

- Put the feedback **on the thing acted upon**, not in a corner. A toast at the bottom
  of the screen about a row at the top makes the user do the correlation.
- **Never fail silently.** An action that produces no visible change is
  indistinguishable from a broken button — and the user's next move is to press it
  again, which is how duplicates get created.
- **Optimistic updates need a real reconciliation path.** If you show success before
  it's confirmed, design the un-showing. A row that silently reverts is worse than a
  spinner.
- **Distinguish "nothing matched" from "nothing loaded" from "nothing exists".** Three
  different states, three different user responses, and most UIs render all three as
  an identical blank area.

## Constraints, concretely

Prefer a constraint that makes the wrong action impossible over a validation that
reports it afterwards. Ranked, best first:

1. **Make the wrong state unrepresentable.** Date pickers that can't produce an
   inverted range. A select instead of a free-text field with five valid answers. Sum
   allocations that auto-balance to 100%.
2. **Constrain during input.** Masks, steppers, sliders bounded to the legal range,
   drop targets that only highlight for compatible items, autocomplete restricted to
   real entities.
3. **Validate on blur, at the field, with the expectation stated.** Not on submit, not
   at the top of the page, not by clearing what they typed.
4. **Validate on submit** — last resort, and then focus the first offending field and
   say what would be acceptable.

Cultural constraints are free leverage: reuse platform and web conventions unless you
have a specific reason not to. Every convention you honor is a thing the user already
knows. Every one you break is a thing they must learn, and they didn't ask to.

Semantic and logical constraints show up as *derived* UI: don't ask for what you can
infer, don't offer options that this state makes meaningless, and hide or explain what
cannot apply rather than presenting it as available.

## Forcing functions in software

| Kind | Purpose | Good software examples | Failure mode |
|---|---|---|---|
| Interlock | Enforce sequence | Required steps in a wizard; can't merge until checks pass; can't publish without a title | Sequence that's arbitrary rather than semantic — users resent it and route around |
| Lock-in | Prevent premature exit | Unsaved-changes prompt; "you have an active session"; draft preserved on navigate-away | Prompt that blocks without offering the resolution inline |
| Lockout | Block a dangerous space | `--force` required for destructive ops; production guardrails; typed confirmation for irreversible deletes | Applied to routine actions, so users automate past it |

Design rule for all three: **offer the desired action inside the barrier.** Norman
liked the save-on-quit dialog precisely because you can save from within it — the
forcing function becomes a shortcut instead of an obstacle. A prompt that only says
"you can't do that" has spent the user's attention and given nothing back.

Second rule: **cost the tax.** If a guard fires on the common path, it's in the wrong
place. Move it to the rare, dangerous path, or make it cheaper.

## Modes

A mode error is performing a correct action in the wrong context — the most
under-diagnosed class of interface bug.

Where modes hide in software: edit vs view, insert vs overwrite, staging vs
production, the selected environment/tenant/branch, "recording", offline mode, a
modal that stole focus, vim, a shell in a different directory.

The hierarchy of remedies:

1. **Remove the mode.** Make the action explicit per-invocation instead of a persistent
   state. This is why `git commit -m` beats an editor mode for one-liners.
2. **If it must exist, make it continuously and unmissably visible** — not a badge
   in a corner. Change the background, the chrome color, the prompt itself. Production
   should not look like staging.
3. **Make the mode's effect visible in the action itself.** The button says "Deploy to
   **production**", not "Deploy".
4. **Add a sensibility check at the boundary** — the action that only makes sense in
   the other mode should ask.

## Destructive actions and undo

The default should be reversible. Work down this list and stop at the first one you
can actually implement:

1. **Fully undoable** — soft delete, trash, versioned state, an undo toast with a
   real window (5–10 s, and longer for bulk).
2. **Undoable with effort** — archived rather than deleted, exportable before removal,
   recoverable from a snapshot the user is told about.
3. **Irreversible but gated** — the object named prominently, the consequence spelled
   out, a typed confirmation proportional to the blast radius, and the button labelled
   with the actual action and count.

Never: a bare "Are you sure?" with OK/Cancel guarding an irreversible operation. It
reads as ceremony, gets clicked through, and provides no information about *what* is
about to be destroyed.

For bulk operations, show the manifest. "Delete 47 items" should be able to tell you
which 47 before it does it.

## Error messages that teach

An error message is feedback under the worst possible conditions — the user is already
blocked and probably annoyed. Norman's framing: in conversation we don't beep at each
other, we ask for clarification. Structure:

1. **What happened**, in the user's terms, about their thing. Not the exception class.
2. **Why**, if you know — and if you don't know, say that rather than guessing.
3. **What to do next**, as a concrete action, ideally a control right there.
4. **Preserve their work.** Never clear the form. Never lose the draft.

Bad: `Error: invalid input`. Better: `Start date (12 Mar) is after end date (4 Mar) —
swap them?` with a button that swaps them.

Reserve the loud treatment for things that are genuinely both urgent and actionable.
Everything else degrades the channel — Norman's control room where every alarm sounds
at once and operators spend the emergency silencing them.

## Interruption and resumption

Interruption is a major error source, and the expensive part is coming back, not going
away. Most software discards precisely the state needed to resume.

- Persist drafts continuously, not on submit.
- Preserve scroll position, expansion state, filters, and selection across navigation
  and reload.
- When a multi-step flow is re-entered, say where they were and what remains.
- Deep-link to state so the user can leave and return via their own bookmark.
- If you must interrupt them, make your interruption resumable too — a dismissed
  dialog should be retrievable, not gone.

## Surface-specific notes

**CLI.** Discoverability is the whole problem: the affordance is total (any string can
be typed) and the signifiers are nearly zero. `--help` is the primary signifier — treat
it as a designed screen, not a dump. Show the three commands people actually use
before the forty they don't. `--dry-run` is a forcing function that costs nothing.
Errors should print the fix as a runnable command. Confirm destructive ops by naming
the target, and let `--force` exist so scripts aren't forced to fake a TTY. Exit codes
are feedback for programs; make them meaningful.

**Config files.** The file is the interface. Comment the conceptual model at the top.
Fail with the line number, the offending key, the expected shape, and the nearest
valid key when someone typos. Defaults are the mapping: the default config should
describe the common case, not the empty case.

**APIs.** Names are signifiers; parameter order is mapping; error responses are
feedback. Make illegal states unrepresentable in the type/schema rather than
documenting them. Idempotency keys are undo for distributed systems.

## The activity, not the task

Norman's sharpest process point applies at design time too: design for **activities**,
not isolated tasks. An activity is "get this report to my boss"; the tasks are filter,
export, rename, attach, send. Products that support the whole activity feel effortless;
products that perfect one task and dump you at its boundary feel hostile even when
every screen is individually good.

Practically: trace the user's path from before your UI to after it. The export that
lands in a folder they then have to find, rename, and re-upload has optimized a task
and ignored the activity. Where does your interface hand off, and does it hand off well?

People will tolerate real complexity if it belongs to the activity — a cockpit is fine
for a pilot. What they will not tolerate is arbitrary complexity that belongs to your
implementation. Complexity is not the enemy; confusion is.
