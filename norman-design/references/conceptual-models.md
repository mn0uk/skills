# Conceptual models and the system image

Read this when the hard part isn't the layout — it's that users don't understand what
the thing *is*, or when the interface and the implementation disagree about what
exists.

## The three models

Norman's triangle, and the reason design is hard:

- **The design model** — what the designer/engineer believes the system is. Usually
  accurate, usually structured around the implementation.
- **The user's model** — what the person operating it believes. Built almost entirely
  by inference from what they can see and what similar things did before.
- **The system image** — everything perceivable: the visible structure, labels,
  states, transitions, error text, docs, marketing copy, the URL, the file layout.

The designer and the user never communicate directly. Everything the user believes
arrives through the system image. So a conceptual model is not something you *have*;
it is something you *transmit*, and the only channel is what you built.

When the system image is incomplete, incoherent, or contradictory, users construct a
model anyway — an incorrect one — and then act confidently on it. That is worse than
confusion, because confusion at least prompts them to look for help.

## What a good conceptual model is

An explanation, deliberately simplified, of how the thing works. It does not need to
be complete or even accurate. It needs to be **predictive**: it must let the user
correctly anticipate what their action will do, and correctly interpret what they see.

Folders don't exist on a disk. The desktop metaphor is fiction. Both are excellent
conceptual models because they predict correctly within their domain. Fiction is fine.
Fiction that mispredicts is not.

Test for a conceptual model: *can the user, without trying it, say what will happen if
they do X?* If not, they will be operating by rote — memorizing button sequences
without understanding, unable to recover when anything deviates. Norman's washing
machine owner who memorized a single setting and ignored the other forty is what rote
operation looks like in the wild.

## Deriving one

1. **Name the objects.** What are the nouns the user thinks in? Not your tables — their
   domain. "Project", "invoice", "run", "draft". If your UI exposes a noun the user has
   no concept of (a join table, a "resource", a "config object"), you have leaked the
   implementation into the model.
2. **Name the relationships.** Contains, belongs to, derives from, replaces. These
   become your navigation and your layout hierarchy; if the layout contradicts the
   relationships, the layout is teaching a wrong model.
3. **Name the states and the transitions.** What can each object be, and what moves it
   between states? Every state the user can reach must be nameable and visible; every
   transition must be triggerable and observable.
4. **Write the sentence.** One or two lines a new user could read and then predict
   behavior from. If you cannot write it, the model isn't coherent yet and no amount
   of UI polish will rescue it.
5. **Check the sentence against the implementation.** Every promise the sentence makes
   must be one the system actually keeps.

## Where models break, and what to do

A simplified model holds only while its assumptions hold. Most model failures are
edge-state failures, not steady-state failures.

| Assumption | Breaks when | Design response |
|---|---|---|
| The data on screen is current | Network loss, stale cache, another user edited it | Show freshness; show the disconnected state as a state, not as normal |
| My change is saved | Optimistic update failed, offline queue, conflict | Show pending vs committed distinctly; make failure visible and retryable |
| This is my copy | Shared, concurrent, or collaborative editing | Show presence and provenance; make conflict a designed state |
| It's here | Actually remote, lazily loaded, partially synced | Make locality visible where it changes behavior |
| It's gone | Soft-deleted, retained for compliance, cached elsewhere | Say what "delete" actually means, once, where it happens |
| It happened instantly | Eventually consistent, queued, batched | Show the queue; give the operation an identity the user can track |

The general rule: **design the boundary of the model, not just its interior.** The
moment the fiction stops being true is the moment the user most needs to be told,
because their model has no explanation for what they're seeing.

## When the UI and the backend disagree

This is the refrigerator problem, and it is extremely common in software. The UI
presents two independent controls; the system has one shared mechanism. Or the UI
presents a flat list; the system has a hierarchy with rules the user can't see. Or the
UI says "delete" and the system archives.

The user cannot succeed here no matter how careful they are, because the model they
were handed does not correspond to the machine. Symptoms in the wild: support tickets
that all say "I changed X and Y also changed", users running controlled experiments to
work out the rules, elaborate folk theories in your community forum.

Three legitimate resolutions, in order of preference:

1. **Make the promise true.** Change the implementation so the independent controls
   really are independent. Norman's own verdict on the fridge: more work for the
   engineering team, worth it.
2. **Change the promise.** Present the actual mechanism, even if it's less pretty —
   one thermostat and a distribution ratio, honestly labelled, is usable; two lying
   dials are not.
3. **Expose the coupling in the interface.** If both controls must stay, show the
   relationship live — move one slider and watch the other respond. A visible
   dependency is learnable; an invisible one is not.

Never: keep the false model and explain it in the documentation. Documentation is part
of the system image, but it is the weakest part, and Norman's fridge manual said
exactly the right thing ("allow 24 hours to stabilize") to no effect whatsoever.

## Feedback is what makes a model learnable

A model that cannot be tested cannot be learned. The fridge failed twice over: false
model *and* twenty-four-hour feedback latency, so even a determined user couldn't
reverse-engineer the truth.

For any setting or control, ask: how long until the user sees the consequence, and can
they attribute it? If the answer is "eventually" and "not really", you have built
something un-learnable regardless of how good the labels are. Fixes: live preview,
immediate simulated effect, an explicit "here's what this will do" summary, or a
history that lets them correlate cause and effect after the fact.

## Expressing the model in the artifact

The model should be visible in the thing itself, not only in the docs:

- **Naming.** Consistent, domain-language nouns everywhere — UI, URLs, API, logs,
  error messages. A concept called three different names in three places is three
  concepts to the user.
- **Structure.** Navigation and layout hierarchy should mirror the object hierarchy.
  If "runs belong to projects", runs live under projects.
- **Empty states.** The best place to state the model, because there's nothing else
  competing for attention. "A project holds your runs. Create one to get started."
- **Onboarding.** Useful, but a supplement — anything only explained in onboarding is
  effectively unexplained for everyone who returns in three months.
- **Error text.** Errors are where the model gets stress-tested; a message that
  references a concept the user has never seen reveals the model was never transmitted.

## Complexity vs confusion

Your own kitchen is complex and not confusing; someone else's is confusing and no more
complex. The difference is knowledge, and knowledge comes from structure.

So don't respond to "this is too complicated" by removing capability. Respond by
finding the model that makes the existing complexity intelligible. Complexity that
matches the user's activity is accepted readily. Confusion — complexity with no
apparent structure or reason — is what people actually object to, and it's usually
caused by exposing implementation structure instead of activity structure.
