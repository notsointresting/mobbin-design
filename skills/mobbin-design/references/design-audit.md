# Design Audit — Working Against What Exists

Most of this skill assumes greenfield: research, lock a direction, build it. Most real work
is not that. It is a build that already exists, that nobody locked, and that needs to get
better without being thrown away.

This file is the protocol for that case. The order matters: **read the build before
researching**, because research done first anchors you to what you wish existed rather than
what is there, and the resulting critique is unusable.

## When To Use This

- "Make this page better" / "polish this" / "this feels off"
- A redesign of an existing product surface
- Reviewing someone else's implementation against a design direction
- Checking whether a build still matches its `DESIGN.md`
- Any request where code or a screenshot exists before the conversation starts

Not for: greenfield work (use the standard research workflow), or a specific named fix
("make this button bigger") which is just a direct build.

## Phase 1 — Read The Build, Infer The System

Before any Mobbin search. The goal is to state what the current design *actually is*, as a
system, in values. Most existing builds have an implicit design system nobody wrote down;
your first job is to write it down.

From the code or the screenshot, extract:

| Dimension | What to record |
|-----------|----------------|
| Color | Every distinct value in use, as hex/oklch, with where it appears |
| Accent discipline | How many accents, what roles they hold, approximate screen coverage |
| Type | Families, the actual size steps in use, weights, letter-spacing on display sizes |
| Spacing | The rhythm actually used, and whether it is a system or ad hoc |
| Radius / border / elevation | Values, and whether components are ranked or all identical |
| Layout | Container widths, grid structure, section rhythm |
| Motion | Every transition and animation, its duration, its easing |
| States | Hover, focus-visible, active, disabled, loading, empty, error — which exist |
| Imagery | What media is used, and whether roles are consistent |

Then state the two findings that matter:

**Is there a system, or a pile?** A system means values repeat and mean something. A pile
means twelve grays, four radii, and three easing curves that arrived independently. Say which
this is, plainly — it changes everything downstream.

**What is the strongest thing here?** Every build has one. Find it before criticizing
anything, because the audit's job is to amplify it, not replace it. A redesign that discards
the one good instinct in a build is a worse outcome than no redesign.

If a `DESIGN.md` exists, read it now and record every place the build has diverged from it.
Divergence is not automatically wrong — sometimes implementation taught the design something
— but every instance is either a drift to fix or a lock to update, and it must be classified.

## Phase 2 — Research Against What You Found

Now search, and search **specifically for the weaknesses you named**, not for the product
category in general.

If Phase 1 found flat hierarchy, search for how strong products build hierarchy on that
surface. If it found no empty states, search empty states. If it found a dense table that is
hard to scan, search dense tables. The research is a targeted response to identified problems,
which is what makes an audit different from generic inspiration gathering.

Run the direction pass too, but with a narrower purpose than in greenfield: look for the
direction **this build is already reaching for** and failing to reach, not a new direction to
impose. Ask what it would look like if the existing instinct were executed properly.

Batch of 2-3, notes as text between batches, same as always.

## Phase 3 — Classify Every Finding

Findings are useless as a flat list. Classify each one, because the categories carry
different authority and the user needs to know which is which.

| Class | Meaning | Authority |
|-------|---------|-----------|
| **Broken** | Objectively wrong: contrast failures, missing focus states, unreadable at a breakpoint, inaccessible interaction | High — fix regardless of taste |
| **Inconsistent** | Violates the system the build itself established | High — the build already agreed to this rule |
| **Drift** | Diverges from `DESIGN.md` where one exists | High — locked and then abandoned |
| **Below the bar** | Matches a Failure Gallery row in [exceptional-bar.md](exceptional-bar.md) | Medium — evidence-backed, name the row |
| **Opportunity** | Would be better a different way, per research | Medium — cite the reference |
| **Taste** | You would do it differently, no evidence | Low — say so, or do not raise it |

The last row is the honest one. An audit that presents preference as defect is how design
review earns its reputation. Mark it or drop it.

## Phase 4 — Prioritize By Cost And Effect

Rank by effect-per-unit-effort, not by severity alone. A contrast fix and a full layout
rework are not comparable items on one list.

```text
P0  Broken and blocking — accessibility failures, unusable states
P1  High effect, low cost — type scale, spacing rhythm, focus states, tabular-nums
P2  High effect, high cost — layout restructure, motion system, component rework
P3  Low effect — polish, taste, nice-to-have
```

The P1 band is where audits actually pay off, and it is the band most often skipped in favor
of a dramatic P2 rewrite. Type scale, spacing rhythm, `:focus-visible`, `tabular-nums`, and
letter-spacing on display type routinely move a build further than a redesign does, at a
fraction of the risk.

## Phase 5 — Report

```text
## What this build is

System or pile: <which, and the evidence>
Inferred tokens: <the values actually in use>
Strongest existing trait: <the thing to amplify>
DESIGN.md status: <matches / diverged in N places / none exists>

## Findings

| # | Finding | Class | Priority | Evidence | Fix |
|---|---------|-------|----------|----------|-----|
| 1 | <what is wrong> | <class> | <P0-P3> | <mobbin_url / gallery row / a11y rule> | <specific change> |

## Recommended sequence

<the P0/P1 set, in the order that makes each next step easier>

## What I would not change

<the strongest trait, and anything the build gets right that a redesign would break>
```

The last section is not politeness. It is what makes the audit safe to act on — it tells
whoever implements the fixes where the load-bearing walls are.

## Phase 6 — Offer The Lock

If the audit found "pile, not system", the highest-value output is not the fix list. It is a
`DESIGN.md` giving the build a system to be consistent with going forward. Offer to write one
from the audit's inferred tokens plus the research — see [design-md.md](design-md.md).

Without that, the same audit becomes necessary again in three months.

## Anti-Patterns

| Pattern | Why it fails |
|---------|--------------|
| Researching before reading the build | Anchors on an imagined product; produces critique of what is absent rather than what is present |
| A flat list of everything wrong | Unprioritized findings get triaged by whoever reads them, badly |
| Presenting taste as defect | Burns the authority needed for the real findings |
| Proposing a rewrite when P1 fixes would do | High risk, high cost, discards working code and institutional knowledge |
| Ignoring the strongest existing trait | The rebuild loses the one thing the build had |
| Auditing against a `DESIGN.md` without reading it in full | You will "find" divergences the lock explicitly permits |
| No fix stated, only a problem named | A finding without a specific change is a complaint |
