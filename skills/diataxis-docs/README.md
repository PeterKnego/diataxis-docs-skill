# diataxis-docs

A Claude Code skill that generates a Diátaxis-shaped documentation set —
tutorials, how-to guides, reference and explanation — from a codebase. It
runs as a gated, phased workflow: survey the repo, propose a plan and get
explicit approval, write one quadrant at a time against a distilled rule
sheet, then assemble the four quadrants into a linked structure.

## Install

As a plugin (recommended — gets you updates via the plugin manager). In
Claude Code:

```
/plugin marketplace add PeterKnego/diataxis-docs-skill
/plugin install diataxis-docs@diataxis-docs-skill
```

Update later with `/plugin marketplace update diataxis-docs-skill`.

Or as a bare personal skill — from a clone of the repository root:

```bash
ln -s "$(pwd)/skills/diataxis-docs" ~/.claude/skills/diataxis-docs
```

Update by pulling the clone.

## Usage

Invoke explicitly — the skill never self-triggers:

```
/diataxis-docs
```

Runs the full workflow across all four quadrants.

```
/diataxis-docs reference only
```

Scopes the run to a single quadrant (e.g. `reference`, `how-to`,
`explanation`, `tutorial`). Survey and planning still happen first, but only
the requested quadrant is written.

See [SKILL.md](SKILL.md) for the full phase-by-phase workflow and
[references/](references/) for the per-quadrant rule sheets.

## Licensing

This directory contains material under two licenses:

- `SKILL.md` and this `README.md` are original work, copyright © Peter
  Knego, licensed under the MIT License — see the repo-root
  [LICENSE](../../LICENSE).
- `references/` contains adaptations of [diataxis.fr](https://diataxis.fr)
  content, copyright © Daniele Procida, licensed under
  [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). Each file
  in `references/` carries its own attribution header.

For the full attribution and licensing detail, see the repo-root
[README.md](../../README.md).
