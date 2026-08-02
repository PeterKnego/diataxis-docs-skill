> Adapted from [diataxis.fr](https://diataxis.fr) (https://diataxis.fr/how-to-guides/, https://diataxis.fr/tutorials-how-to/).
> Copyright © Daniele Procida. Licensed under
> [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
> Changes: distilled into rule-sheet form for the diataxis-docs
> Claude Code skill, 2026-08-02.

# How-to guides: rules for writing how-to guides

## Purpose

How-to guides are goal-oriented. They serve the already-competent user who is
at work, not at study. A guide answers to a human project — it should show
what the human needs to do, with the tools at hand, to obtain the result they
need. It never addresses a tool operation for its own sake.

## The failure mode

The common, wrong pattern: a guide defined by "operations that can be
performed with a tool or system." This offers no value — it is not addressed
to any need the user has, only to taking the machinery through its motions.
"To deploy the desired database configuration, select the appropriate options
and press Deploy" is not guidance; it is useless information any competent
practitioner already has.

Before writing or accepting a guide, answer: what does a person need to get
done in the real world? If the honest answer names only a tool's own
controls, the guide is not yet a how-to guide.

## Enforced

- **Title form:** *How to \<real-world goal\>*.
- **Conditional imperatives.** "If you want x, do y. To achieve w, do z."
- **Branch where the real world branches.** Sequences may fork and overlap
  and have multiple entry and exit points — real-world problems do not
  always reduce to a single linear procedure.
- **Assume competence.** The guide serves the already-competent user; omit
  what any practitioner in this domain already knows.
- **Start and end at reasonable points.** Practical usability matters more
  than completeness. Unlike a tutorial, a how-to guide need not be a
  complete, end-to-end account — it should begin and end somewhere
  meaningful and let the reader join it up to their own work.
- **Order the sequence for flow.** Ground steps in the pattern of the user's
  own activity and thinking. Minimize context-switching between tools or
  subjects. Don't require the user to hold a thought open across steps that
  could instead resolve it in action.

## Forbidden

Each of these has a home elsewhere — link out instead of writing it here:

- **Goals that amount to "call this function."** Not a real-world project.
- **Teaching or explanation.** Action and only action; digression dilutes
  the guide.
- **Exhaustive option lists.** "Refer to the x reference guide for a full
  list of options" — don't pollute the guide with every related possibility.

## Title test

- good: *How to integrate application performance monitoring*
- bad: *Integrating application performance monitoring* — maybe the document
  is about whether to, not how to.
- very bad: *Application performance monitoring* — could be how, whether, or
  just an explanation of what it is.

## Tutorial vs. how-to guide

| | Tutorial | How-to guide |
|---|---|---|
| Setting | contrived — a learning environment | real world |
| Path | a single line, no choices | forks and branches, multiple routes |
| Responsibility for trouble | the teacher | the user |
| The unexpected | eliminated | prepared for |
| Serves | study — acquisition of skill | work — application of skill |

## Closing self-check

Run the compass check from `references/compass.md` on each guide, section by
section as well as whole-guide — a guide can pass at a distance while one of
its sections teaches or turns into a reference table. It must land in
informs-action / application-of-skill. And ask, per guide: what human
project does this serve? The answer must not be a tool operation.
