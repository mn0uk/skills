# Running the human-centered design activities

Practical mechanics for observation, ideation, prototyping, and testing — including how
to do useful versions of each when you don't have access to real users, which is the
normal situation when working from a repository.

## Contents

- [Observation](#observation)
- [Observation without users](#observation-without-users)
- [From observation to a problem statement](#from-observation-to-a-problem-statement)
- [Ideation](#ideation)
- [Prototyping: the fidelity ladder](#prototyping-the-fidelity-ladder)
- [Wizard of Oz](#wizard-of-oz)
- [Running a five-person test](#running-a-five-person-test)
- [Personas, scenarios, and specifications](#personas-scenarios-and-specifications)
- [The activity hierarchy](#the-activity-hierarchy)
- [Where this collides with reality](#where-this-collides-with-reality)

---

## Observation

The point of observation is to find needs people cannot articulate. Two reasons they
can't: they've adapted to the friction and stopped seeing it, and they don't question
the overall method they use — only the irritations inside it.

Practical rules:

- **Go where the work happens.** Same place, same tools, same interruptions, same
  deadline pressure. Context is not decoration; it is most of the finding.
- **Watch, don't interview.** Asking "how do you do X" gets you a tidy narrative.
  Watching gets you the truth, including the parts they'd never mention because they're
  embarrassing or feel like exceptions.
- **When they deviate from their own description, that's the finding.** "Oh, I had to
  do this one differently, it was a special case." Collect these. Most cases are
  special cases, and a design that only handles the canonical path will be abandoned.
- **Follow the whole activity, past your product's edges.** Where did this data come
  from, where is it going, what happens to it next.
- **Match the people to the intended audience.** Age and income usually matter less
  than the activity; but if the product is for a specific subculture, that specific
  group must be observed, not a convenient stand-in.
- **Note the workarounds.** Spreadsheets beside the app, copy-pasted templates,
  scripts, sticky notes, a shared doc of incantations. Each one is a requirement
  written by a user.
- **Small n is fine.** Depth over breadth is the whole point; tens, not thousands.
  Pair it with quantitative work if you need to know scale.

## Observation without users

Usually you can't watch anyone. These are legitimate substitutes, in rough order of
value — use them and label them honestly rather than inventing a user.

1. **Support tickets and issue trackers.** Cluster them. Repeated questions are
   discoverability defects; repeated complaints about the same concept are conceptual
   model defects. The exact words people use are also your vocabulary source.
2. **Logs, funnels, and error rates.** Where people drop off, what they retry, what
   they abandon. Tells you where, never why.
3. **Workarounds visible in the codebase or config.** Scripts users wrote, exports
   nobody asked for that everyone uses, copy-pasted config blocks, wrapper CLIs.
4. **Search queries** — inside your product and to your docs. A direct readout of what
   people expected to find and didn't.
5. **The team's own language.** What they say out loud in demos is a signifier missing
   from the product. What they explain repeatedly is a model that isn't transmitted.
6. **Competitor and adjacent products.** Not to copy, but to see which conventions
   users already carry with them.
7. **Your own use of the thing, as a novice.** Weak, because you can't unknow the
   model — but honest first-run notes taken before you look at the code have real value.

State which of these you used. "Inferred from 40 support tickets" and "assumed" are
different claims.

## From observation to a problem statement

Converge to one sentence you could put on a wall:

> **[Person]** trying to **[activity]** currently has to **[workaround]**, which costs
> them **[specific cost]**, because **[underlying cause]**.

Test it:

- Does it name a goal, not a feature? If the sentence contains your proposed solution,
  you haven't converged on a problem yet.
- Would the person recognize themselves in it?
- Does it suggest at least three different solutions? If exactly one, it's a solution
  in disguise.
- Do you know what would make it false?

## Ideation

Diverge properly before choosing. The failure mode is fixating on the first workable
idea and spending the rest of the project defending it.

Rules:

- **Quantity first.** Early fixation is the danger; generate more than feels necessary.
- **Defer judgment.** Wrong ideas carry extractable insight. Criticism during
  generation kills the risky ideas first, and the risky ideas are why you're diverging.
- **Question everything, especially the obvious.** The "stupid question" — why does
  this step exist, why must the user choose, why is this a separate concept — is often
  profound, because the honest answer is usually "nobody remembers".

Prompts that reliably produce genuinely different options:

- What's the version with **no UI at all**? (a default, an inference, an automation)
- What's the version that's **one control**?
- What if the user **never has to choose** — what would we have to know?
- What if this happened **somewhere else entirely** — their email, their terminal,
  their existing tool?
- What's the **do-nothing** option, and what does it actually cost?
- What would this look like if the **constraint we assume is fixed** weren't?
- What's the version we could ship **tomorrow**?

Always carry forward at least: the cheapest possible version, the do-nothing option,
and one structurally different approach. A recommendation with no alternatives is a
decision that was never made.

## Prototyping: the fidelity ladder

Use the lowest rung that answers your question. Fidelity beyond the question wastes
time and — worse — makes people critique the styling instead of the idea.

| Rung | Form | Answers |
|---|---|---|
| 0 | A sentence describing the experience | Is this even the right problem |
| 1 | Sketch, whiteboard, ASCII layout | Does the structure make sense |
| 2 | Static mockup or plain HTML, no logic | Is it discoverable; do labels read right |
| 3 | Clickable prototype, hardcoded data | Does the flow hold together |
| 4 | Wizard of Oz — human behind the curtain | What do people actually ask for |
| 5 | Working prototype, real data, ugly | Does it survive real content and edge cases |
| 6 | Production candidate | Does it hold up at scale |

Cheap materials are a feature: index cards, spreadsheets, slides, a script printing to
a terminal, a skit for a service. Something disposable gets honest criticism; something
polished gets defended by its author.

During diamond one, prototypes exist to check you've understood the *problem*. During
diamond two, they test the *solution*. Don't confuse the two — a beautiful prototype
of the wrong problem is the most expensive artifact in software.

## Wizard of Oz

Fake the system with a human behind it. The highest-value technique for anything
intelligent, automated, or conversational, because it lets you learn what people ask
for before you can build anything that answers.

Mechanics: the participant believes they're using a real system; a person elsewhere
reads their input and responds as the system would. Log everything.

What it gives you that nothing else does: **the real distribution of requests.** Norman
ran this for a travel booking system and found people asked things the designed system
couldn't conceive of — "I need to be back before my 9am class" — which revealed the
project had been scoped as language understanding when the actual requirement was
understanding activities. That reframing was worth more than any amount of building.

Applies directly to anything agentic or AI-shaped: before designing prompts, tools, or
UI, have a human play the assistant for a day and read what people actually ask.

## Running a five-person test

Five people, individually, surfaces the major findings. Then fix and run five more.
Several rounds of improvement beats one large study — a single test of twenty gives you
one round of learning; four tests of five gives you four.

Setup:

- Recruit people who match the intended audience for the *activity*.
- Give them a real goal, not instructions. "Get last month's numbers to your manager",
  not "click export and then choose CSV".
- Sit behind them or watch remotely. Visible camera, described in advance.
- **Do not help.** The urge to rescue is overwhelming and destroys the data. Silence is
  the finding.
- Optionally test in pairs — one drives, one narrates — which makes reasoning audible
  without the artificiality of think-aloud.

During:

- Record where they hesitate, what they say aloud, what they click that isn't
  clickable, what they never notice, and where they give up.
- Note wrong turns as design defects, not user errors. Every "they should have seen
  it" is a missing signifier.

After:

- Retrace their steps with them and ask why at each decision point. Video helps.
- Ask what they expected to happen, not whether they liked it. Preference data from
  five people is noise; comprehension data from five people is signal.

Analysis: cluster into defects, rank by severity, fix the top ones, run again. If the
same defect appears in three of five, it's real.

## Personas, scenarios, and specifications

Useful as communication artifacts; dangerous when they replace observation.

- **Persona** — a synthesized person representing observed patterns. Only as good as
  the observation behind it. A persona invented in a meeting is a shared assumption
  wearing a photo. Keep them short, activity-focused, and traceable to real evidence.
- **Scenario** — a story of a persona pursuing a goal, with context and constraints. The
  more useful artifact, because it forces the whole activity including what happens
  before and after. Write the messy scenarios too: interrupted, offline, second attempt,
  someone else's data.
- **Use case** — the sequence of interactions in the abstract. Necessary for
  completeness; insufficient for design, because it strips motive and context.
- **Design specification** — what gets built. Norman's point that most teams miss: the
  spec must cover both *buying* and *using*. A product nobody purchases fails; a
  purchased product nobody can use also fails. Both belong in the requirements.

## The activity hierarchy

Useful for locating what you're designing for. Goals stack:

- **Be-goals** — why the person acts at all; identity and long-term motivation ("be
  good at my job", "be seen as reliable"). Slow-moving, rarely addressed directly, but
  they determine what people care about.
- **Do-goals** — the practical objective driving this session ("get the board deck
  out"). This is the level the seven stages of action operate at, and the level you
  should design for.
- **Motor-goals** — how an action is physically performed ("click this, type that").
  Tasks and operations. Optimizing here while ignoring do-goals is how you get products
  where every interaction is smooth and the whole thing is useless.

Activities decompose into tasks, tasks into operations. Design at the activity level;
make the tasks work seamlessly together; don't let one task's requirements break
another's.

## Where this collides with reality

Norman devotes a section to admitting the process rarely runs as described, and the
honesty is worth carrying:

- **Schedule and budget are real constraints**, not obstacles to route around. The
  discipline is to explore freely inside a firm deadline; nothing produces convergence
  like a date. Expect the result to arrive in the last day regardless of how long you
  had.
- **You are one discipline among several.** Engineering, marketing, legal, and finance
  all have legitimate requirements, and design does not automatically outrank them. A
  design that ignores what makes the product viable is not a good design.
- **Iteration looks like thrashing** to people watching the schedule, particularly when
  you loop back to redefine the problem. Explain what you're doing and what it buys.
- **You will not get to observe users** on most projects. Do the honest proxy version
  and label the confidence, rather than skipping the step or fabricating.
- **The full process is usually unaffordable.** Match depth to stakes. A single pass at
  the interrogation questions, taking ten minutes, captures most of the value on most
  tasks. That's not a compromise of the method — it *is* the method, scaled.
