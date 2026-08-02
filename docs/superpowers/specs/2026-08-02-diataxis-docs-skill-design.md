# Design: `diataxis-docs` skill

Date: 2026-08-02

## Purpose

A Claude Code skill that generates a complete Diátaxis-shaped documentation set
from a codebase: tutorials, how-to guides, reference and explanation.

## Background and the central tension

Diátaxis (https://diataxis.fr) identifies four kinds of documentation, each
answering a distinct user need, positioned on two axes: action/cognition and
acquisition/application.

| | serves study (acquisition) | serves work (application) |
|---|---|---|
| **informs action** | tutorial | how-to guide |
| **informs cognition** | explanation | reference |

Generating this set from code runs into two documented objections, and the skill
is designed around them rather than in spite of them.

**Only reference is derivable from source.** Diátaxis: reference "is led by the
product it describes"; the other three are led by user needs. How-to guides in
particular must answer to a human project — a guide defined by "operations that
can be performed with a tool" is the named failure mode, and that is precisely
what naive code-derived generation produces.

**Diátaxis is anti-scaffolding.** It states that creating empty
tutorial/how-to/reference/explanation structures is "horrible" and prescribes
small iterative improvements over top-down planning.

The skill therefore mines the repository for need-evidence, confirms the
remaining gaps with the user before writing, and never creates a section it
cannot populate.

## Decisions

| Decision | Choice |
|---|---|
| Primary job | Generate a full doc set from a codebase |
| Need discovery | Mine the repo for evidence, then confirm gaps with the user |
| Run shape | Phased, with an approval checkpoint after planning |
| Existing docs | Absorb and reclassify — preserve human-written prose |
| Packaging | Personal skill at `~/.claude/skills/diataxis-docs/` with bundled reference sheets |
| Output format | Match detected docs tooling; fall back to Markdown under `docs/` |

## Structure

```
~/.claude/skills/diataxis-docs/
├── SKILL.md              # workflow, phase gates, anti-pattern guards
└── references/
    ├── compass.md        # the decision tool + blur diagnostics
    ├── reference.md      # per-quadrant rule sheets
    ├── how-to-guides.md
    ├── explanation.md
    ├── tutorials.md
    └── structure.md      # hierarchy, landing pages, contents-page craft
```

Rule sheets load on demand. A run writing reference material does not carry
tutorial pedagogy in context.

## Workflow

### Phase 0 — Survey (read-only)

Detect docs tooling (`mkdocs.yml`, `conf.py`, `docusaurus.config.*`, bare
`docs/`) to fix the output format and location.

Map the product surface: entry points, public exports, CLI commands, config
options, module structure.

Harvest need-evidence:

- README, CHANGELOG, ADRs, design docs
- `examples/` directory
- integration and end-to-end tests — the best available source of real user goals
- commit history
- PR and issue titles via `gh`, when available
- docstrings and inline comments

Inventory existing documentation and classify each piece with the compass.

### Phase 1 — Plan and gap confirmation

Write `diataxis-plan.md` at the root of the detected docs directory, containing,
for every proposed document: its
title, the user need it serves, and the source material backing it. Existing
docs are annotated keep / move / split.

Then ask the user one batched round of questions covering only what the repo
cannot answer:

- who the audiences are
- their top real-world goals
- which decisions carry rationale worth explaining

The user approves the plan before any documentation is written.

A quadrant with no genuine material stays empty, and the plan says so. No
scaffolds.

### Phases 2–5 — Write, one quadrant at a time

Order: reference → how-to → explanation → tutorial.

Reference goes first because it is the most reliably derivable and becomes the
link target that keeps the other three uncluttered. Tutorial goes last because
it is the most expensive to produce and can only be constructed once the writer
knows what reference and how-to material exists to link out to.

Each quadrant is written with its rule sheet loaded, and runs a compass
self-check before closing.

### Phase 6 — Assemble structure

Landing pages read as overviews with introductory prose, not bare lists. Lists
longer than about seven items are sub-grouped. Cross-links are wired up:
linking out instead of digressing is the mechanism that keeps the quadrants from
bleeding into each other.

## Per-quadrant rules

### Reference

Enforced: structure mirrors the code's structure; neutral description; a
consistent entry pattern across all entries; signatures, parameters, return
values, exceptions, options, flags, defaults, limits, error messages; every
claim verified against source.

Forbidden: instruction, explanation, opinion, invented API.

Examples are permitted as illustration but must not develop into explanation.

### How-to guides

Enforced: titles of the form *How to \<real-world goal\>*; each guide answers a
human project; conditional imperatives ("if you want x, do y"); branching where
the real world branches; assumed competence; starts and ends at reasonable
points rather than being exhaustive.

Forbidden: goals that amount to "call this function"; teaching; exhaustive
option lists — link to reference instead.

Title test: *How to integrate application performance monitoring* is good;
*Integrating application performance monitoring* is bad; *Application
performance monitoring* is very bad.

### Explanation

Enforced: titles that read with an implicit *About …*; why — design decisions,
history, technical constraints, trade-offs, alternatives, comparisons; opinion
and perspective are welcome; the topic is bounded.

Forbidden: step-by-step instructions; reference tables.

Sources: ADRs, commit messages, PR discussion, RFCs, design documents.

### Tutorials

Enforced: one meaningful, achievable end-to-end project; "in this tutorial we
will …"; a single line with no choices or alternatives; a visible, comprehensible
result at every step; exact expected output; "notice that …" prompts;
first-person plural; minimal explanation with links out.

Forbidden: "you will learn …"; options and alternatives; explanation beyond a
single clause.

**Tutorials are verified by execution.** Where the environment allows, the skill
runs each command and captures real output rather than asserting that it works.
Diátaxis demands perfect reliability of tutorials, and execution is the only
honest basis for that claim. Where commands cannot be run, the skill states this
in the plan rather than implying verification it did not perform.

## Guards

A compass self-check runs per document before each quadrant closes: *does this
inform action or cognition? does it serve acquisition or application?* Content
landing in the wrong quarter is moved, not deleted.

Explicitly blocked:

- empty quadrant scaffolds
- how-to guides shaped by tool operations rather than user goals
- explanation inside tutorials
- explanation inside reference
- tutorial and how-to conflation
- reference lists inside how-to guides
- titles that do not say what the document does

## Provenance

The full diataxis.fr corpus (18 pages) is committed under `site/` — `site/html/`
as retrieved, `site/txt/` as extracted text. The reference sheets are distilled
from this corpus, so the skill's rules can be traced back to their source and
re-derived if the site changes.
