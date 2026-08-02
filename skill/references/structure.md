> Adapted from [diataxis.fr](https://diataxis.fr) (https://diataxis.fr/complex-hierarchies/, https://diataxis.fr/how-to-use-diataxis/).
> Copyright © Daniele Procida. Licensed under
> [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
> Changes: distilled into rule-sheet form for the diataxis-docs
> Claude Code skill, 2026-08-02.

# Structure: hierarchy, landing pages, cross-linking

## Canonical layout

```
Home              <- landing page
Tutorial          <- landing page
    Part 1
    Part 2
    Part 3
How-to guides     <- landing page
    Install
    Deploy
    Scale
Reference         <- landing page
    Command-line tool
    Available endpoints
    API
Explanation       <- landing page
    Best practice recommendations
    Security overview
    Performance
```

Each of the four sections is a landing page containing an overview of what's
within it. The tutorial landing page describes what the tutorial offers and
gives it context; the same holds for how-to guides, reference, and
explanation.

## Landing pages are overviews, not lists

A landing page's content must read like an overview: it introduces the
material, it does not simply present a bare list of children. Headings and
short snippets of introductory text catch the eye and give context before
the links appear. Always write for the human reader, not to satisfy a
scheme.

## The list rule

Lists longer than about seven items are hard to read unless they have an
inherent mechanical order (numerical, alphabetical). If a table of contents
list runs longer than that, break it into smaller, headed sub-groups, each
introduced by a sentence of context. Example shape for a how-to landing page:

- a heading ("Installation guides") with a sentence of context, followed by
  the links it groups (Local installation, Docker, Virtual machines, Linux
  containers)
- a second heading ("Deployment and scaling") with its own sentence of
  context, followed by its links (Deploy an instance, Scale your
  application)

## Anti-scaffold rule

A quadrant with no content gets no directory and no landing page. Never
create empty tutorial/how-to/reference/explanation structures in advance of
having material for them — an empty scaffold serves no reader and should
not exist.

## Two-dimensional structures

When audiences or platforms split the docs — users vs. developers vs.
contributors, product-on-cloud-A vs. product-on-cloud-B — and those splits
are effectively different products, structure by audience or platform
first, then apply Diátaxis ordering within each. Diátaxis is not a scheme
that forces exactly four top-level divisions; it identifies four kinds of
documentation and structures around them, which can nest under another
axis when the underlying concerns are genuinely different products.
Complexity here is acceptable, and even necessary, as long as the
arrangement still follows Diátaxis principles within each branch — that is
what keeps it navigable.

## Cross-linking rule

Linking out instead of digressing is the mechanism that keeps the four
quadrants apart. Every "forbidden — link out instead" redirect named in the
other rule sheets (exhaustive option lists from how-to guides, explanation
from reference, teaching from how-to guides, and so on) must land on a real
link target that this structure wires up. A quadrant boundary that isn't
backed by a working cross-link is just a place where content gets lost.

## Deeper hierarchy over forked pages

When one page would otherwise have to cover multiple forks (e.g. install
instructions that differ by platform), add a layer of hierarchy instead of
letting the page carry the fork itself — turn the single page into a
landing page with per-fork children:

```
How-to guides     <- landing page
    Install       <- landing page
        Local installation
        Docker
        Virtual machine
        Linux container
    Deploy
    Scale
```

Even large documentation sets stay navigable this way, as long as the added
layer is itself a proper landing page (overview, not a bare list) and the
grouping follows Diátaxis principles.
