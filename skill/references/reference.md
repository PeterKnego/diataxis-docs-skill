> Adapted from [diataxis.fr](https://diataxis.fr) (https://diataxis.fr/reference/).
> Copyright © Daniele Procida. Licensed under
> [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
> Changes: distilled into rule-sheet form for the diataxis-docs
> Claude Code skill, 2026-08-02.

# Reference: rules for writing reference material

## Purpose

Reference is information-oriented: describe the machinery, succinctly, and in
an orderly way. It is led by the product it describes, not by the user.

## Enforced

- **Structure mirrors code structure.** Module/class/method relationships
  appear identically in the docs.
- **Neutral description only.** State what the machinery is and does; nothing
  else.
- **One consistent entry pattern for every entry:** name → signature →
  parameters → returns → errors/exceptions → defaults/limits → example. Where
  a field does not apply to a kind of entry — an environment variable has no
  signature and no return value — omit that field rather than filling it with
  "N/A", and keep the remaining fields in this order. Every entry of the same
  kind carries the same fields.
- **Verify every claim against source.** Note file:line for each claim in the
  plan file while drafting, not in the published page.
- **Examples are permitted as illustration only.**

## Forbidden

Where one of these has a home elsewhere, redirect to it instead of writing it here:

- **Instruction** ("to do X, …") — link to the how-to guide instead.
- **Explanation** (any *why*) — link to the explanation instead.
- **Opinion.**
- **Invented or assumed API.**

## Language patterns

- State facts: "X inherits Y's defaults; defined in `path`."
- List: "Sub-commands are: a, b, c."
- Warn: "You must use a. Never d."

## Autodoc rule

If Sphinx autodoc, typedoc, godoc, rustdoc, or a similar generator already
produces reference material for this codebase, improve the docstrings and the
structure feeding that pipeline. Never write parallel pages — they drift from
the generated output.

## Closing self-check

Run the compass check from `references/compass.md` on each page. Everything
must land in informs-cognition / application-of-skill. Move (don't delete)
anything that doesn't — park it in the plan file.
