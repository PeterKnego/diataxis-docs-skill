# How to generate documentation for a repository

This guide shows you how to run diataxis-docs against a codebase and steer
it to a committed documentation set. It assumes the skill is installed and
you have a Claude Code session open in the target repository.

## Prepare the repository

The skill refuses to start on an unversioned or dirty tree, so it can
guarantee every move of existing content is revertable.

- If the repo is not under git, let the skill run `git init` and an initial
  commit when it offers, or do both yourself first.
- If the tree is dirty, commit or stash before invoking.

If you want better output, improve the evidence before the run: the skill
mines README, CHANGELOG, ADRs, `examples/`, integration tests, and issue
titles for user goals. A repo with an `examples/` directory or end-to-end
tests yields how-to guides grounded in real workflows rather than in
guesses.

## Start the run

```
/diataxis-docs
```

The skill surveys the repo read-only, then writes `diataxis-plan.md` at the
repo root and stops. Nothing else is written until you approve.

## Answer the questions

Expect one batched round covering what the repo could not tell the skill:
who the documentation is for, what those users need to get done, and which
design decisions deserve explanation. Answer concretely — these answers
decide which documents get written, and they persist in the plan file so
future runs will not re-ask them.

## Review the plan before approving

Read `diataxis-plan.md` (see the [plan file reference](../reference/plan-file.md)
for what each section means). Check three things in particular:

- **Reference scope.** On a large codebase, "document the API" is unbounded.
  If the proposed scope is too wide, narrow it in your reply — e.g. "public
  API only" or "just the top-level modules".
- **Split and move annotations.** The skill relocates existing doc content it
  classifies as misplaced. Every move and split names its destination; if
  you want something left alone, say so now.
- **Not-created quadrants.** Each names what evidence is missing and what
  would remedy it. If you disagree — you know a learner audience exists,
  say — supply the missing context and ask for a revised plan.

Approve explicitly. The skill treats anything less than a clear yes as
not-approved and will ask again.

## During and after the run

Quadrants are written one at a time — reference first, tutorial last. If a
tutorial is produced, its commands are executed in a disposable copy of your
repo, never your working tree; steps that cannot be safely executed are
marked unverified in the plan file rather than silently trusted.

The run ends with the generated docs and a trimmed `diataxis-plan.md`
committed together. Review the commit as you would any other.

## Run a single quadrant

If you only need one kind of documentation, scope the invocation:

```
/diataxis-docs reference only
```

Survey and a scoped plan still happen — approval too — but only that
quadrant is written. Use this for incremental adoption: reference first on
one run, how-to guides on a later one, letting each run read the committed
plan from the last.

## Rerun after the code changes

Invoke the skill again. It reads the committed `diataxis-plan.md` from the
previous run, keeps your recorded answers and scope, and proposes only the
delta. Your job at the gate is the same: review the revised plan, approve
explicitly.
