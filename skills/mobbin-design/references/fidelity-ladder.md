# The Fidelity Ladder — How Close Is This Supposed To Be?

"Make it like Linear" is four different requests, and the skill has historically answered only
one of them. Its rule — *do not copy one reference, synthesize* — is correct as a default and
insufficient as a whole vocabulary, because real users legitimately want degrees of closeness
the rule does not name.

This file names them, and specifies what the user must supply at each level.

## The Ladder

| Level | What it means | What comes from the reference | What must come from the user |
|-------|---------------|-------------------------------|------------------------------|
| **1. Inspired-by** | One trait borrowed, everything else yours | A single detail | Brand, content, structure, tone |
| **2. Genre-match** | Reads as the same category of product | Density, structure, conventions | Brand, content, visual identity |
| **3. High-fidelity** | Feels like a sibling product | Layout, spacing, type scale, motion character, component rules | Brand, copy, imagery, product content — all of it |
| **4. Identical clone** | Indistinguishable including brand | Everything | — not built |

Level 3 is what most people mean by "make it identical," and it is entirely legitimate. Level
4 is not built — see the boundary below.

**Default to level 2** when the user has not said. Escalate to 3 only when they ask for it and
can supply the materials, because level 3 without materials produces something worse than
level 2: correct proportions wrapped around invented content, which reads as a template no
matter how good the layout is.

## The Boundary

Level 4 is not declined for squeamishness. Three concrete reasons:

1. **Trade dress and brand assets are protected.** Logos, wordmarks, custom typefaces,
   illustration systems, and photography belong to the company that made them. Reproducing
   them is not a design decision; it is a legal exposure the user inherits.
2. **It does not serve the user.** A product indistinguishable from a competitor gives nobody
   a reason to choose it over the competitor. The value in a reference is its *thinking*, and
   thinking transfers at level 3 without the liability.
3. **The skill's own evidence rules make it incoherent.** Every decision must trace to a
   source and be adapted to the user's product, audience, and constraints. A clone has no
   adaptation step, so the decision ledger has nothing to record.

If a user asks for level 4, say this once, plainly, offer level 3, and move on. Do not
moralize, do not repeat it, and do not quietly deliver level 2 while calling it level 3.

## The Intake — What The User Must Submit For Level 3

This is the section that matters. Most "make it like X" builds fail not because the reference
was misread, but because the user supplied a reference and **nothing else**, so the agent
invented copy, invented content, and invented imagery. Invented content is what makes a build
read as generic regardless of how precise the layout is.

Ask for these before starting. Missing items are not blockers, but each one you have to invent
lowers the achievable fidelity, and the user should learn that trade before the build rather
than after.

```text
## Intake — high-fidelity build

BRAND
  Logo files            <svg/png, light and dark variants>
  Brand colors          <hex values, and what each one is FOR>
  Typeface              <name + license, or "use a free alternative", or "pick one">
  Existing guidelines   <link or file, if any>

CONTENT  (highest-value section — invented copy is the top cause of generic output)
  Headlines             <real ones, or "write them">
  Body copy             <real, or the key points to write from>
  CTA text              <exact words>
  Product/feature names <exact spelling and capitalization>
  Voice                 <2-3 adjectives, or a sample of existing writing>

ASSETS
  Product screenshots   <files, or "generate", or "use placeholders">
  Photography           <files, stock budget, or "none — code-native only">
  Illustration          <files, budget, or "none">
  Icons                 <library preference, or "pick one">

STRUCTURE
  Surfaces needed       <which pages or screens>
  Per-surface goal      <what each must accomplish>
  Priority order        <what gets built first>

REFERENCE
  The target            <URL, mobbin_url, or screenshots>
  Traits that matter    <the 3-5 things the user actually wants preserved>
  Traits to drop        <what about the reference does NOT fit this product>

CONSTRAINTS
  Framework             <React/Next/Vue/plain/etc, and any existing design system>
  Accessibility         <target level>
  Breakpoints           <which matter>
  Deadline              <scope-setting>

DO NOT CHANGE
  <anything in the user's current product that must survive untouched>
```

## The Rules That Make Intake Work

**Ask once, in one block.** A drip of individual questions across ten turns exhausts the user,
and they start answering "whatever you think" — which is how you end up inventing content
anyway.

**Offer defaults for everything.** Every line above has a reasonable fallback. Present the
intake as "here is what I will assume unless you tell me otherwise," not as a form to fill
out. The user answers three lines and you proceed on stated assumptions for the rest.

**Name the fidelity cost of each gap.** "No real copy means I write placeholder headlines,
which will read as generic no matter how well the layout lands" is useful. Silently proceeding
is not.

**The `DO NOT CHANGE` line is the one people forget to ask for,** and it causes the most
rework. Anything already shipped, already approved, or already loved must be named before the
build, not discovered during review.

**`Traits to drop` is not optional.** A reference always carries something wrong for the
user's product — a tone, a density, a convention from a different market. Naming it converts
"match this" from an impossible instruction into a bounded one.

## Recording It

The chosen fidelity level goes in `DESIGN.md`, inside the reference lock, on its own line:

```text
Fidelity: level 3 (high-fidelity) — layout, spacing, type scale, and motion character track
  the reference; brand, copy, imagery, and content are the user's throughout.
  Dropped from reference: <traits that do not fit>.
```

Without that line, the next session cannot tell whether a close match was the goal or an
accident, and will "correct" it in one direction or the other.

## Relationship To Other Files

- Inventorying a reference product before choosing a level:
  [product-teardown.md](product-teardown.md)
- Persisting the chosen level and the lock: [design-md.md](design-md.md)
- The quality bar, which applies identically at every level:
  [exceptional-bar.md](exceptional-bar.md)
