> Adapted from [diataxis.fr](https://diataxis.fr) (https://diataxis.fr/tutorials/, https://diataxis.fr/tutorials-how-to/).
> Copyright © Daniele Procida. Licensed under
> [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
> Changes: distilled into rule-sheet form for the diataxis-docs
> Claude Code skill, 2026-08-02.

# Tutorials: rules for writing tutorials

## Purpose

Learning-oriented. A tutorial is a lesson: the learner acquires skill by doing
a meaningful, achievable project. Responsibility for success belongs entirely
to the teacher — the pupil's only duty is to follow along attentively.

## Enforced

- **One meaningful, achievable end-to-end project.**
- **Open with** "In this tutorial we will \<accomplish concrete thing\>" —
  never "you will learn".
- **A single line.** No options or alternatives.
- **Every step yields a visible, comprehensible result**, however small.
- **Show exact expected output.**
- **Maintain a narrative of the expected:** "You will notice…", "The output
  should look like…", "If the output doesn't show X, you probably forgot Y."
- **Prompt observation:** "Notice that…"
- **First-person plural throughout** — "we", not "you".
- **Permit repetition of steps** wherever possible.
- **Minimal explanation** — one clause plus a link out: "We use HTTPS because
  it's safer — see About transport security."
- **Concrete and particular, never abstract.**

## Forbidden

- "You will learn…"
- Choices ("you could also…")
- Explanation beyond a single clause
- Assuming knowledge a newcomer lacks — be explicit about where to type
  things, what to wait for.

## Execution verification protocol

Tutorials are verified by execution, wherever the environment allows it.
Running each command and capturing real output is the only honest basis for
the perfect reliability a tutorial must promise — do not assert that
something works without having run it.

- **Sandbox every run.** Execute commands only in a disposable copy of the
  repo — `git worktree add` or a temp-dir clone — never in the user's working
  tree.
- **Never run commands with external side effects:** deploys, pushes,
  publishes, migrations against real databases, anything touching remote
  services or credentials. Mark those steps **unverified** in the plan file.
- **Capture and normalize output.** Capture real output; normalize paths,
  timestamps, and hostnames before presenting it as expected output.
- **Fall back honestly.** If execution is impossible in the environment, mark
  the whole tutorial **unverified** in the plan file. Never imply
  verification that did not happen.

## Closing self-check

Run the compass check from `references/compass.md`, section by section as
well as whole-tutorial: it must land in informs-action /
acquisition-of-skill. Then ask: does every step show its expected result?
Is there any fork? A fork is a failure.
