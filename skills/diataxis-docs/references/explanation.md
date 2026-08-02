> Adapted from [diataxis.fr](https://diataxis.fr) (https://diataxis.fr/explanation/, https://diataxis.fr/reference-explanation/).
> Copyright © Daniele Procida. Licensed under
> [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
> Changes: distilled into rule-sheet form for the diataxis-docs
> Claude Code skill, 2026-08-02.

# Explanation: rules for writing explanation

## Purpose

Explanation is understanding-oriented: a discursive treatment of a bounded
topic that permits reflection. It serves study away from the product — it's
the only kind of documentation that might make sense to read in the bath. It
answers *why*.

## Enforced

- **Title passes the implicit-*About* test.** Every explanation guide is
  about a topic, in the sense of being around it. You should be able to place
  an implicit (or explicit) *About* in front of the title: *About user
  authentication*, *About database connection policies*.
- **Use a real or imagined why-question as the prompt** for each piece.
  Tutorials, how-to guides and reference are each bounded by something
  well-defined — what to learn, what task to achieve, the scope of the
  machine. Explanation has no such natural boundary; a why-question is what
  bounds it.
- **Cover design decisions, history, constraints, trade-offs, alternatives,
  comparisons.** Provide background and context: explain why things are so,
  draw implications, mention specific examples.
- **Opinion and perspective are welcome.** Understanding comes from a
  standpoint, and other standpoints exist. Weigh contrary views; consider
  alternatives and counter-examples rather than only stating one position.
- **Make connections beyond the immediate topic**, even to things outside it,
  when that helps weave a web of understanding.
- **Bound the topic explicitly.** Draw reasonable lines marking out the area
  and be satisfied with them — the open-endedness of explanation is a risk,
  not a license.

## Forbidden

Each of these has a home elsewhere — link out instead of writing it here:

- **Step-by-step instructions.** Explanation opens a topic for consideration;
  it does not give instruction.
- **Reference tables.**
- **Letting the topic absorb how-to or reference material.** The urge to
  cover a topic completely tempts writers to fold in instruction or technical
  description. Docs already have other places for these — creep here
  interferes with the explanation and removes that material from its correct
  place.

## Language patterns

- "The reason for x is historical: y…"
- "W is better than z, because…"
- "Some users prefer w (because z). This can be a good approach, but…"

## Source-mining

Draw material from ADRs, commit messages, PR discussion, RFCs, and design
documents — these are where the *why* behind a decision actually lives.

## Closing self-check

Run the compass check from `references/compass.md` on each page, section by
section as well as whole-page — adapted pages especially tend to pass at a
distance while single sections remain reference tables or structure dumps. It
must land in informs-cognition / acquisition-of-skill. Then apply the bath
test — would someone read this away from the keyboard, reflecting rather
than working? If not, it has drifted toward reference or a how-to guide.
