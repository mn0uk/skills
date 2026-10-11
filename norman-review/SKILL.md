---
name: norman-review
description: Audit an existing interface the way Don Norman would — walk the seven stages of action, find where discoverability, feedback, mapping, conceptual model, or constraints break down, classify each defect by the error it will cause, and prescribe the specific fix. Use this whenever someone asks you to review, critique, audit, or improve existing UI, or says a design "feels off", users are confused, people keep clicking the wrong thing, support keeps getting the same question, or a flow has a high drop-off. Also use it proactively before shipping any interface you or someone else just built. Trigger on screenshots, mockups, Figma descriptions, component code, CLI help output, and live URLs alike. To design something new, use norman-design; to question whether it's the right thing to build, use norman-discover.
---

# Norman review

You are auditing an artifact against one standard: **can a person who has never seen
this, and will not read anything first, operate it and understand what happened?**

Everything the user will ever know arrives through what you can see. So review only
what is perceivable. Knowing how the code works makes you a worse reviewer unless you
deliberately set it aside — you are the one person who cannot be surprised by it.

Two rules that keep the review honest:

- **The design is at fault, not the user.** When you catch yourself writing "users
  should notice" or "it's obvious once you know", you have found a defect and
  described it as a user failing. Rewrite it as a defect.
- **Don't criticize unless you can do better.** Every finding carries a specific,
  implementable fix. A critique without a fix is an opinion.

## Procedure

### 1. Establish the model

Before looking for faults, write the conceptual model the interface *appears* to
project — one or two sentences, inferred only from what's visible. Then compare it to
what the system actually does.

Divergence here outranks every other finding, because it is the one class of defect
the user cannot overcome by being careful. Two controls that share one mechanism, a
"delete" that archives, a flat list over a hidden hierarchy: the user builds a wrong
model, acts confidently, and gets punished. Report it first.

If you cannot write the sentence at all, that is itself the top finding.

### 2. Walk the seven stages

Take a real goal a user would have, and traverse it as a newcomer would. At each
stage, ask whether the interface — not the docs, not the tooltip you'd have to hunt
for — answers the question.

**Gulf of Execution** (answered by signifiers, constraints, mapping, conceptual model):

1. **What do I want to accomplish?** Does the interface support the goal the user
   actually has, or only the tasks the team implemented?
2. **What are my alternatives?** Are the possible routes visible, or does one require
   knowing it exists?
3. **What can I do right now?** Is every actionable element identifiable as
   actionable? Is every unavailable one explained rather than merely greyed?
4. **How do I do it?** Once they've chosen, is the mechanism clear without trial?

**Gulf of Evaluation** (answered by feedback and conceptual model):

5. **What happened?** Is there feedback at all, on the thing acted upon, promptly?
6. **What does it mean?** Is the feedback informative, or just "something occurred"?
7. **Did I get what I wanted?** Can they confirm success against their goal, or only
   against the system's notion of success?

Do this for the happy path, then again for at least one failure path and one
interruption (navigate away and come back). Most defects live outside the happy path,
and most reviews never leave it.

### 3. Classify the errors it will cause

For each weak point, name the error a real user will make. This is what turns a vague
"confusing" into a fix, because each class has a different remedy. Full taxonomy and
remedies in `references/error-taxonomy.md`; the short form:

- **Slips** — right intention, wrong execution. Caused by similarity, proximity,
  invisible modes, and interruption. Fixed by separation, distinctness, visible state,
  and undo. Experts make these more than novices.
- **Mistakes** — wrong intention. Caused by a bad conceptual model, ambiguous state,
  or misleading labels. Fixed by better models, clearer information, and sensibility
  checks. Confirmation dialogs do **not** fix mistakes — the user believes they are
  right, so they confirm.

That last point is worth stating in reviews explicitly, because "add a confirmation"
is the reflex fix and it's usually the wrong one.

### 4. Check the standard failure catalogue

Run the interface past `references/failure-catalog.md` — the recurring named patterns
(Norman doors in software, confirmation theater, hidden gestures, alarm fatigue, false
mapping, featuritis, and the rest). It catches things a stage-walk misses because they
are structural rather than sequential.

### 5. Report

## Output format

Lead with the verdict, then findings ordered by severity. Keep it to what someone can
act on this week.

```
## Conceptual model
[The model the interface projects, in 1–2 sentences.]
[Whether the system honors it. If not, this is finding #1.]

## Findings

### 1. [Short name of the defect] — [Critical | Major | Minor]
**Where:** [screen, component, file:line, or the specific control]
**Breaks:** [which of the seven questions goes unanswered, and/or which principle:
discoverability, feedback, mapping, conceptual model, constraints, signifiers]
**What will happen:** [the concrete error a real user makes — name the class]
**Fix:** [specific and implementable, not "improve clarity"]

### 2. ...

## What's working
[Two or three things done well, named in the same vocabulary. This is not politeness —
it tells the team which patterns to propagate.]
```

Severity rubric:

- **Critical** — the user cannot succeed, or will destroy something, or will form a
  false model and act on it. Data loss, irreversible actions without recovery, controls
  that lie about what they do.
- **Major** — the user succeeds only by trial and error, by asking someone, or by
  reading docs. Missing feedback on a common action, undiscoverable primary
  capability, mode with no visible indicator.
- **Minor** — friction, inconsistency, or a defect on a rare path.

Calibrate to what you were asked for. A quick gut-check gets the top three findings; a
pre-launch audit gets the full walk including failure and interruption paths. Ten
findings ranked by severity beats forty ranked by nothing — a review nobody can act on
has the same value as no review.

## Where to look when nothing jumps out

If the interface seems fine but something is off, these are the usual hiding places:

- **The empty state.** Usually unbuilt, and it's the first thing every new user sees.
- **The error state.** Usually a generic string that names an exception rather than a
  remedy.
- **The loading state.** Especially the 1–10 second band where a component renders
  blank and looks broken.
- **The second time through.** Onboarding carried the model; nothing carries it now.
- **Coming back after an interruption.** Draft gone, scroll lost, filters reset, place
  in the flow forgotten.
- **The destructive path.** What's reversible, what isn't, and whether the user can
  tell them apart before acting.
- **The boundary of the fiction.** Offline, stale, conflicted, partially synced,
  permission-denied — states the projected model has no words for.
- **What the team explains verbally in demos.** Anything a colleague has to say out
  loud while showing you the product is a signifier that isn't in the product. Norman's
  tell was hand-lettered signs taped to doors; the software equivalent is the demo
  narration and the pinned Slack message.

## Reviewing your own output

The same procedure applies to interfaces you just produced, and you should run it
before presenting work. Two adjustments: you know the model, so you cannot judge
discoverability by introspection — check that each answer is carried by something
visible, and name the specific element carrying it. And you will be tempted to defend
choices; a finding you argue with is usually a real finding.

## References

- `references/error-taxonomy.md` — slips vs mistakes, all five subtypes, the distinct
  remedy each one requires, and how to tell them apart from a bug report. Read when
  classifying a defect or when a team keeps applying the wrong fix.
- `references/failure-catalog.md` — the recurring named failure patterns with their
  diagnosis and standard fix. Read during step 4, or when you need the right name for
  something you can see is wrong.
