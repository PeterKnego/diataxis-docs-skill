> Adapted from [diataxis.fr](https://diataxis.fr) (https://diataxis.fr/compass/, https://diataxis.fr/map/).
> Copyright © Daniele Procida. Licensed under
> [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
> Changes: distilled into rule-sheet form for the diataxis-docs
> Claude Code skill, 2026-08-02.

# Compass: classify any piece of content

The "compass check". Run it before writing anything, and again before closing
out any quadrant. It resolves "what kind of documentation is this?" when
intuition is unreliable, absent, or contradicts itself.

## The compass table

| If the content… | …and serves the user's… | …then it must belong to |
|---|---|---|
| informs action | acquisition of skill | a tutorial |
| informs action | application of skill | a how-to guide |
| informs cognition | application of skill | reference |
| informs cognition | acquisition of skill | explanation |

## Term glosses

- **action** — practical steps, doing
- **cognition** — theoretical or propositional knowledge, thinking
- **acquisition** — study
- **application** — work

## The two questions

Ask of any content, at sentence level and at document level:

1. action or cognition?
2. acquisition or application?

Answer both, read the table. Apply close-up (a sentence, a paragraph) and from a
distance (the whole document) — a document can pass at the wide view and still
fail at the close view.

## The map summary

| Type | Does | Answers | Oriented to | Form |
|---|---|---|---|---|
| Tutorial | introduces | "Can you teach me to…?" | learning | a lesson |
| How-to guide | guides | "How do I…?" | goals | a series of steps |
| Reference | states | "What is…?" | information | dry description |
| Explanation | explains | "Why…?" | understanding | discursive explanation |

## Blur diagnostics

Adjacent quadrants share a trait and bleed into each other along it. Check each
pair explicitly — don't rely on a single read to catch these:

- **Tutorial ↔ how-to guide** — both guide action. Telltale: a "tutorial" that
  assumes competence it never taught, or a "how-to guide" that stops to teach.
- **How-to guide ↔ reference** — both serve application (work). Telltale:
  option tables and parameter lists growing inside a guide's steps.
- **Reference ↔ explanation** — both are propositional (cognition). Telltale:
  an illustrative example expanding into a digression on *why*.
- **Explanation ↔ tutorial** — both serve acquisition (study). Telltale:
  explanation crowding out the lesson, turning doing into reading.

## Rules of thumb

- Boring and unmemorable → probably reference.
- Lists and tables of things (classes, methods, attributes) → reference.
- Readable in the bath, away from the keyboard → explanation.
- The work/study test: would someone reach for this *while working*, or only
  *after stepping away* to think about it? Working → application (how-to
  guide/reference). Stepped away → acquisition (tutorial/explanation).
