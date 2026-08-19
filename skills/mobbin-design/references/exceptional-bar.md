# The Exceptional Bar

Prose cannot raise a design bar. Adjectives like "polished", "distinctive", or "high-craft"
compress to nothing during generation, and the output regresses to the mean of the training
distribution — which is a starter template.

Artifacts do raise the bar. This file holds working code at the target quality. Read the
exemplar closest to the surface being built **before** writing implementation code, and
carry its structural moves forward.

## How To Use This File

1. Pick the exemplar matching your surface (hero, component, navigation, data, detail).
2. Read the **Why this clears the bar** notes — those name the transferable move.
3. Reimplement the move with the reference lock's tokens. Do not paste the exemplar's colors,
   type, or copy; paste nothing at all. The move transfers, the styling does not.
4. If your build has no move from any exemplar and no equivalent of your own, it is not done.

These exemplars are deliberately un-branded — neutral system colors, no typeface choices,
placeholder copy. That is so the *structure* is what you carry. A build that looks like these
exemplars has failed; a build that is as considered as these exemplars has succeeded.

---

## The Line Between Basic And Exceptional

Every exemplar below is separated from its basic counterpart by exactly one thing: something
in the interface **responds to the user in a way a static mockup cannot express**.

| Basic | Exceptional |
|-------|-------------|
| Section fades in on scroll | Section elements arrive on a scroll-linked timeline, staggered by depth |
| Button changes color on hover | Button has a weighted press, a directional sheen tracking the cursor, and a settled resting state |
| Card lifts on hover | Card lifts, its shadow softens with distance, its border warms, and its content reflows by 1-2px — one gesture, four coordinated properties |
| Tab switches content instantly | Underline morphs between tabs with shared-element continuity; content cross-fades on the same curve |
| Number displayed | Number counts up once on first intersection, then stays put |
| Nav is a nav | Nav condenses, gains a backdrop blur, and re-weights its type past the fold |

The right column is not "more animation." It is **coordinated** animation: multiple
properties, one intent, one easing family.

---

## Exemplar 1 — Scroll-Linked Hero

The most common surface, and the one most often shipped basic.

**Why this clears the bar:** three moves stacked. (a) Scroll-driven transform on the
headline, not a one-shot fade — the page responds continuously. (b) Staggered arrival where
delay maps to visual depth, so the composition assembles rather than appearing. (c) A
cursor-tracked gradient that makes the surface feel lit rather than painted.

```html
<section class="hero">
  <div class="hero__glow" aria-hidden="true"></div>
  <h1 class="hero__title">
    <span class="hero__line" style="--i:0">Build the thing</span>
    <span class="hero__line" style="--i:1">that outlives</span>
    <span class="hero__line" style="--i:2">the demo.</span>
  </h1>
  <p class="hero__sub" style="--i:3">Placeholder subhead. Replace with locked copy.</p>
  <div class="hero__actions" style="--i:4">
    <button class="btn btn--primary">Primary action</button>
    <button class="btn btn--ghost">Secondary</button>
  </div>
</section>
```

```css
.hero {
  --ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
  position: relative;
  min-height: 92svh;
  display: grid;
  align-content: center;
  padding: clamp(1.5rem, 5vw, 6rem);
  overflow: clip;
}

/* (c) cursor-tracked light — the surface reacts to presence */
.hero__glow {
  position: absolute;
  inset: -20%;
  pointer-events: none;
  background: radial-gradient(
    38rem 38rem at var(--mx, 50%) var(--my, 40%),
    color-mix(in oklch, canvastext 9%, transparent),
    transparent 70%
  );
  transition: opacity 600ms var(--ease-out-expo);
}

.hero__title {
  font-size: clamp(2.75rem, 7vw, 6rem);
  line-height: 0.95;
  letter-spacing: -0.035em;
  text-wrap: balance;
  margin: 0;
}

.hero__line { display: block; }

/* (b) staggered arrival — delay maps to reading depth */
.hero__line,
.hero__sub,
.hero__actions {
  opacity: 0;
  transform: translateY(1.25rem);
  animation: rise 900ms var(--ease-out-expo) forwards;
  animation-delay: calc(var(--i) * 70ms);
}

@keyframes rise {
  to { opacity: 1; transform: none; }
}

/* (a) scroll-linked departure — continuous response, not a one-shot */
@supports (animation-timeline: view()) {
  .hero__title {
    animation: settle linear both;
    animation-timeline: view();
    animation-range: exit 0% exit 80%;
  }
  @keyframes settle {
    to { opacity: 0.15; transform: translateY(-3rem) scale(0.97); filter: blur(2px); }
  }
}

@media (prefers-reduced-motion: reduce) {
  .hero__line, .hero__sub, .hero__actions, .hero__title {
    animation: none;
    opacity: 1;
    transform: none;
    filter: none;
  }
}
```

```js
// Pointer light. Throttled to the frame; no listener churn.
const hero = document.querySelector('.hero');
let queued = false, px = 0, py = 0;

hero.addEventListener('pointermove', (e) => {
  const r = hero.getBoundingClientRect();
  px = ((e.clientX - r.left) / r.width) * 100;
  py = ((e.clientY - r.top) / r.height) * 100;
  if (queued) return;
  queued = true;
  requestAnimationFrame(() => {
    hero.style.setProperty('--mx', px + '%');
    hero.style.setProperty('--my', py + '%');
    queued = false;
  });
});
```

**Transferable move:** delay-by-depth staggering via a `--i` index, and
`animation-timeline: view()` behind `@supports` so the scroll response is progressive
enhancement rather than a dependency.

---

## Exemplar 2 — Coordinated Hover On A Card

The highest-frequency component, and the clearest basic/exceptional tell.

**Why this clears the bar:** one gesture drives five properties on one easing family —
elevation, shadow softness, border warmth, interior parallax, and an arrow that commits to a
direction. Basic versions animate exactly one.

```css
.card {
  --ease: cubic-bezier(0.22, 1, 0.36, 1);
  --lift: 0;
  position: relative;
  display: grid;
  gap: 0.75rem;
  padding: 1.5rem;
  border-radius: 14px;
  border: 1px solid color-mix(in oklch, canvastext 12%, transparent);
  background: canvas;
  transform: translateY(calc(var(--lift) * -6px));
  box-shadow:
    0 1px 2px color-mix(in oklch, canvastext calc(6% + var(--lift) * 2%), transparent),
    0 calc(var(--lift) * 18px) calc(var(--lift) * 34px)
      color-mix(in oklch, canvastext calc(var(--lift) * 10%), transparent);
  transition:
    transform 420ms var(--ease),
    box-shadow 420ms var(--ease),
    border-color 420ms var(--ease);
}

.card:hover,
.card:focus-within {
  --lift: 1;
  border-color: color-mix(in oklch, canvastext 24%, transparent);
}

/* interior parallax — content moves less than the card, which reads as depth */
.card__title {
  transform: translateY(calc(var(--lift) * -2px));
  transition: transform 420ms var(--ease);
}

/* the arrow commits to a direction rather than just appearing */
.card__arrow {
  transform: translateX(calc(var(--lift) * 4px));
  opacity: calc(0.45 + var(--lift) * 0.55);
  transition: transform 420ms var(--ease), opacity 420ms var(--ease);
}

@media (prefers-reduced-motion: reduce) {
  .card, .card__title, .card__arrow { transition-duration: 1ms; }
}
```

**Transferable move:** drive everything from **one custom property** (`--lift`) set by a
single `:hover, :focus-within` rule. Coordination becomes structural instead of a pile of
independent transitions that drift out of sync. Note `:focus-within` — keyboard users get
the same affordance, which is a craft requirement, not a bonus.

---

## Exemplar 3 — Shared-Element Tab Morph

**Why this clears the bar:** the indicator travels between tabs instead of teleporting, and
the panel cross-fades on the same curve. Continuity tells the user the two states are the
same object in two positions. Instant swaps make them feel like unrelated pages.

```html
<div class="tabs" role="tablist">
  <button role="tab" aria-selected="true">Overview</button>
  <button role="tab" aria-selected="false">Activity</button>
  <button role="tab" aria-selected="false">Settings</button>
  <span class="tabs__ink" aria-hidden="true"></span>
</div>
```

```css
.tabs { position: relative; display: inline-flex; gap: 0.25rem; }

.tabs [role="tab"] {
  position: relative;
  padding: 0.5rem 0.875rem;
  border: 0;
  background: none;
  color: color-mix(in oklch, canvastext 55%, transparent);
  transition: color 260ms cubic-bezier(0.22, 1, 0.36, 1);
}
.tabs [role="tab"][aria-selected="true"] { color: canvastext; }

.tabs__ink {
  position: absolute;
  bottom: 0;
  height: 2px;
  background: canvastext;
  border-radius: 2px;
  width: var(--w, 0);
  transform: translateX(var(--x, 0));
  transition:
    transform 420ms cubic-bezier(0.22, 1, 0.36, 1),
    width 420ms cubic-bezier(0.22, 1, 0.36, 1);
}

@media (prefers-reduced-motion: reduce) {
  .tabs__ink { transition-duration: 1ms; }
}
```

```js
const tabs = document.querySelector('.tabs');
const ink = tabs.querySelector('.tabs__ink');

function moveInk(el) {
  ink.style.setProperty('--x', el.offsetLeft + 'px');
  ink.style.setProperty('--w', el.offsetWidth + 'px');
}

tabs.addEventListener('click', (e) => {
  const tab = e.target.closest('[role="tab"]');
  if (!tab) return;
  tabs.querySelectorAll('[role="tab"]').forEach((t) =>
    t.setAttribute('aria-selected', String(t === tab)));
  moveInk(tab);
});

moveInk(tabs.querySelector('[aria-selected="true"]'));
addEventListener('resize', () =>
  moveInk(tabs.querySelector('[aria-selected="true"]')), { passive: true });
```

**Transferable move:** measure the target, write geometry to custom properties, let CSS
interpolate. Works for tabs, segmented controls, sidebar selection, filter pills, and stepper
progress — anywhere a selection moves. `aria-selected` drives both state and style, so the
accessible name and the visual share one source of truth.

Where the browser supports it, `view-transition-name` on the panel gives the same continuity
for the content swap with far less code.

---

## Exemplar 4 — Data Surface With Earned Restraint

Dense product UI is where expressive motion becomes wrong. Exceptional here means precision,
not expression — but it is still not basic.

**Why this clears the bar:** tabular figures so digits do not jitter, a sticky header that
gains its border only once content passes under it, row affordance without a hover-highlight
carnival, and a count-up that fires once and never again.

```css
.table-wrap { max-height: 70svh; overflow: auto; }

table { border-collapse: separate; border-spacing: 0; width: 100%; }

/* numbers must not reflow between values */
td.num, .stat__value {
  font-variant-numeric: tabular-nums;
  font-feature-settings: "tnum" 1;
  text-align: right;
}

/* header earns its border only when it is actually occluding content */
thead th {
  position: sticky;
  top: 0;
  z-index: 1;
  background: canvas;
  border-bottom: 1px solid transparent;
  transition: border-color 200ms ease, box-shadow 200ms ease;
}
.table-wrap[data-scrolled="true"] thead th {
  border-bottom-color: color-mix(in oklch, canvastext 14%, transparent);
  box-shadow: 0 6px 12px -10px color-mix(in oklch, canvastext 40%, transparent);
}

/* row affordance: a leading rule, not a full-width wash */
tbody tr td:first-child {
  box-shadow: inset 2px 0 0 transparent;
  transition: box-shadow 160ms ease;
}
tbody tr:hover td:first-child { box-shadow: inset 2px 0 0 canvastext; }
```

```js
const wrap = document.querySelector('.table-wrap');
wrap.addEventListener('scroll', () => {
  wrap.dataset.scrolled = String(wrap.scrollTop > 0);
}, { passive: true });

// count-up: fires once, respects reduced motion, leaves the DOM value correct
const reduce = matchMedia('(prefers-reduced-motion: reduce)').matches;
new IntersectionObserver((entries, obs) => {
  for (const entry of entries) {
    if (!entry.isIntersecting) continue;
    const el = entry.target;
    const target = Number(el.dataset.value);
    obs.unobserve(el);
    if (reduce) { el.textContent = target.toLocaleString(); continue; }
    const start = performance.now();
    const dur = 900;
    const tick = (now) => {
      const t = Math.min(1, (now - start) / dur);
      const eased = 1 - Math.pow(1 - t, 3);
      el.textContent = Math.round(target * eased).toLocaleString();
      if (t < 1) requestAnimationFrame(tick);
    };
    requestAnimationFrame(tick);
  }
}, { threshold: 0.4 }).observe(document.querySelector('.stat__value'));
```

**Transferable move:** state that reflects reality (`data-scrolled`) rather than state that
fires on a timer. And `tabular-nums` — the cheapest craft signal in the discipline, skipped
in almost every generated dashboard.

---

## Exemplar 5 — The Memorable Detail

Every non-trivial build needs one thing a user could describe from memory a day later. It
does not have to be large. It has to be **specific and unrepeated**.

Candidates that work, ordered by cost:

- **A single deliberate type contrast.** One word or figure at 4-6x the body size, set tight,
  where every other element is quiet. Costs nothing; carries a page.
- **An unexpected empty state.** Not an icon and a sentence — an actual composed moment with
  a voice.
- **A transition that names the relationship.** A list item that morphs into the detail view
  it opens.
- **A texture with a reason.** Grain, hairline grid, or noise tied to the product's subject
  matter — not applied because it looked expensive.
- **A cursor state that teaches.** A custom cursor on exactly one interactive surface where
  the interaction is non-obvious.

```css
/* shared-element continuity between a list row and its detail view */
@view-transition { navigation: auto; }

.row[data-id="42"] .row__title { view-transition-name: title-42; }
.detail__title { view-transition-name: title-42; }

::view-transition-old(*), ::view-transition-new(*) {
  animation-duration: 380ms;
  animation-timing-function: cubic-bezier(0.22, 1, 0.36, 1);
}

@media (prefers-reduced-motion: reduce) {
  ::view-transition-old(*), ::view-transition-new(*) { animation-duration: 1ms; }
}
```

**Transferable move:** pick exactly one. Two memorable details cancel each other out — the
page reads as busy rather than considered. The detail is named in the decision ledger before
implementation, so it survives the build.

---

## Failure Gallery

What "basic" actually looks like in output. If the build matches any row, it has not cleared
the bar.

| Symptom | What it signals |
|---------|-----------------|
| Every section is the same vertical padding with a centered fixed-width column | No spatial system; default utility rhythm |
| Every interactive element uses the same 150ms ease-in-out | No motion system; one transition copy-pasted |
| Indigo/violet accent nobody asked for | Model default, not a research finding |
| Hero → 3-up feature grid → testimonial → pricing → FAQ → CTA | Template order, not researched page rhythm |
| Icons all the same weight, size, and library, floating unaligned | Icons treated as decoration, not typography |
| Headings at bold weight with default letter-spacing | Display type never tuned; a 4-second fix skipped |
| Numbers shift width as they update | `tabular-nums` skipped |
| One opacity fade-in on scroll, applied to everything | Motion treated as a checkbox |
| Nothing responds to hover except color | No coordinated state |
| No `:focus-visible` styling distinct from hover | Keyboard path never considered |
| A gradient border on a card for no stated reason | Decoration without a role rule |
| Every surface at the same elevation and radius | No depth system; components never ranked |

---

## Bar Check

Before handoff, answer in words. An answer you cannot write down does not exist.

1. Name the signature interaction. What does it do and why does it fit this product?
2. Name the motion system: durations, easing family, and what is deliberately still.
3. Name the one detail a user would describe from memory tomorrow.
4. Point at the row of the Failure Gallery you came closest to, and say what you did instead.
5. If a screenshot of this build sat beside a screenshot of a default starter template, what
   is the first difference someone would name?

If any answer is "nothing" or a hedge, the work is not finished.
