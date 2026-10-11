---
name: norman-discover
description: Work out whether you're solving the right problem before building, the way Don Norman would — question the brief, diverge before converging, design for activities rather than tasks, and prototype and test cheaply. Use this whenever a request arrives as a stated solution ("add a dashboard", "we need a settings page", "build an export feature"), whenever requirements are vague or someone is unsure what to build, when planning a new product, feature, or flow, when scoping a rewrite or redesign, when a feature shipped and nobody used it, or when the team keeps adding features without the product getting better. Trigger it especially when the ask sounds clear — a confident brief is exactly what Norman warns you to interrogate. To build the interface, use norman-design; to audit one that exists, use norman-review.
---

# Finding the right problem

Norman's most expensive observation: teams reliably build the thing they were asked
for, and reliably fail to check whether it was the right thing. The hardest part of
design is getting the requirements right — which means making sure the right problem
is being solved, not merely that the solution is well made.

So when a request arrives already shaped as a solution, your first job is not to
implement it. It is to find the problem underneath it, and confirm the proposed
solution actually addresses that.

This is not a license to stall. Explore fast, hold to the deadline, and converge. But
converging on the wrong problem is the most costly failure available, and it is
invisible until launch.

## The double diamond

Two diverge–converge cycles, in order. Most teams skip the first one entirely and
start halfway through the second.

```
   FIND THE RIGHT PROBLEM          FIND THE RIGHT SOLUTION
        ◆                                  ◆
   discover → define              develop → deliver
   (diverge) (converge)          (diverge) (converge)
```

**Diamond one — the problem.** Widen before narrowing. What is the person actually
trying to accomplish? What do they do today instead? What happens just before and just
after? Where does it actually hurt? Then converge on a single problem statement you
can write down.

**Diamond two — the solution.** Widen again. Generate several genuinely different
approaches before choosing. Then converge, prototype, test, and ship.

Expect to loop. Converging on a solution often reveals the problem was framed wrong,
which sends you back — faster the second time. That looks like thrashing to whoever is
watching the schedule; it is the process working.

## Interrogating the brief

When handed a solution, work back to the problem. A short, specific version of the
questions:

1. **What is the person trying to accomplish?** Not the feature — the goal. "Add an
   export button" → "get this data into their monthly board deck".
2. **What do they do today instead?** The existing workaround is your best evidence.
   It tells you what the real requirement is and what's already tolerable.
3. **What happens immediately before and after?** The activity extends past your
   product's boundaries, and the pain is usually at the seams.
4. **How do we know this is the problem?** Observation, or somebody's assumption? Both
   are allowed, but they are not the same and should not be reported the same way.
5. **What would have to be true for this to be the wrong thing to build?** Cheapest
   check available, and the one most often skipped.
6. **Ask the stupid question.** Norman is explicit about liking these — questions so
   fundamental everyone assumes the answer is obvious. Why does this step exist at
   all? Why does the user have to choose? Why is this a separate object? Often nobody
   knows, and the answer is "it has always been this way".

If you get answers to 1–3 and they reframe the request, say so plainly before
building. If they confirm it, you have lost ten minutes and gained confidence.

## Design for activities, not tasks

A **task** is a low-level unit: "filter the list", "export a CSV". An **activity** is
the coherent thing a person is trying to do: "get the monthly numbers to my board".
Activities contain many tasks, ordered toward one goal.

Designing for tasks produces products where every screen is individually fine and the
whole is exhausting — you finish one task and get dumped at its boundary. Designing
for activities produces products that feel effortless, because the handoffs are
designed too.

This is also the answer to "how can one product serve very different people?"
Activities are far more similar across cultures and demographics than people are. Let
the activity define the structure; let the product's conceptual model be built around
the activity's conceptual model.

Practical consequences:

- Map the whole activity, including the parts outside your product. Where does the
  user arrive from, and where do they go next? Those seams are where most real friction
  lives.
- **People tolerate complexity that belongs to the activity** — a cockpit is fine for
  a pilot. They don't tolerate complexity that belongs to your implementation. Before
  cutting capability in the name of simplicity, check which kind you have.
- **Every case is a special case.** When you ask people how they do their work and
  then watch them, they deviate from their own description and explain it as an
  exception. Most cases are exceptions. A system that only supports the canonical path
  will be abandoned.

## The human-centered design loop

Four activities, iterated. Each cycle sharpens both the problem and the solution.

```
observe → ideate → prototype → test → (repeat)
```

**Observe.** Watch people do the real thing, in the real place, with their real
constraints. Not a lab, not a call, not a survey. Requirements produced in the abstract
are wrong; requirements produced by asking people what they need are wrong; requirements
come from watching. People report the everyday irritants they've noticed and are blind
to the larger failures they've adapted around.

**Ideate.** Generate many options before judging any. Two rules: produce numerous
ideas, because early fixation on one or two is the danger; and defer criticism, because
obviously-wrong ideas frequently contain an extractable insight. Norman adds a third:
question everything, especially the obvious.

**Prototype.** The only way to know if an idea holds is to make something and try it.
Make it cheap and disposable — sketches, a static page, a spreadsheet, a script with
hardcoded data, a skit for a service. **Wizard of Oz** is the highest-leverage
technique available: fake the intelligent system with a human behind the curtain and
learn what people actually ask for. Norman's own use of it revealed his team had scoped
the problem wrong — they thought they were building language understanding and
discovered they needed to understand activities.

**Test.** Five people, individually, is enough to surface the major findings. Then fix
and test five more. Five-then-iterate beats twenty-at-once, because it gives you
several rounds of improvement instead of one. Have them use it the way they actually
would. Watch, don't guide. Testing in pairs — one operating, one narrating — surfaces
reasoning that a solo user keeps internal. Afterwards, retrace their steps and ask why.

**Iterate.** "Fail frequently, fail fast." Failures are the only place learning
happens; if everything works, you learned nothing. Each loop should make the problem
statement clearer and the prototype closer to real.

## Evidence, honestly labelled

Two different kinds of research, both necessary, routinely confused:

| | Design research | Market research |
|---|---|---|
| Asks | What do people really need, how do they actually work | What will people buy, how do they decide |
| Method | Deep qualitative observation, small n (tens) | Surveys, focus groups, analytics, A/B, large n |
| Gives you | Real needs and the reasons behind behavior | Reliable breadth about purchasing |
| Blind to | Whether it will sell | Why people do what they do |

You need both. Numbers tell you what happened, never why; big data is seductive
precisely because it's cheap, quantitative, and produces persuasive charts about
people's real needs that it cannot actually see. When you present findings, label
which kind you have. "Users want X" backed by a survey and "users need X" backed by
watching three people are different claims and should not be written identically.

Where direct observation isn't available to you — which, working from a repo, is most
of the time — say so rather than inventing users. Legitimate proxies: existing support
tickets, issue trackers, logs and funnels, the workarounds visible in the codebase,
competitor behavior, and the way the team itself talks about the product. Weak proxies
labelled as such beat confident fabrication.

## Incremental vs radical

Almost all good design is **incremental**: test, find the problems, fix, retest. Norman's
term is hill climbing — feel for the uphill direction and take a step. It reliably gets
you to the top of the hill you're on.

It cannot find a different hill. That takes **radical innovation** — starting fresh,
usually enabled by a new capability or a reframing of what the thing even is. Radical
innovation is what everyone wants and it mostly fails; when it succeeds it often takes
decades. Incremental innovation happens millions of times a year.

Useful in practice for scoping conversations: name which one is being proposed.
"Redesign the navigation" is hill climbing and should be evaluated by whether each
change tests better. "Replace the concept of a project with something else" is a
different hill, and needs a different bar of evidence and a different appetite for
risk. Conflating them is how redesigns eat a year.

## Delivering the output

When you use this skill, produce a short document, not an essay. Something like:

```
## The stated request
[what was asked for, verbatim]

## The problem underneath
[1–2 sentences. The goal, not the feature.]
[What they do today instead, and what it costs them.]

## Evidence
[What this rests on, honestly labelled: observed / reported / inferred / assumed.]

## The activity
[The whole arc, including what happens before and after our product.
 Where the seams and handoffs are.]

## Options considered
[2–4 genuinely different approaches, one line each — including the cheapest
 possible version and the do-nothing option.]

## Recommendation
[Which one, why, and what would prove it wrong.]

## Cheapest next test
[The smallest thing that would validate this before committing. A prototype,
 a Wizard of Oz, five users, a week of instrumentation.]
```

Then stop and get agreement before building. Skipping this is the specific failure the
skill exists to prevent.

## When to shortcut this

Norman is honest that real projects have deadlines, budgets, and stakeholders, and that
the full process is often unaffordable. Match the depth to the stakes:

- **Trivial or reversible** — skip it. A tooltip's wording does not need a diamond.
- **A day's work, clear brief** — one pass at the interrogation questions, inline, in
  a couple of sentences. Then build.
- **A week or more, or a new surface** — the full short document above, agreed before
  building.
- **A rewrite, a new product, or something that shipped and nobody used** — the full
  loop, including finding actual users to watch.

The failure mode this skill guards against isn't insufficient rigor. It's building
confidently in the wrong direction because nobody spent ten minutes asking what the
person was actually trying to do.

## References

- `references/methods.md` — how to actually run each activity: observation without
  access to users, ideation prompts, prototype fidelity ladder, running a five-person
  test, personas and scenarios, writing a design specification. Read when you're doing
  the work rather than deciding whether to.
