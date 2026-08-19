# Motion Library — Named Motion Languages

Mobbin returns stills. `motion-signature.md` says motion may be sourced from "a named
product's motion language, described concretely" — but a source you cannot describe precisely
is not a source, and "make it feel like Linear" is a vibe, not evidence.

This file is the catalog. Each entry describes a real product's motion language in enough
detail to be cited and implemented. Use it the way the reference lock uses a Mobbin screen:
pick one as primary, borrow at most one or two specific behaviors from others.

## How To Cite From This File

```text
Motion direction: Linear — instant open with no scale-in, short content cross-fade, zero
  overshoot anywhere. Chosen because the visual lock is dense and hairline-bordered, and
  mechanical motion matches that precision.
```

Naming the product is not enough. Name the **behavior** you are taking, or the citation is
decoration.

## A Warning About This File

These descriptions are written from observation, and products change. Treat entries as a
starting characterization, not a spec — if you can open the real product, open it. Where the
description and the live product disagree, the product wins.

They are also intentionally **characterizations rather than measurements**. Where a duration
appears it approximates the felt behavior; it is not a value read from source. Say so when you
cite one.

---

## The Precision Family

Fast, small, mechanical. No overshoot. Motion confirms rather than performs.

**Linear** — The reference point for this family. Panels and palettes appear at full size with
no scale-in; the only transition is a short opacity cross-fade. Hover states change well under
200ms. Nothing bounces, nothing eases dramatically, and there is no scroll-linked motion in
the product surface at all. The message is that the tool is faster than you are.

- Take it for: dense product UI, developer tools, anything where speed is the product claim
- Signature behavior: instant appearance plus a brief cross-fade, never scale-from-center
- Do not pair with: expressive scroll choreography — the two contradict each other

**Things 3** — Motion is small and physical rather than fast. Rows settle into place when
reordered; a completed task animates out with a brief, restrained gesture. The distinguishing
trait is that motion is *scoped to the element that changed*, never to the screen.

- Take it for: list-heavy apps, task and note surfaces, anything with reordering
- Signature behavior: only the changed element moves; its neighbors settle rather than jump

**Stripe (dashboard)** — Almost no motion in the data surfaces. Tables, charts, and filters
update in place. What motion exists is confined to navigation and modals. The restraint is the
point: financial data that animates reads as untrustworthy.

- Take it for: dashboards, tables, financial and admin surfaces
- Signature behavior: data does not animate; only navigation does

---

## The Continuity Family

Motion exists to explain where things came from and where they went.

**iOS (system)** — The most widely internalized motion language in existence, which makes it
the safest choice for a mobile product. Push transitions move horizontally in the direction of
travel; sheets rise from the bottom edge and can be dragged back down along the same axis;
modals scale from their trigger point. Every transition is interruptible mid-flight and
reverses from where it is, not from where it would have been.

- Take it for: any iOS product; also a sound default for mobile web
- Signature behavior: interruptibility and directional consistency
- The trait most often missed when imitating it: reversal from the current position

**Arc** — Sidebar and tab transitions carry a shared-element quality: the active item's
indicator travels rather than teleports, and the space reflows around it. Spaces switch with a
coordinated slide that keeps the user oriented.

- Take it for: navigation-heavy interfaces, sidebars, workspace switchers
- Signature behavior: the selection indicator is a persistent object that moves

**Notion** — Deliberately minimal, but every drag and block manipulation shows exactly where
the block will land before you release it. Motion is used for *prediction*, not for feedback
after the fact.

- Take it for: editors, drag-and-drop surfaces, builders
- Signature behavior: the interface shows the outcome during the gesture, not after it

---

## The Expressive Family

Motion is part of the message. Correct on marketing surfaces, wrong in dense product UI.

**Vercel / Next.js marketing** — Scroll-linked composition throughout. Elements arrive
staggered as sections enter the viewport, gradients and glows track pointer position, and code
samples animate their own content. Meanwhile the product surface behind it is nearly
motionless — the same company shipping expressive marketing and restrained product is the
clearest available demonstration of surface-tier discipline.

- Take it for: developer-tool landing pages, technical marketing
- Signature behavior: staggered scroll arrival plus pointer-tracked lighting
- Learn from the contrast, not only the motion

**Apple (product pages)** — The high end of scroll-driven narrative. Long sequences where the
product rotates, disassembles, or transitions as the page advances, bound tightly to scroll
position rather than to time. Expensive to produce and easy to do badly.

- Take it for: single-product launch pages with real production budget
- Signature behavior: scroll position drives a continuous timeline, not discrete triggers
- Honest caveat: a bad imitation of this is worse than a restrained page

**Consumer onboarding with spring physics** (Family, Duolingo, and similar) — Playful springs,
elements that overshoot and settle, character in the movement itself. Works when the product
is consumer, emotional, and low-stakes; fails hard on anything financial, medical, or
professional.

- Take it for: consumer onboarding, first-run, celebration moments
- Signature behavior: spring overshoot with a visible settle
- Never take it for: transactional confirmations, destructive actions, data entry

---

## Deriving From The Visual Lock Instead

If no product in this catalog fits, derive the motion character from the visual system you
already locked. The mapping is reliable because both express the same intent:

| Visual lock trait | Implied motion |
|-------------------|----------------|
| Dense, hairline borders, small type | Fast, small, mechanical. 120-240ms. No overshoot. |
| Generous space, large type, high contrast | Slower, larger travel. 400-900ms. Overshoot permitted. |
| Near-black canvas, single warm accent | Slow fades, low travel, light-based transitions over movement |
| Editorial, serif, content-forward | Almost none. Type and space carry it; motion would cheapen it |
| Playful color, rounded, illustrated | Spring physics, character, a visible settle |
| Precise, data-heavy, tabular | Motion confined to navigation; data never animates |

State the derivation explicitly when you use it: "the lock is dense and hairline-bordered, so
motion is fast and mechanical — 120/240ms, no overshoot." That sentence is a citable source.

---

## Capturing Motion Yourself

The best source is the live product. If browser automation is available, open the reference
site, record the interaction, and describe what you observe.

Record specifically:

1. What triggers it
2. Which properties change, and which deliberately do not
3. Approximate duration, and whether it feels linear, eased-out, or springy
4. Whether it can be interrupted, and what happens when you try
5. What the reduced-motion version does, if the site respects the preference

Five observations turn "feels nice" into a spec. That is the difference between a motion lock
that survives implementation and one that gets improvised at build time.
