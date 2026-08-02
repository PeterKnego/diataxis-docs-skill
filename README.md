# diataxis-docs

Design work for a Claude Code skill that generates a Diátaxis-shaped
documentation set (tutorials, how-to guides, reference, explanation) from a
codebase.

- Design spec: [docs/superpowers/specs/2026-08-02-diataxis-docs-skill-design.md](docs/superpowers/specs/2026-08-02-diataxis-docs-skill-design.md)
- Source corpus: [site/txt/](site/txt/) — the pages of diataxis.fr as extracted
  text, kept as provenance so the skill's rules can be traced to their source.

## Attribution and licensing

The files under [site/txt/](site/txt/) are extracted from
[diataxis.fr](https://diataxis.fr), copyright © **Daniele Procida**, and are
licensed under
[Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/).
The license of the original material is stated in the
[source repository](https://github.com/evildmp/diataxis-documentation-framework)
for the website.

Changes made: the text was extracted from the published HTML pages on
2026-08-02 and converted to Markdown; site formatting, navigation and images
were removed. Each file carries its own attribution header naming the exact
source page.

In accordance with the ShareAlike condition, the contents of `site/txt/` — and
any material in this repository derived from them, such as distilled rule
sheets — are likewise distributed under CC BY-SA 4.0.

To cite Diátaxis itself, refer to [diataxis.fr](https://diataxis.fr), per its
[colophon](https://diataxis.fr/colophon/).
