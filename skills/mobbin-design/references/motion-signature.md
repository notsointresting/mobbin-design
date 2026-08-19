# Signature Motion

`motion.md` is the safety reference: timing floors, easing sanity, reduced-motion
compliance, and the restraint that dense product UI requires. It answers *how do I not make
this worse*.

This file answers the opposite question: *what makes this memorable*. Read it whenever the
surface is a landing page, hero, marketing site, launch page, onboarding, empty state,
pricing page, or any moment where the interface is trying to make an impression rather than
get out of the way.

## The Default Is Wrong By Surface

The most common failure is applying product-UI restraint to a marketing surface. A settings
panel and a launch page are not the same design problem, and one motion default cannot serve
both.

| Surface | Motion ceiling | What "right" looks like |
|---------|----------------|-------------------------|
| Dense product UI: tables, settings, forms, admin | Micro-feedback and state transitions only | Hover affordance, focus rings, optimistic state, skeleton→content. Nothing announces itself. |
| Navigation, modals, drawers, tabs | Layout continuity | Shared-element morphs, directional entry matching the trigger, backdrop that tracks progress |
| Onboarding, empty states, first-run | Layout continuity plus one expressive beat | The moment the user first succeeds gets a single expressive response |
| Landing, hero, marketing, launch, pricing | Expressive, and expected | Scroll-linked composition, a signature interaction, coordinated multi-property states |

**The rule:** marketing and first-impression surfaces default *up*. Product surfaces default
*down*. A landing page with only hover-color changes has under-delivered exactly as badly as
a settings table with a parallax hero has over-delivered.

## The Signature Move Requirement

Every non-trivial build ships **at least one** interaction that cannot be expressed as a
single utility class. Named in the decision ledger before implementation, verified at the
Bar Check.

Qualifying moves:

| Move | Mechanism | Best for |
|------|-----------|----------|
| Scroll-linked composition | `animation-timeline: scroll()` / `view()`, or IntersectionObserver + transform | Heroes, long marketing pages, narrative sections |
| Shared-element continuity | View Transitions API, or FLIP measurement | List→detail, tab switches, route changes, modals from a trigger |
| Staggered orchestration | Delay mapped to an index or to visual depth | Any group arriving at once: cards, nav items, list rows |
| Spring / physical response | Spring easing, or a physics library where it earns its weight | Drag, dismiss, pull, toggle, anything the user "grabs" |
| Cursor-coupled surface | Pointer position written to custom properties | Hero lighting, magnetic buttons, tilt cards, spotlight reveals |
| Coordinated state | One custom property driving 3+ properties | Cards, buttons, rows, any hoverable component |
| Continuous ambient motion | Canvas, WebGL, SVG, or CSS on a slow loop | Backgrounds where the subject matter justifies it |
| Numeric / textual transition | Count-up, scramble, odometer, morph | Metrics, prices, status changes |

Non-qualifying (these are baseline, not signature):

- A fade-in on scroll
- A color change on hover
- A transform on a single property
- A spinner
- Anything applied uniformly to every element on the page

## Implementation Notes That Separate Good From Broken

**Prefer the platform.** `animation-timeline`, View Transitions, and `@starting-style` cover
most of the above with no runtime cost and automatic interruption handling. Reach for a
library only when the platform genuinely cannot express the move — drag physics and complex
sequenced timelines are the honest cases.

**Progressive enhancement, always.** Wrap platform-new features in `@supports`. The build
must be complete and correct without them, then better with them. A page that is broken in a
browser without `animation-timeline` has traded correctness for a trick.

**Compositor-only properties.** `transform`, `opacity`, and `filter` animate cheaply.
Animating `width`, `height`, `top`, `left`, `margin`, or shadow blur on every frame causes
layout thrash. The card exemplar in `exceptional-bar.md` transitions its shadow on a 420ms
one-shot, not on a scroll timeline — that distinction matters.

**One easing family per build.** Pick a curve and a scale, then derive everything from it.
Mixed easing reads as a page assembled by several people.

```css
:root {
  --ease-out: cubic-bezier(0.22, 1, 0.36, 1);
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);
  --ease-expo: cubic-bezier(0.16, 1, 0.3, 1);

  --dur-tap: 120ms;
  --dur-state: 240ms;
  --dur-move: 420ms;
  --dur-scene: 900ms;
}
```

**Reduced motion is a design decision, not a kill switch.** Blanket-disabling all animation
is lazy and often worse — the user loses affordance and continuity along with the movement.
Replace expressive movement with instant state or a cross-fade; keep the feedback.

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 1ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 1ms !important;
    scroll-behavior: auto !important;
  }
}
```

Use that as the floor, then hand-restore the specific feedback that carries meaning.

**Interruption.** Every motion must be interruptible. If a user hovers off mid-transition,
the element returns from where it is, not from where it would have been. CSS transitions get
this free; hand-rolled `requestAnimationFrame` loops do not, and that is the usual reason a
custom animation feels broken.

**Entry once, not on every scroll pass.** `IntersectionObserver` callbacks must `unobserve`
after firing. Elements that re-animate every time they scroll into view read as a bug.

## Sourcing Motion When Mobbin Cannot

Mobbin returns **still screenshots**. It has no motion data. This is a real gap, and the
skill must not let it become a reason to ship static work.

Motion direction is therefore sourced differently from visual direction:

1. **Name a product whose motion language you are targeting** and describe it concretely.
   "Linear's command palette: instant open, no scale-in, content cross-fades on a 120ms
   curve" is a usable source. "Smooth and modern" is not. A catalog of named motion languages,
   each described in enough detail to cite, is in [motion-library.md](motion-library.md).
2. **Derive from the visual lock.** A dense, precise, hairline-bordered direction implies
   fast, small, mechanical motion. A generous, high-contrast, expressive direction implies
   slower, larger, springier motion. The visual system constrains the motion system — say how.
3. **Cite this file and `motion.md`** for the mechanism. Craft references are a legitimate
   source under the skill's evidence rules.

Record it in the reference lock as `Motion direction:` with a named source and a described
character. A motion decision with no stated source is the same failure as a color decision
with no stated source.

## Anti-Patterns

| Pattern | Why it fails |
|---------|--------------|
| Every element fades up 20px on scroll | The page becomes a slideshow; nothing is emphasized because everything is |
| Animation on page load that delays content | Motion must never gate reading. Content is present; motion decorates its arrival |
| Parallax on text | Reading a moving target. Parallax belongs to background and imagery layers |
| Durations over 1s on anything the user triggered | Feels unresponsive. Scene-setting can be slow; response cannot |
| Bounce/elastic easing on utility UI | Reads as toy. Reserve overshoot for physical metaphors |
| Autoplaying looped motion near text | Competes with reading; also an accessibility problem |
| Hover-only interactions with no touch or keyboard path | Excludes most users on most devices |
| Motion that fires on every intersection | Reads as a glitch, not a flourish |
| A different easing curve per component | No system; the page feels assembled rather than designed |

## Motion Lock Format

Fill this before implementation. It belongs alongside the visual reference lock.

```text
Motion direction: [named product / derived-from-visual-lock, with described character]
Surface tier: [product UI / navigation / onboarding / marketing] -> ceiling: [micro / continuity / expressive]
Signature move: [name it, from the qualifying table, and say why it fits this product]
Easing family: [curve + the durations derived from it]
Choreography: [entry, scroll response, hover/press, state change, exit]
Deliberately still: [what does NOT move, and why that restraint is the point]
Reduced-motion plan: [what is replaced, not merely removed]
```

If `Signature move` is empty, the build is not ready to start.
