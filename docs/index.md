# diataxis-docs documentation

Documentation for the diataxis-docs Claude Code skill, organized by what
you need from it right now.

## Get it working

Practical directions for the two things every user does: getting the skill
into Claude Code, and pointing it at a codebase.

- [How to install and update the skill](how-to/install-and-update.md)
- [How to generate documentation for a repository](how-to/run-on-a-repo.md)

## Look something up

Technical description of the skill's one user-facing artifact — the plan
file you review at the approval gate.

- [The plan file: `diataxis-plan.md`](reference/plan-file.md)

## Understand the design

Background reading for when you are away from the keyboard: why the run is
gated, why quadrants are written in the order they are, why tutorials are
executed rather than asserted, and where the licensing split comes from.

- [About the design of this skill](explanation/design.md)

---

There is no tutorial section yet; the skill's own gated run doubles as the
guided first experience. The engineering history of the skill — design
spec and implementation plan — lives in
[docs/superpowers/](superpowers/), and the Diátaxis source corpus in
[site/txt/](../site/txt/).
