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
| Invocation | Explicit only — `/diataxis-docs` or asking for the skill by name |

## Invocation

The skill triggers only on explicit invocation: `/diataxis-docs`, or the user
naming it ("use the diataxis skill on this repo"). Its frontmatter description
states this restriction so it does not auto-fire on incidental documentation
requests ("add a note to the README" must never launch a seven-phase workflow).

A full run executes all phases. The user may also invoke a single quadrant
("`/diataxis-docs` reference only"); the skill still performs Phase 0 and a
scoped Phase 1 plan for that quadrant, then writes only it.

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

Verify the repo is under version control with a clean working tree; if not,
stop and offer `git init` / commit first. Later phases move and split
human-written files, and every such change must be revertable.

Detect docs tooling (`mkdocs.yml`, `conf.py`, `docusaurus.config.*`, bare
`docs/`) to fix the output format and location. Also detect API-reference
generators (Sphinx autodoc, typedoc, godoc, rustdoc and similar): where one
exists, the reference phase improves the docstrings and structure that feed it
rather than writing parallel pages that would drift from the generated output.

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

Write `diataxis-plan.md` outside the published docs tree (repo root, or the
scratchpad if the user prefers no artifact) so site generators never render it.
It contains, for every proposed document: its title, the user need it serves,
and the source material backing it. Existing docs are annotated
keep / move / split.

The plan also proposes the reference **scope** as its own approvable item —
e.g. public API only, or the top-level modules — since "document the product
surface" is unbounded on a large codebase. The user approves scope, not just
the document list.

Then ask the user one batched round of questions covering only what the repo
cannot answer:

- who the audiences are
- their top real-world goals
- which decisions carry rationale worth explaining

The user approves the plan before any documentation is written.

A quadrant with no genuine material is not created at all — no directory, no
landing page — and the plan says so. An empty section is itself the forbidden
scaffold.

### Phases 2–5 — Write, one quadrant at a time

Order: reference → how-to → explanation → tutorial.

Reference goes first because it is the most reliably derivable and becomes the
link target that keeps the other three uncluttered. Tutorial goes last because
it is the most expensive to produce and can only be constructed once the writer
knows what reference and how-to material exists to link out to.

Each quadrant is written with its rule sheet loaded, and runs a compass
self-check before closing. Content that belongs in a quadrant not yet written
(e.g. explanation surfacing during the reference phase) is parked in the plan
file and picked up when its phase arrives.

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
honest basis for that claim.

Execution is sandboxed: commands run in a disposable copy of the repo (git
worktree or temp directory), never in the user's working tree. Commands with
external side effects — deploys, pushes, migrations against real databases,
anything touching remote services — are never run; those steps, and any
environment where execution is impossible, are explicitly marked unverified in
the plan rather than implying verification that did not happen. Captured output
is normalized (paths, timestamps, hostnames) before being shown as expected
output.

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

The full diataxis.fr corpus (18 pages) is committed under `site/txt/` as text
extracted from the published pages, retrieved 2026-08-02. The reference sheets
are distilled from this corpus, so the skill's rules can be traced back to their
source and re-derived if the site changes.
