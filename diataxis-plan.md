# Diátaxis plan — diataxis-docs-skill repo

Last full run: 2026-08-02. Output location: Markdown under `docs/`
(no generator config found; `docs/superpowers/` is engineering provenance,
not user documentation, and is left untouched).

## Documents

### Reference — approved scope

**Scope: one page — the `diataxis-plan.md` file format.** The skill's
phases, gates and guards are deliberately NOT documented in a parallel
reference page: SKILL.md is the machinery's own authoritative, shipped
description, and a hand-written copy would drift (the skill's own autodoc
rule, applied to itself).

| Document | Need served | Source material |
|---|---|---|
| `docs/reference/plan-file.md` | A user reviewing the plan at the approval gate needs to know what each section means and what they are approving | SKILL.md Phases 1 and 6 |

### How-to guides

| Document | Need served | Source material |
|---|---|---|
| `docs/how-to/install-and-update.md` | Get the skill installed (plugin or clone route) and keep it updated | Root README, skill README, plugin manifests |
| `docs/how-to/run-on-a-repo.md` | Run the skill on a real repo: prepare the tree, pass the gate, review the plan, approve scope, single-quadrant runs, reruns | SKILL.md workflow, dry-run experience in commit history |

### Explanation

| Document | Need served | Source material |
|---|---|---|
| `docs/explanation/design.md` | Understand why the skill works the way it does: the central tension, gated approval, reference-first order, plan-file persistence, execution verification, licensing split | Design spec, commit history |

### Tutorials — not created

- **Reason:** no study-oriented need in evidence. The audience is Claude
  Code users already competent at invoking skills; the product's own gated
  run is itself the guided first experience. Additionally, a tutorial's
  reliability cannot be verified by execution here — an end-to-end run
  requires an interactive session with an approval gate.
- **Remedy:** if learner-audience evidence appears (e.g. issues asking for
  a guided first walkthrough), build a lesson around a small bundled sample
  repo and verify it by running the walkthrough in a disposable session
  before shipping.

## Audiences, goals, rationale (answered 2026-08-02)

1. **Audiences: users only.** Claude Code users who install and run the
   skill. Contributor docs deferred until there are contributors; the spec
   and commit history serve that need meanwhile.
2. **Top goals: install-and-update and run-on-a-repo.** Single-quadrant use
   is a section inside run-on-a-repo, not a separate guide.
3. **Explanation: all six decisions, one bounded page** — central tension,
   gated approval, reference-first order, plan-file persistence, execution
   verification, licensing split.

## Tutorial verification status

(no tutorials — see not-created entry)
