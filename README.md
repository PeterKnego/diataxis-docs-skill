# Diataxis Documentation Skill

A Claude Code skill that generates a Diátaxis-shaped documentation set
(tutorials, how-to guides, reference, explanation) from a codebase.

- Design spec: [docs/superpowers/specs/2026-08-02-diataxis-docs-skill-design.md](docs/superpowers/specs/2026-08-02-diataxis-docs-skill-design.md)
- Source corpus: [site/txt/](site/txt/) — the pages of diataxis.fr as extracted
  text, kept as provenance so the skill's rules can be traced to their source.
- Skill: [skills/diataxis-docs/](skills/diataxis-docs/) — the installable
  skill itself. The repo is a Claude Code plugin marketplace; install in
  Claude Code with:

  ```
  /plugin marketplace add PeterKnego/diataxis-docs-skill
  /plugin install diataxis-docs@diataxis-docs-skill
  ```

  For updates, the clone-based install, and verification, see
  [How to install and update the skill](docs/how-to/install-and-update.md).

## Documentation

Full documentation lives at [docs/](docs/index.md):

- [How-to guides](docs/how-to/install-and-update.md) — install and update
  the skill, and run it against a repository.
- [Reference](docs/reference/plan-file.md) — the `diataxis-plan.md` file
  format you review at the approval gate.
- [Explanation](docs/explanation/design.md) — why the skill is gated,
  ordered, and licensed the way it is.

## Licensing and attribution

This repository contains material under two licenses:

- **Original work** — the skill design, specs, and everything outside
  `site/txt/` and `skills/diataxis-docs/references/` — is copyright
  © Peter Knego and licensed under the [MIT License](LICENSE).
- **Diátaxis material** — the extracted pages under [site/txt/](site/txt/)
  and the rule sheets adapted from them under
  [skills/diataxis-docs/references/](skills/diataxis-docs/references/) — is
  copyright © Daniele Procida and licensed under
  [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/), as stated
  in the website's
  [source repository](https://github.com/evildmp/diataxis-documentation-framework).

Changes made to the Diátaxis material: extracted from the published
[diataxis.fr](https://diataxis.fr) pages on 2026-08-02, converted to
Markdown (formatting, navigation and images removed), and distilled into
rule-sheet form for the skill. Each file carries an attribution header
naming its exact source pages.

To cite Diátaxis itself, refer to [diataxis.fr](https://diataxis.fr), per
its [colophon](https://diataxis.fr/colophon/).
