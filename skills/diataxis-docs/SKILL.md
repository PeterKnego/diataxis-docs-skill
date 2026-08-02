---
name: diataxis-docs
description: Generate a complete Diátaxis documentation set (tutorials, how-to guides, reference, explanation) from a codebase. Use ONLY when the user explicitly invokes /diataxis-docs or asks for this skill by name — never for incidental documentation requests like editing a README.
---

# diataxis-docs

Generate a Diátaxis-shaped documentation set from a codebase: tutorials,
how-to guides, reference and explanation. Runs as a gated, phased workflow —
survey, then plan and get approval, then write one quadrant at a time, then
assemble.

## Invocation modes

- **Full run** — all phases, all four quadrants.
- **Single quadrant** — e.g. "reference only". Still run Phase 0 (survey) and
  a Phase 1 plan scoped to that quadrant. Write only the requested quadrant.

Never self-trigger. Only act on explicit invocation (`/diataxis-docs`, or the
user naming the skill).

## Phase 0 — Survey (read-only)

Do not write or move any file in this phase.

1. **Clean-tree gate.** Confirm the repo is under git (`git rev-parse
   --is-inside-work-tree`) with a clean working tree (`git status
   --porcelain` empty). If the repo is not under git, STOP and offer to run
   `git init` *and* make an initial commit — `git init` alone leaves every
   file untracked and still fails this gate. If the tree is dirty, STOP and
   offer to commit or stash the outstanding changes. Do not proceed until
   the tree is clean — later phases move and split human-written files, and
   every such change must be revertable.
2. **Prior plan.** If `diataxis-plan.md` exists at the repo root, read it.
   It is the durable record of previous runs: audiences, real-world goals,
   decisions worth explaining, approved reference scope, not-created
   quadrant reasoning, and tutorial verification status. Treat its contents
   as established context — Phase 1 updates this file rather than starting
   blank, and re-asks only what the current run's evidence contradicts.
3. **Docs tooling detection.** Look in the repo root and in `docs/` for
   `mkdocs.yml`, `conf.py`, `docusaurus.config.*`, or a `docs/` directory
   with no generator config beside it. Whatever is found fixes the output
   format and location for everything written in Phases 2–6; if a generator
   config and a bare `docs/` are both present, the generator config wins.
   Absent any of these, fall back to Markdown under `docs/`.
4. **Autodoc detection.** Look for Sphinx autodoc config, `typedoc.json`,
   godoc conventions, `cargo doc` / rustdoc setup, or similar. Where one is
   found, it sets the reference-phase mode — see the Autodoc rule in
   `references/reference.md` before writing reference material.
5. **Product-surface mapping.** Identify entry points, public exports, CLI
   commands, config options, and module structure.
6. **Need-evidence harvest.** Read README, CHANGELOG, ADRs, design docs,
   `examples/`, integration and end-to-end tests (the best available source
   of real user goals), commit history, PR/issue titles via `gh` when the
   repo has a GitHub remote and `gh` is authenticated (skip this source
   otherwise), and docstrings/inline comments.
7. **Classify existing docs.** Load `references/compass.md` and run every
   existing piece of documentation through it — section by section as well
   as whole-document, since a document can pass at the wide view and still
   fail at the close view. Record the quadrant of each document *and of each
   of its sections* (or "misclassified" / "blurred") for use in Phase 1.

## Phase 1 — Plan and gap confirmation

Write `diataxis-plan.md` at the repo root, outside any published docs tree,
so site generators never render it. If a prior plan exists (Phase 0), update
it in place — keep its durable answers, add this run's proposals — instead of
starting blank. For every proposed document, record:

- title
- the user need it serves
- the source material backing it

Annotate every existing doc from the Phase 0 compass pass as **keep**,
**move**, or **split**, at the section granularity that pass recorded. For
every **move** and for every part of every **split**, name the destination
quadrant and the destination document — an annotation with no destination
cannot be approved and leaves the content with nowhere to go in Phases 2–6.
List the proposed reference **scope** (e.g. public
API only, or top-level modules) as its own approvable item — "document the
product surface" is unbounded on a large codebase, and the user approves
scope, not just the document list. A quadrant with no genuine material
backing it is listed as **not created** — no directory, no landing page, no
placeholder. An empty section is itself the forbidden scaffold. Every
**not created** entry must state (a) the reason — what material is missing
(e.g. "no user-goal evidence found: no examples/, no e2e tests, no issues"),
and (b) a remedy — what would make the quadrant creatable in a future run
(e.g. "add an examples/ script per supported workflow", "answer the audience
questions", "record ADRs for the decisions worth explaining"). A bare "not
created" with no reason and remedy cannot be approved.

Then ask the user **one batched round** of questions, covering only what the
repo could not answer: who the audiences are, their top real-world goals, and
which decisions carry rationale worth explaining. Answers already recorded in
a prior plan are not re-asked unless this run's evidence contradicts them.
Fold the answers back into
`diataxis-plan.md` before asking for approval — those answers decide which
documents exist, so the plan put up for approval must be the revised one, not
the draft written before the questions were asked.

**STOP. Do not write, move, or split any documentation file until the user
has explicitly approved the plan and the reference scope.** Presenting the
plan is not approval. Asking the batched questions is not approval. Silence
is not approval. A vague or partial reply ("looks fine", "sure") that does
not address the plan and scope is not approval — ask again. There is no
condition under which Phase 2 may begin without an explicit yes from the
user on this specific plan.

## Phases 2–5 — Write quadrants

Order: **reference → how-to → explanation → tutorial**. Reference goes first
because it is the most reliably derivable and becomes the link target that
keeps the other three uncluttered. Tutorial goes last because it is the most
expensive to produce and depends on knowing what reference and how-to
material already exists to link out to.

For a single-quadrant run, write only the requested quadrant. Link out to
other quadrants only where they already exist; if a linked-to quadrant does
not exist, omit the link rather than creating a placeholder for it.

Write each quadrant's pages into their own directory under the output
location fixed in Phase 0 (e.g. `docs/reference/`). Phase 6 adds the landing
pages and cross-links on top of that placement; it does not relocate pages.

For each quadrant, before writing:

- Load only that phase's rule sheet — never load a sheet for a quadrant not
  currently being written:
  - Reference: `references/reference.md`
  - How-to guides: `references/how-to-guides.md`
  - Explanation: `references/explanation.md`
  - Tutorials: `references/tutorials.md`

Before closing each phase:

1. Run the sheet's own closing self-check.
2. Park any content that belongs to a quadrant not yet written (e.g.
   explanation surfacing while writing reference) in `diataxis-plan.md`,
   tagged for its future phase. Do not write it into the current quadrant
   and do not discard it.

## Phase 6 — Assemble structure

Load `references/structure.md`. Write landing pages as overviews with
introductory prose, apply the list rule, and wire up cross-links between
quadrants per the sheet.

**Wire the README.** The repo's README is the de-facto documentation entry
point; generated docs that the README never mentions are invisible. If the
repo has a README, add — or update, on a rerun — a short documentation
section linking the docs landing page, with one sentence per quadrant
landing page that exists. Keep it an overview, not a copy of the landing
page; the README remains the repo's front door, not a docs mirror. Respect
any keep/split annotations already applied to the README in this run.

Then close the run by trimming and committing `diataxis-plan.md`:

- **Remove** what is now dead: applied keep/move/split annotations and the
  parking lot (which must be empty — unparked content means a phase closed
  incorrectly).
- **Keep** what future runs need: per-document title/need/source, the
  audience/goal/rationale answers, the approved reference scope, every
  not-created entry with its reason and remedy, and tutorial verification
  status (verified / unverified markers stay until discharged).
- **Commit** the trimmed plan together with the generated documentation.
  The Phase 0 clean-tree gate depends on this: a committed plan passes it;
  a leftover dirty plan blocks every future run.

## Guards

These are hard rules. They apply across every phase and every quadrant:

- No empty quadrant scaffolds.
- No how-to guides shaped by tool operations rather than user goals.
- No explanation inside tutorials.
- No explanation inside reference.
- No tutorial and how-to conflation.
- No reference lists inside how-to guides.
- No titles that do not say what the document does.

Misplaced content is always moved, never deleted.

## Licensing

This file (`SKILL.md`) is original work, copyright © Peter Knego, licensed
under the MIT License. The rule sheets under `references/` are adaptations
of [diataxis.fr](https://diataxis.fr) content, copyright © Daniele Procida,
licensed under CC BY-SA 4.0 — each carries its own attribution header.
