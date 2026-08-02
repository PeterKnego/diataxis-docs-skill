# The plan file: `diataxis-plan.md`

The plan file is the working and persistent artifact of a diataxis-docs run.
The skill writes it at the repo root during Phase 1, presents it for approval
before any documentation is written, updates it during Phases 2–5, and trims
and commits it at the end of Phase 6. On later runs, Phase 0 reads the
committed file as established context.

Location: repo root. The file must not be placed inside a published docs
tree; site generators must never render it.

## Sections

Sections appear in this order. Sections marked *transient* exist only during
a run and are removed by the Phase 6 trim; sections marked *persistent*
survive between runs.

### Header — *persistent*

Run date and the detected output location (docs tooling and format fixed in
Phase 0).

### Proposed documents — *persistent*

One table per quadrant. Each row carries three fields:

- **Document** — output path of the page.
- **Need served** — the user need this page answers.
- **Source material** — where the content comes from.

The reference subsection additionally carries the proposed **scope**
statement. Scope is an approvable item in its own right: the user approves
it separately from the document list.

The explanation subsection names, for the *About the architecture* page,
each proposed C4 diagram level (System Context, Container). A level
without evidence behind it appears under not-created entries instead.

### Not-created entries — *persistent*

A quadrant with no genuine material is listed as **not created**. Each entry
carries two required fields:

- **Reason** — what material is missing.
- **Remedy** — what would make the quadrant creatable in a future run.

An entry without both fields cannot be approved.

### Existing docs — compass annotations — *transient*

One row per existing document or section, with its compass classification
and one of three actions:

- **keep** — left in place.
- **move** — relocated whole; the row names the destination quadrant and
  document.
- **split** — divided; the row names a destination for every part.

Applied annotations are removed by the Phase 6 trim.

### Batched questions and answers — *persistent*

The single round of user questions asked in Phase 1, with the user's
answers. Later runs do not re-ask a question answered here unless current
evidence contradicts the recorded answer.

### Diagram evidence notes — *transient*

One note per diagram element while the architecture page is drafted:
which harvested evidence (deploy manifest, dependency, API client,
documented role) the element traces to. Checked by the diagrams sheet's
closing self-check; removed by the Phase 6 trim.

### Parking lot — *transient*

Content that surfaced during one quadrant's phase but belongs to a quadrant
not yet written, tagged with its destination phase. The parking lot must be
empty when Phase 6 closes; the trim removes the section.

### Tutorial verification status — *persistent*

One entry per tutorial: **verified** (commands executed in a disposable
copy, output captured) or **unverified**, with the reason. Unverified
markers remain until discharged by a later verified run.

## Lifecycle

| Phase | Operation on the plan file |
|---|---|
| Phase 0 | Read, if present from a prior run |
| Phase 1 | Written or updated; presented for approval |
| Phases 2–5 | Parking lot and diagram evidence notes appended; verification notes recorded |
| Phase 6 | Trimmed of transient sections; committed with the docs |

A committed plan file passes the Phase 0 clean-tree gate. An uncommitted,
modified plan file fails it and blocks the next run.
