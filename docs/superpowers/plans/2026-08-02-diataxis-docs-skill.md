# diataxis-docs Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the `diataxis-docs` Claude Code skill — SKILL.md plus six reference sheets — that generates a Diátaxis-shaped documentation set from a codebase, per the approved spec.

**Architecture:** The skill is authored in this repo under `skill/` (source of truth, public, git-tracked) and installed by symlinking `~/.claude/skills/diataxis-docs` to it. SKILL.md holds the phased workflow and guards; six sheets under `skill/references/` hold per-quadrant rules, distilled from the corpus in `site/txt/`, loaded only when their phase runs.

**Tech Stack:** Markdown with YAML frontmatter (Claude Code skill format). No code, no build step. Verification is by grep checks, a frontmatter parse, and a dry-run against a sample repo.

## Global Constraints

- Spec: `docs/superpowers/specs/2026-08-02-diataxis-docs-skill-design.md` — every rule below traces to it.
- Corpus: `site/txt/*.md` is the only source for Diátaxis rules. Do not invent rules; do not consult the live site.
- **Every file under `skill/references/` MUST begin with this exact header** (adjust only the source URL list per sheet):

  ```markdown
  > Adapted from [diataxis.fr](https://diataxis.fr) (<page URLs>).
  > Copyright © Daniele Procida. Licensed under
  > [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
  > Changes: distilled into rule-sheet form for the diataxis-docs
  > Claude Code skill, 2026-08-02.
  ```

- `skill/SKILL.md` and `skill/README.md` are © Peter Knego, MIT. No CC header on them.
- Sheets are working tools, not essays: each ≤ 120 lines including header.
- Explicit invocation only: the SKILL.md description must restrict triggering to `/diataxis-docs` or the user naming the skill.
- Commit after every task. Conventional-commit style messages as shown.

## File Structure

```
skill/
├── SKILL.md              # Task 7 — frontmatter, phased workflow, guards
├── README.md             # Task 8 — dual-license statement
└── references/
    ├── compass.md        # Task 1 — routing tool + blur diagnostics
    ├── reference.md      # Task 2 — reference quadrant rules
    ├── how-to-guides.md  # Task 3 — how-to quadrant rules
    ├── explanation.md    # Task 4 — explanation quadrant rules
    ├── tutorials.md      # Task 5 — tutorial rules + execution protocol
    └── structure.md      # Task 6 — hierarchy and landing-page rules
```

---

### Task 1: `skill/references/compass.md`

**Files:**
- Create: `skill/references/compass.md`
- Read (sources): `site/txt/compass.md`, `site/txt/map.md`, `site/txt/tutorials-how-to.md`, `site/txt/reference-explanation.md`, `site/txt/foundations.md`

**Interfaces:**
- Produces: the classification tool every other sheet's self-check refers to as "the compass check". SKILL.md (Task 7) instructs loading this file in Phase 0 and before closing each quadrant.

- [ ] **Step 1: Write the sheet.** Header URLs: `https://diataxis.fr/compass/`, `https://diataxis.fr/map/`. Required content, in order:
  1. The compass table verbatim in structure:

     | If the content… | …and serves the user's… | …then it must belong to |
     |---|---|---|
     | informs action | acquisition of skill | a tutorial |
     | informs action | application of skill | a how-to guide |
     | informs cognition | application of skill | reference |
     | informs cognition | acquisition of skill | explanation |

  2. Term glosses: action = practical steps, doing; cognition = theoretical knowledge, thinking; acquisition = study; application = work.
  3. The two questions to ask of any content, at sentence level and document level: *action or cognition? acquisition or application?*
  4. The map summary table (what each type does / question it answers / orientation / form): tutorials introduce–"Can you teach me to…?"–learning–a lesson; how-to guides guide–"How do I…?"–goals–a series of steps; reference states–"What is…?"–information–dry description; explanation explains–"Why…?"–understanding–discursive explanation.
  5. Blur diagnostics — the four adjacencies where types bleed, each with its telltale: tutorial↔how-to (both guide action; telltale: a "tutorial" assuming competence, or a guide teaching); how-to↔reference (both serve work; telltale: option tables inside guides); reference↔explanation (both propositional; telltale: examples growing into *why* digressions); explanation↔tutorial (both serve study; telltale: explanation crowding a lesson).
  6. Rules of thumb from the corpus: boring and unmemorable → probably reference; lists and tables → reference; readable in the bath → explanation; "would someone use this while working, or having stepped away?" as the work/study test.

- [ ] **Step 2: Verify.**
  ```bash
  head -6 skill/references/compass.md | grep -c "CC BY-SA 4.0"   # expect 1
  grep -c "informs action" skill/references/compass.md            # expect >= 2
  wc -l skill/references/compass.md                               # expect <= 120
  ```

- [ ] **Step 3: Commit.**
  ```bash
  git add skill/references/compass.md
  git commit -m "feat: add compass rule sheet"
  ```

### Task 2: `skill/references/reference.md`

**Files:**
- Create: `skill/references/reference.md`
- Read (sources): `site/txt/reference.md`, spec §Per-quadrant rules/Reference

**Interfaces:**
- Consumes: compass check from Task 1 (cite as "run the compass check from references/compass.md").
- Produces: the rule sheet SKILL.md loads in Phase 2.

- [ ] **Step 1: Write the sheet.** Header URL: `https://diataxis.fr/reference/`. Required content:
  1. Purpose line: reference is information-oriented; describe the machinery, succinctly, in an orderly way; led by the product, not the user.
  2. Enforced rules: structure mirrors code structure (module/class/method relationships appear identically in the docs); neutral description only; one consistent entry pattern for all entries (state the pattern: name → signature → parameters → returns → errors/exceptions → defaults/limits → example); every claim verified against source with file:line noted during drafting; examples permitted as illustration only.
  3. Forbidden: instruction ("to do X, …"), explanation (any *why*), opinion, invented or assumed API. Each with the redirect: link to the how-to or explanation instead.
  4. Language patterns (from corpus): state facts — "X inherits Y's defaults; defined in `path`"; list — "Sub-commands are: a, b, c"; warn — "You must use a. Never d."
  5. Autodoc rule from the spec: if Sphinx autodoc/typedoc/godoc/rustdoc exists, improve docstrings and the structure feeding that pipeline; never write parallel pages.
  6. Closing self-check: run the compass check from `references/compass.md` on each page — everything must land in informs-cognition/application-of-skill; move (not delete) anything that doesn't, parking it in the plan file.

- [ ] **Step 2: Verify.**
  ```bash
  head -6 skill/references/reference.md | grep -c "CC BY-SA 4.0"  # expect 1
  grep -ci "autodoc" skill/references/reference.md                # expect >= 1
  wc -l skill/references/reference.md                             # expect <= 120
  ```

- [ ] **Step 3: Commit.**
  ```bash
  git add skill/references/reference.md
  git commit -m "feat: add reference rule sheet"
  ```

### Task 3: `skill/references/how-to-guides.md`

**Files:**
- Create: `skill/references/how-to-guides.md`
- Read (sources): `site/txt/how-to-guides.md`, `site/txt/tutorials-how-to.md`, spec §Per-quadrant rules/How-to guides

**Interfaces:**
- Consumes: compass check from Task 1.
- Produces: the rule sheet SKILL.md loads in Phase 3.

- [ ] **Step 1: Write the sheet.** Header URLs: `https://diataxis.fr/how-to-guides/`, `https://diataxis.fr/tutorials-how-to/`. Required content:
  1. Purpose line: goal-oriented; serves the already-competent user at work; addresses a human project, never a tool operation.
  2. The named failure mode, quoted in substance: a guide defined by "operations that can be performed with a tool" serves no user need — "to deploy the desired configuration, press Deploy" is not guidance. Every guide must answer: what does a person need to get done in the real world?
  3. Enforced rules: title form *How to <real-world goal>*; conditional imperatives ("if you want x, do y"); branch where the real world branches (multiple entry/exit points allowed); assume competence — omit what any practitioner knows; start and end at reasonable points, completeness not required; sequence ordered for flow (minimize context-switching, don't leave thoughts open across steps).
  4. Forbidden: goals that amount to "call this function"; teaching or explanation (link out); exhaustive option lists (link to reference).
  5. Title test block: good *How to integrate application performance monitoring*; bad *Integrating application performance monitoring*; very bad *Application performance monitoring*.
  6. Tutorial/how-to boundary table (from tutorials-how-to): tutorial = contrived setting, single line, teacher responsible, eliminates the unexpected; how-to = real world, forks, user responsible, prepares for the unexpected.
  7. Closing self-check: compass check per guide — informs-action/application-of-skill; and per guide the question "what human project does this serve?" must have a non-tool answer.

- [ ] **Step 2: Verify.**
  ```bash
  head -6 skill/references/how-to-guides.md | grep -c "CC BY-SA 4.0"  # expect 1
  grep -c "How to " skill/references/how-to-guides.md                  # expect >= 2
  wc -l skill/references/how-to-guides.md                              # expect <= 120
  ```

- [ ] **Step 3: Commit.**
  ```bash
  git add skill/references/how-to-guides.md
  git commit -m "feat: add how-to rule sheet"
  ```

### Task 4: `skill/references/explanation.md`

**Files:**
- Create: `skill/references/explanation.md`
- Read (sources): `site/txt/explanation.md`, `site/txt/reference-explanation.md`, spec §Per-quadrant rules/Explanation

**Interfaces:**
- Consumes: compass check from Task 1.
- Produces: the rule sheet SKILL.md loads in Phase 4.

- [ ] **Step 1: Write the sheet.** Header URLs: `https://diataxis.fr/explanation/`, `https://diataxis.fr/reference-explanation/`. Required content:
  1. Purpose line: understanding-oriented; discursive treatment of a bounded topic; serves study away from the product; answers *why*.
  2. Enforced rules: titles pass the implicit-*About* test (*About user authentication*); use a real or imagined why-question as the prompt for each piece; cover design decisions, history, constraints, trade-offs, alternatives, comparisons; opinion and perspective are welcome — weigh contrary views; make connections beyond the immediate topic; bound the topic explicitly.
  3. Forbidden: step-by-step instructions; reference tables; letting the topic absorb how-to or reference material (link out instead).
  4. Language patterns: "The reason for x is historical: y…"; "W is better than z, because…"; "Some users prefer w (because z). This can be a good approach, but…".
  5. Source-mining note from the spec: draw material from ADRs, commit messages, PR discussion, RFCs, design documents.
  6. Closing self-check: compass check per page — informs-cognition/acquisition-of-skill; the bath test (would someone read this away from the keyboard?).

- [ ] **Step 2: Verify.**
  ```bash
  head -6 skill/references/explanation.md | grep -c "CC BY-SA 4.0"  # expect 1
  grep -ci "about " skill/references/explanation.md                 # expect >= 1
  wc -l skill/references/explanation.md                             # expect <= 120
  ```

- [ ] **Step 3: Commit.**
  ```bash
  git add skill/references/explanation.md
  git commit -m "feat: add explanation rule sheet"
  ```

### Task 5: `skill/references/tutorials.md`

**Files:**
- Create: `skill/references/tutorials.md`
- Read (sources): `site/txt/tutorials.md`, `site/txt/tutorials-how-to.md`, spec §Per-quadrant rules/Tutorials

**Interfaces:**
- Consumes: compass check from Task 1.
- Produces: the rule sheet SKILL.md loads in Phase 5, including the sandboxed execution protocol SKILL.md's Phase 5 gate depends on.

- [ ] **Step 1: Write the sheet.** Header URLs: `https://diataxis.fr/tutorials/`, `https://diataxis.fr/tutorials-how-to/`. Required content:
  1. Purpose line: learning-oriented; a lesson; the learner acquires skill by doing a meaningful project; the teacher bears all responsibility for success.
  2. Enforced rules: one meaningful, achievable end-to-end project; open with "In this tutorial we will <accomplish concrete thing>" — never "you will learn"; single line, no options or alternatives; every step yields a visible, comprehensible result; show exact expected output; maintain the narrative of the expected ("You will notice…", "The output should look like…", "If the output doesn't show X, you probably forgot Y"); prompt observation ("Notice that…"); first-person plural throughout; permit repetition of steps where possible; minimal explanation — one clause plus a link ("We use HTTPS because it's safer — see About transport security"); concrete and particular, never abstract.
  3. Forbidden: "you will learn…"; choices ("you could also…"); explanation beyond a single clause; assuming knowledge a newcomer lacks (be explicit about where to type things, what to wait for).
  4. **Execution verification protocol** (spec-mandated, verbatim in force):
     - Run every command in a disposable copy of the repo — `git worktree add` or a temp-dir clone — never the user's working tree.
     - Never run commands with external side effects: deploys, pushes, publishes, migrations against real databases, anything touching remote services or credentials. Mark those steps **unverified** in the plan file.
     - Capture real output; normalize paths, timestamps, hostnames before presenting as expected output.
     - If execution is impossible in the environment, mark the whole tutorial unverified in the plan file. Never imply verification that did not happen.
  5. Closing self-check: compass check — informs-action/acquisition-of-skill; plus: does every step show its expected result? is there any fork? (a fork is a failure).

- [ ] **Step 2: Verify.**
  ```bash
  head -6 skill/references/tutorials.md | grep -c "CC BY-SA 4.0"  # expect 1
  grep -ci "worktree" skill/references/tutorials.md               # expect >= 1
  grep -c "unverified" skill/references/tutorials.md              # expect >= 2
  wc -l skill/references/tutorials.md                             # expect <= 120
  ```

- [ ] **Step 3: Commit.**
  ```bash
  git add skill/references/tutorials.md
  git commit -m "feat: add tutorials rule sheet with execution protocol"
  ```

### Task 6: `skill/references/structure.md`

**Files:**
- Create: `skill/references/structure.md`
- Read (sources): `site/txt/complex-hierarchies.md`, `site/txt/how-to-use-diataxis.md`, spec §Phase 6

**Interfaces:**
- Consumes: nothing from other sheets.
- Produces: the rule sheet SKILL.md loads in Phase 6.

- [ ] **Step 1: Write the sheet.** Header URLs: `https://diataxis.fr/complex-hierarchies/`, `https://diataxis.fr/how-to-use-diataxis/`. Required content:
  1. Canonical layout example (Home / Tutorial / How-to guides / Reference / Explanation, each a landing page with children) — copy the indented example from the corpus.
  2. Landing pages read as overviews: introductory prose per group, then links; never a bare list of children.
  3. The list rule: lists longer than ~7 items get sub-grouped under headed sections with a sentence of context each.
  4. The anti-scaffold rule restated: a quadrant with no content gets no directory and no landing page.
  5. Two-dimensional structures: when audiences or platforms split the docs (users vs contributors, cloud A vs cloud B), structure by audience/platform first if they are effectively different products; Diátaxis ordering applies within each. Complexity is acceptable if arrangement follows Diátaxis principles.
  6. Cross-linking rule: linking out instead of digressing is the mechanism that keeps quadrants apart — every "forbidden" redirect in the other sheets lands on a link target wired here.
  7. Deeper hierarchy: add a layer (e.g. How-to guides → Install → per-platform pages) rather than letting one page cover forks.

- [ ] **Step 2: Verify.**
  ```bash
  head -6 skill/references/structure.md | grep -c "CC BY-SA 4.0"  # expect 1
  grep -ci "landing" skill/references/structure.md                # expect >= 2
  wc -l skill/references/structure.md                             # expect <= 120
  ```

- [ ] **Step 3: Commit.**
  ```bash
  git add skill/references/structure.md
  git commit -m "feat: add structure rule sheet"
  ```

### Task 7: `skill/SKILL.md`

**Files:**
- Create: `skill/SKILL.md`
- Read (sources): spec §§Invocation, Workflow, Guards; all six sheets (for exact filenames only)

**Interfaces:**
- Consumes: `references/compass.md`, `references/reference.md`, `references/how-to-guides.md`, `references/explanation.md`, `references/tutorials.md`, `references/structure.md` — loaded by relative path at the phases below.
- Produces: the installable skill entry point.

- [ ] **Step 1: Write SKILL.md.** Frontmatter exactly:

  ```yaml
  ---
  name: diataxis-docs
  description: Generate a complete Diátaxis documentation set (tutorials, how-to guides, reference, explanation) from a codebase. Use ONLY when the user explicitly invokes /diataxis-docs or asks for this skill by name — never for incidental documentation requests like editing a README.
  ---
  ```

  Body sections, in order (© Peter Knego, MIT — no CC header):
  1. **Invocation modes:** full run (all phases) or single quadrant ("reference only" etc. — still runs Phase 0 and a scoped Phase 1).
  2. **Phase 0 — Survey (read-only).** Gate: repo under git with clean working tree, else stop and offer `git init`/commit. Detect docs tooling (`mkdocs.yml`, `conf.py`, `docusaurus.config.*`, bare `docs/`) → fixes output format/location. Detect autodoc (Sphinx autodoc, typedoc, godoc, rustdoc) → sets the reference-phase mode per `references/reference.md`. Map product surface: entry points, public exports, CLI commands, config options, module structure. Harvest need-evidence: README, CHANGELOG, ADRs, `examples/`, integration/e2e tests (best source of real user goals), commit history, PR/issue titles via `gh` when available, docstrings. Load `references/compass.md`; classify every existing doc.
  3. **Phase 1 — Plan + gap confirmation.** Write `diataxis-plan.md` at repo root (outside any published docs tree): per proposed document — title, user need served, source material; existing docs annotated keep/move/split; reference **scope** as its own approvable item; quadrants with no material listed as "not created". Ask the user ONE batched round: audiences, top real-world goals, decisions worth explaining. STOP — user approves plan (and scope) before any writing.
  4. **Phases 2–5 — Write quadrants** in order reference → how-to → explanation → tutorial, loading only that phase's sheet. Before closing each phase: run the sheet's closing self-check; park misplaced content in `diataxis-plan.md` for its future phase.
  5. **Phase 6 — Assemble.** Load `references/structure.md`; write landing pages, wire cross-links, apply the list rule.
  6. **Guards (hard rules, verbatim from spec):** no empty quadrant scaffolds; no tool-shaped how-to guides; no explanation inside tutorials or reference; no tutorial/how-to conflation; no reference lists inside how-to guides; no titles that don't say what the document does; misplaced content is moved, never deleted.
  7. **Licensing note:** sheets under `references/` are CC BY-SA 4.0 adaptations of diataxis.fr (© Daniele Procida); this file is © Peter Knego, MIT.

- [ ] **Step 2: Verify frontmatter parses and files resolve.**
  ```bash
  python3 -c "
  import re, os
  t = open('skill/SKILL.md').read()
  fm = t.split('---')[1]
  assert re.search(r'^name: diataxis-docs$', fm, re.M)
  desc = re.search(r'^description: (.+)$', fm, re.M).group(1)
  assert 'ONLY' in desc and len(desc) < 1024
  refs = set(re.findall(r'references/[a-z-]+\.md', t))
  assert len(refs) == 6, refs
  assert all(os.path.exists('skill/'+r) for r in refs)
  print('OK')
  "
  ```
  Expected: `OK`

- [ ] **Step 3: Commit.**
  ```bash
  git add skill/SKILL.md
  git commit -m "feat: add diataxis-docs SKILL.md workflow"
  ```

### Task 8: Skill README, install symlink

**Files:**
- Create: `skill/README.md`
- Modify: `README.md` (repo root — add skill location + install line)

**Interfaces:**
- Consumes: license split as stated in repo README and spec §Licensing.

- [ ] **Step 1: Write `skill/README.md`.** Content: one-paragraph description; install instructions (`ln -s "$(pwd)/skill" ~/.claude/skills/diataxis-docs`); usage (`/diataxis-docs`, `/diataxis-docs reference only`); licensing section stating the split — SKILL.md and README © Peter Knego (MIT, see repo-root LICENSE), `references/` © Daniele Procida (CC BY-SA 4.0, headers in each file).

- [ ] **Step 2: Update repo-root `README.md`.** Under the description, add: the skill lives in `skill/`; install with the symlink command above.

- [ ] **Step 3: Install and verify.**
  ```bash
  ln -sfn "$(pwd)/skill" ~/.claude/skills/diataxis-docs
  ls -la ~/.claude/skills/diataxis-docs/SKILL.md   # resolves
  ```

- [ ] **Step 4: Commit.**
  ```bash
  git add skill/README.md README.md
  git commit -m "feat: add skill README and install instructions"
  ```

### Task 9: Dry-run verification on a sample repo

**Files:**
- Create: `<scratchpad>/sample-repo/` (throwaway — a ~5-file Python CLI: `pyproject.toml`, `src/greet/cli.py` with two subcommands, `src/greet/config.py` with three options, `tests/test_cli.py`, `README.md` with one usage paragraph)
- No repo files modified.

**Interfaces:**
- Consumes: the installed skill from Task 8.

- [ ] **Step 1: Build the sample repo** in the scratchpad with `git init`, one commit, the files above. Include one deliberately blurred doc: a README section mixing install steps with a paragraph of *why* (tests the compass classification and keep/move/split annotation).

- [ ] **Step 2: Execute Phases 0–1 by hand against it,** following SKILL.md literally as the checklist. Confirm: clean-tree gate fires on a dirty tree (touch a file, verify the stop; then clean); tooling detection reports "none → Markdown under docs/"; `diataxis-plan.md` lands at sample-repo root with need-per-document, scope item, keep/move/split for the README section, and at least one "not created" quadrant; the batched questions round is one round.

- [ ] **Step 3: Spot-check one quadrant.** Write the reference pages for the sample per `references/reference.md`; verify entry-pattern consistency and that the compass self-check catches the blurred README *why*-paragraph and parks it for explanation.

- [ ] **Step 4: Record findings.** Fix any SKILL.md/sheet wording that the dry run showed to be ambiguous or unexecutable. Delete the sample repo.

- [ ] **Step 5: Commit fixes (if any).**
  ```bash
  git add skill/
  git commit -m "fix: tighten skill wording after dry run"
  ```
