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
   --porcelain` empty). If either check fails, STOP and offer to run `git
   init` or to commit/stash outstanding changes first. Do not proceed until
   the tree is clean — later phases move and split human-written files, and
   every such change must be revertable.
2. **Docs tooling detection.** Look for `mkdocs.yml`, `conf.py`,
   `docusaurus.config.*`, or a bare `docs/` directory. Whatever is found
   fixes the output format and location for everything written in Phases
   2–6; absent any of these, fall back to Markdown under `docs/`.
3. **Autodoc detection.** Look for Sphinx autodoc config, `typedoc.json`,
   godoc conventions, `cargo doc` / rustdoc setup, or similar. Where one is
   found, it sets the reference-phase mode — see the Autodoc rule in
   `references/reference.md` before writing reference material.
4. **Product-surface mapping.** Identify entry points, public exports, CLI
   commands, config options, and module structure.
5. **Need-evidence harvest.** Read README, CHANGELOG, ADRs, design docs,
   `examples/`, integration and end-to-end tests (the best available source
   of real user goals), commit history, PR/issue titles via `gh` when
   available, and docstrings/inline comments.
6. **Classify existing docs.** Load `references/compass.md` and run every
   existing piece of documentation through it. Record each piece's quadrant
   (or "misclassified" / "blurred") for use in Phase 1.

## Phase 1 — Plan and gap confirmation

Write `diataxis-plan.md` at the repo root, outside any published docs tree,
so site generators never render it. For every proposed document, record:

- title
- the user need it serves
- the source material backing it

Annotate every existing doc from the Phase 0 compass pass as **keep**,
**move**, or **split**. List the proposed reference **scope** (e.g. public
API only, or top-level modules) as its own approvable item — "document the
product surface" is unbounded on a large codebase, and the user approves
scope, not just the document list. A quadrant with no genuine material
backing it is listed as **not created** — no directory, no landing page, no
placeholder. An empty section is itself the forbidden scaffold.

Then ask the user **one batched round** of questions, covering only what the
repo could not answer: who the audiences are, their top real-world goals, and
which decisions carry rationale worth explaining.

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
