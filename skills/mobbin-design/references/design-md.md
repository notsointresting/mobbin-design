# DESIGN.md — The Durable Lock

The reference lock and motion lock are the two artifacts that keep a build from sliding back
to generic. Both normally live in conversation, which means both die at the first compaction,
session boundary, or "continue this tomorrow."

That is a mechanical failure, not a taste failure. Turn 40 of a build has no access to what
turn 3 decided, so it re-derives from model priors — and model priors are exactly the average
this skill exists to escape. A design that drifts this way was never overruled; it was
forgotten.

`DESIGN.md` is the fix. Write it once, after research and before implementation. Commit it.
Read it at the start of every subsequent design turn.

## When To Write One

Write `DESIGN.md` when the work will outlive one session:

- a new product surface, landing page, or app
- a redesign of anything with more than two screens
- any project where a second person, or a future session, will touch the design
- any build where the user asked for a specific direction worth protecting

Skip it for a one-off fix, a single component tweak, or a task that will be finished and
handed off inside the same conversation. A lock file for a button color change is bureaucracy.

## Where It Goes

Project root, next to `README.md`. Not inside `.claude/`, not inside a `docs/` folder nobody
opens — the point is that it is found without being looked for.

If the project already has a design system document, **extend it rather than competing with
it.** Two files claiming authority over the same tokens is worse than none.

## The Template

```markdown
# Design — <product name>

Last updated: <YYYY-MM-DD> · Research: <n> screens, <n> flows, <n> sections

## Brief

Designing <what> for <who> on <platform>.
Goal: <primary user goal>
Tone: <desired feeling>
Main objection: <the thing the design must overcome>
Constraints: <brand, framework, budget, deadline, accessibility>

## Reference Lock

Primary direction: <one dominant source> — <mobbin_url>
Preserve: <3-5 traits that must survive implementation>
Borrow only: <1-2 specific details, each with its source and mobbin_url>
Role rules: <token and component meanings that must not be repurposed>
Media strategy: <real / generated / stock / code-native / placeholder, with art direction>
Reject: <the defaults that would collapse this direction>

## Motion Lock

Motion direction: <named source, described concretely>
Surface tier: <product UI / navigation / onboarding / marketing> -> ceiling: <micro / continuity / expressive>
Signature move: <the one interaction that is not a utility class>
Easing family: <one curve>
Duration scale: <tap / state / move / scene>
Choreography: <entry, scroll, hover/press, state change, exit>
Deliberately still: <what does not move, and why>
Memorable detail: <the one thing a user could describe tomorrow>
Reduced-motion plan: <what is replaced, not merely removed>

## Tokens

Values, never adjectives. Sampled from the reference screenshots.

| Role | Value | Rule |
|------|-------|------|
| canvas | #FCFCFA | — |
| surface | #FFFFFF | cards only; never full-bleed |
| text primary | #1A1A18 | — |
| text secondary | #6B6B66 | — |
| border | #E8E8E4 | hairline only; no shadows on this surface |
| accent | #2E6BE6 | selection state ONLY — never CTA, never badge fill |
| cta | #1A1A18 | — |

Type scale: <base size, ratio, and the actual computed steps>
Spacing: <base unit and the scale>
Radius: <value, and which components deviate>
Accent coverage: <approximate % of the screen the accent occupies — this is the discipline>

## Decision Ledger

| Decision | Source | Source rule / role | Why |
|---|---|---|---|
| <choice> | <mobbin_url / brief / craft reference> | <role to preserve> | <specific rationale> |

## Research Index

Every screen reviewed, with the one line that made it worth reviewing. This is what stops a
future session from re-running the same searches.

| Screen | URL | Finding |
|---|---|---|
| <app — screen type> | <mobbin_url> | <the one concrete observation> |

## Open Questions

Decisions deferred, and what would settle them.
```

## The Rules That Make It Work

**Values, not adjectives.** "Warm off-white canvas with a restrained blue accent" survives
into implementation as `#fff` and `#3b82f6`. `#FCFCFA` and `#2E6BE6` survive as themselves.
Every row of the token table is a value.

**Role rules are the point of the token table.** A hex code without its rule gets repurposed
— the accent becomes a CTA, then a badge fill, then a background wash, and the direction is
gone. The rule column prevents that, and it is the column most often left empty.

**The reject list is load-bearing.** It is where the defaults that would collapse this
specific direction get written down. Generic reject lists ("no AI slop") do nothing. Specific
ones ("no 7-column streak grids, no gradient CTAs, no countdown urgency") do the work.

**The research index prevents re-research.** Without it, session three re-runs session one's
searches, gets a different result set, and quietly locks a different direction. With it, the
evidence stays durable after the images have left context.

**Update it when a decision changes.** A lock file that has diverged from the build is worse
than none, because it will be trusted. When implementation forces a change, change the file
and note why in the ledger.

## Reading It Back

At the start of any design turn on a project that has a `DESIGN.md`:

1. Read the whole file, not a summary of it.
2. Restate the reject list and the role rules before writing code — those two sections decay
   fastest under generation pressure.
3. If the task conflicts with the lock, say so explicitly and ask, rather than silently
   softening the lock to fit.

The last point matters most. The failure mode is never a deliberate override. It is a small
accommodation, then another, until the direction has been averaged away by a consent nobody
gave.

## Relationship To Other Files

- The lock formats here mirror the ones in `SKILL.md`; this file is where they are
  **persisted** rather than merely stated.
- Motion lock fields are defined in [motion-signature.md](motion-signature.md).
- The quality bar the build is measured against is in
  [exceptional-bar.md](exceptional-bar.md).
- Mid-build enforcement of the reject list is in [drift-check.md](drift-check.md).
- Auditing an existing build against this file is in [design-audit.md](design-audit.md).
