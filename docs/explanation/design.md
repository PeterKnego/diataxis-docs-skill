# About the design of this skill

Why does a documentation generator refuse to generate documentation until a
human has approved a plan? Why does it write reference pages first and
tutorials last — and often decline to write tutorials at all? This page
explains the reasoning behind the skill's design, which will otherwise seem
needlessly cautious for a tool whose whole point is automation.

## The central tension

The skill exists inside a contradiction. Diátaxis holds that three of the
four kinds of documentation are led by *user needs* — what people are trying
to learn, or get done, or understand. Only reference is led by the product
itself. But a code-reading tool has direct access only to the product. The
needs are not in the source.

Naive generation resolves this the wrong way: it derives everything from
code, which produces exactly the failure Diátaxis warns about most loudly —
how-to guides that are really tool inventories ("to deploy the
configuration, press Deploy"), addressed to no human project at all. The
skill instead treats the repository as *evidence* of needs — examples,
end-to-end tests, issues, changelogs are all traces of what people actually
do — and asks the user to fill only the gaps the evidence leaves. The
batched question round is not politeness; it is the mechanism by which the
three need-led quadrants get their needs.

## Why the run is gated

The approval gate exists because the two failure modes of generated
documentation are both cheaper to prevent than to undo. One is scope
explosion: "document the API" on a large codebase is unbounded work
producing hundreds of pages nobody asked for. The other is destructive
reorganization: the skill moves and splits existing human-written prose,
and a wrong guess about where content belongs is much easier to veto in a
plan table than to reverse across twenty files. The clean-tree requirement
serves the same end from below — every change the run makes is one
`git revert` from undone.

There is a cost: the run is not fire-and-forget. That trade was made
deliberately. A documentation set is a long-lived artifact; a minute at the
gate is small against years of maintaining pages that should never have
existed.

## Why reference comes first and tutorials last

The writing order — reference, how-to, explanation, tutorial — follows
dependency and cost, not importance.

Reference is the most reliably derivable quadrant (it is the one the code
actually contains) and, once written, it becomes the link target that keeps
every other quadrant clean. Diátaxis discipline depends on linking out
instead of digressing: a how-to guide can only say "see the full option
list in the reference" if the reference exists.

Tutorials are last for the mirror-image reason: they are the most expensive
to produce, the most fragile under product change, and they need the most
to link out to. A tutorial written first would have nowhere to send its
learner and would inevitably absorb the explanation and reference material
that belongs elsewhere.

## Why the plan file persists

The plan file began as a scratch artifact and became the skill's memory.
The reasoning: the durable knowledge a run acquires — who the audiences
are, what they need to get done, which decisions deserve explanation — is
precisely the knowledge that is *not* in the code and therefore cannot be
re-derived next time. Deleting the plan would mean re-interviewing the user
on every run. So the run ends by trimming the file to its durable parts and
committing it: migration annotations die with the run, but needs, scope,
and the reasoning behind not-created quadrants survive to inform the next
one. A side effect is practical: a committed plan passes the clean-tree
gate, so runs compose.

## Why tutorials are verified by execution

Diátaxis demands "perfect reliability" of tutorials: a learner who follows
the steps and doesn't get the promised result loses confidence in the
tutorial, the product, and themselves. A language model asserting "the
output should look like this" is not reliability — it is plausibility. The
only honest ground for the claim is having run the commands. So the skill
executes tutorial steps in a disposable copy of the repo and captures real
output; what it cannot safely run (deploys, migrations, anything touching
remote services) it marks *unverified* rather than implying a verification
that never happened. Some tutorials therefore ship with visible caveats.
That is by design: an honest caveat is recoverable, a false promise is not.

## Why the licensing is split

The skill's rule sheets are distilled from diataxis.fr, whose content
Daniele Procida licenses under CC BY-SA 4.0. ShareAlike means adaptations
must carry the same license — so the sheets do, each with an attribution
header. The workflow logic, which is original work, is MIT like most of the
Claude Code skill ecosystem. The split is unusual in a code repo, but the
alternatives were worse: licensing everything CC BY-SA would burden the
code with a license not designed for code, and licensing everything MIT
would simply violate the source's terms. The lesson generalizes: a skill
built on someone else's methodology inherits that methodology's license
wherever it reproduces the methodology's text.
