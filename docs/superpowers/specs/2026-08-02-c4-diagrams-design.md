# C4 architecture diagrams in diataxis-docs — design

Date: 2026-08-02
Status: approved in brainstorming; spec for implementation planning

## Goal

Extend the `diataxis-docs` skill so that a full run (or an
explanation-quadrant run) derives C4 **System Context** and **Container**
diagrams from the codebase and embeds them, as **Mermaid C4 syntax**, in a
single explanation page — *About the architecture* — written during the
explanation phase under that quadrant's normal rules.

## Decisions made

| Decision | Choice | Rationale |
|---|---|---|
| Scope | Skill generates diagrams during a run | Placement-only rules would leave the main value (derivation from code) to the user |
| C4 levels | Context + Container only | The maintainable levels; C4 practice (Brown) recommends most teams stop at Container |
| Build vs reuse | Native, self-contained | Existing C4 skills (bitsmuggler/c4-skill, cheriftj/c4-model-skill) generate standalone artifacts and know nothing of Diátaxis placement or the plan/approval workflow; an external-skill dependency breaks the self-contained install story |
| Quadrant placement | All diagrams in one explanation page | A C4 diagram read with its narrative is understanding-oriented; splitting Context/Container across quadrants fragments one story |
| Format | Mermaid C4 syntax (`C4Context` / `C4Container`) | Renders natively on GitHub, MkDocs Material, Docusaurus; diffs cleanly; no toolchain. Syntax is experimental in Mermaid — keep to the stable subset |
| Wiring | New rule sheet `references/diagrams.md` | Keeps the CC BY-SA Procida adaptations untouched; C4 material derives from c4model.com (CC BY 4.0) and needs its own attribution header |

## New file: `references/diagrams.md`

A rule sheet in the house style (attribution header, Purpose, Enforced,
Forbidden, Closing self-check). Rules:

### Enforced

- **Levels: Context and Container only.** If the user asks for Component or
  Code level, record a not-created entry in the plan with reason ("below the
  maintainable line; drifts from code faster than reruns occur") and remedy
  ("maintain by hand outside the generated set, or use IDE tooling").
- **Evidence rule.** Every element in a diagram traces to evidence found in
  Phase 0: a dependency, a config file, a deploy manifest (docker-compose,
  k8s, Procfile), an API client, a documented user role. No invented boxes,
  no assumed external systems — the diagram equivalent of reference's "no
  invented API". While drafting, note the evidence source for each element
  in the plan file, not in the published page (mirrors the reference sheet's
  file:line practice).
- **Thin-repo rule.** A codebase with a single deployable unit gets a
  Context diagram only; its Container diagram is recorded as not created,
  with reason ("one container — the Context diagram already shows it") and
  remedy ("becomes creatable if the system splits into multiple deployable
  units").
- **C4 notation rules.** Every element carries name + technology + one-line
  responsibility. Every relationship is a labeled, directed arrow. No mixed
  abstraction levels in one diagram. Diagram titles say what they show
  (implicit-*About* test applies to the page title; diagram titles name the
  level and system, e.g. "System Context — <system>").
- **Register rule.** Diagrams are embedded in explanation prose. The page
  must pass the explanation sheet's closing self-check (compass + bath
  test). A diagram dropped in with no surrounding narrative fails the
  self-check — the diagram illustrates the *why*; the prose carries it.
- **Mermaid specifics.** Fenced ```mermaid blocks using `C4Context` /
  `C4Container`. Keep to the stable subset: `Person`, `System`,
  `System_Ext`, `Container`, `ContainerDb`, `System_Boundary`,
  `Container_Boundary`, `Rel`, `Rel_Back`, `title`. No `UpdateLayoutConfig`
  or other layout directives that render inconsistently across engines.

### Forbidden

- Component and Code level diagrams (see Levels rule — not-created entry,
  never silently drawn).
- Elements without evidence.
- Deployment/dynamic/landscape diagram types (out of scope; a future run
  can revisit if evidence and need appear).
- Replacing existing hand-drawn architecture diagrams without plan
  approval (see Existing diagrams below).

### Closing self-check

- Every element traces to recorded evidence.
- Diagrams render (mentally walk the Mermaid syntax; on a rendering-capable
  host, verify).
- The page as a whole still passes the explanation sheet's own self-check.

## SKILL.md changes

Each a few lines, in existing sections:

- **Phase 0 (survey).** Add an architecture-evidence harvest step:
  deployable units, external systems, actors — recording where each was
  found (deploy manifests, service entry points, external API clients,
  documented user roles).
- **Phase 1 (plan).** The architecture explanation page is listed like any
  proposed document (title / need / source), with the diagram levels named
  as part of the entry. Approval covers it. A level lacking evidence gets a
  not-created entry with reason and remedy, same as a quadrant.
- **Explanation phase (Phases 2–5).** Load `references/diagrams.md` in
  addition to `references/explanation.md` **only when** the plan includes
  the architecture page. (Preserves the "load only the current phase's
  sheet" discipline.)
- **Rerun semantics.** Nothing new: the plan file records the architecture
  page like any document; a rerun re-verifies diagram elements against
  current evidence the same way Phase 6 re-checks standing decisions.

## Edge cases

- **Pure library, no deployable structure.** Context diagram still usually
  works (the library, its consumers, its external dependencies). If even
  that lacks evidence, the whole architecture page is a not-created entry
  with reason and remedy.
- **Existing hand-drawn architecture diagrams found in Phase 0.** Classified
  by the compass like any content and annotated keep/move/split in the
  plan. The skill never deletes them in favor of generated ones — replacing
  a hand-drawn diagram is a plan item the user approves explicitly.
- **Mermaid-hostile output targets.** If Phase 0's docs-tooling detection
  finds a generator that cannot render Mermaid, note it in the plan and
  propose the fallback (plain fenced code block with a rendering note) as
  an approvable item rather than silently degrading.

## Testing

Rerun the skill on a repo with genuine multi-container structure (the
`ultima_db` validation repo, if it fits) and confirm:

1. Diagrams render on GitHub.
2. Every element traces to evidence recorded in the plan file.
3. The architecture page passes the explanation sheet's closing self-check.
4. A single-unit repo correctly declines the Container diagram with a
   not-created entry.

## Licensing

`references/diagrams.md` gets its own header: original rule-sheet text;
C4 model concepts from [c4model.com](https://c4model.com), © Simon Brown,
licensed CC BY 4.0, attribution noted. The README licensing section gains
one line for the C4 attribution. SKILL.md's licensing paragraph is updated
to mention the diagrams sheet's distinct provenance.

## Out of scope

- Component/Code/Deployment/Dynamic/Landscape diagrams.
- Structurizr DSL or PlantUML output.
- Delegating to or detecting third-party C4 skills.
- Rendering diagrams to PNG/SVG.
