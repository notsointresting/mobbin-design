# Product Teardown — Inventorying What A Real Product Contains

"Show me what Linear actually has" is a different question from "show me a settings screen."
The first asks for a product's **shape**: what screens exist, how they relate, what component
vocabulary recurs, and what the whole thing adds up to. Mobbin can answer it, but not in one
call.

## What Mobbin Can And Cannot Do Here

There is **no "get all screens for app X" tool**. The official server exposes three searches
and nothing else — no app browsing, no category listing, no popularity sort, no trending feed.
`mobbin.com` returns 403 to programmatic fetches, so there is no scraping fallback.

What does work: **named-app queries filter to that app.** `"Things 3 task list screen"`
returns Things 3 screens. A teardown is therefore built by *systematic repeated querying*,
walking past each result set with `exclude_screen_ids` until returns stop being new.

This has a consequence you must state in the output: **a teardown is a sample, never a
complete inventory.** Coverage depends on what Mobbin indexed for that product. Say what you
covered and what you could not find, rather than implying the map is the territory.

## Trending And Popular — Not Available

If the user asks what is trending or popular on Mobbin, the honest answer is that the MCP
surface cannot report it. Do not invent a list, and do not present your own impressions of
which products are fashionable as if they were Mobbin data.

The workaround is better than the missing feature anyway: ask the user to browse Mobbin and
paste three to five app names they find compelling. Human curation beats a popularity sort,
because the user knows which of those products is relevant to theirs.

## The Sweep

Run these passes in order. Batches of 2-3, notes written as text between batches — a teardown
generates a lot of images, and the notes are the deliverable, not the screenshots.

### Pass 1 — Entry and core

What the product is, and the screen it is actually used on.

```text
<app> home screen
<app> main <primary object> list
<app> <primary object> detail screen
```

The core screen carries the density, the type scale, and the component vocabulary everything
else inherits. Read it first and read it hardest.

### Pass 2 — Structure

Navigation, hierarchy, and how the product organizes itself.

```text
<app> navigation sidebar
<app> search screen
<app> filters applied
```

### Pass 3 — Account surfaces

Every product has these, and they are where design systems either hold or fall apart.

```text
<app> settings screen
<app> profile screen
<app> billing or subscription screen
<app> notification settings
```

### Pass 4 — The edges

Where craft actually shows. A product that has designed these is a different product from one
that has not.

```text
<app> empty state
<app> error state
<app> loading skeleton
<app> confirmation dialog
```

### Pass 5 — Journeys

```text
<app> onboarding          (mobbin_search_flows)
<app> signup              (mobbin_search_flows)
```

### Pass 6 — Marketing, for web products

```text
<app> hero section        (mobbin_search_sections)
<app> pricing page
```

Stop when a pass returns screens you have already seen twice. That is the coverage ceiling for
that product, and pushing past it burns context for nothing.

## What To Extract

The point is not a screenshot gallery. It is a description of the product's system.

**Screen inventory.** What exists, grouped by area, each linked to its `mobbin_url`.

**Information architecture.** How screens relate: what is top-level, what is nested, what is
modal, what is a separate flow. Draw the tree in text.

**Component vocabulary.** The recurring parts and their rules. "Cards have 12px radius, a
hairline border, no shadow. Buttons come in two sizes only. Every list row is 56px with a
leading icon slot." This is the most transferable output of a teardown.

**Token estimates, as values.** Sampled off the screenshots: canvas, surface, text primary and
secondary, border, accent, and the accent's approximate coverage. Hex or oklch, never
adjectives.

**Density and rhythm.** The spacing scale actually in use, and how tight the product runs.

**Content strategy.** How much copy per screen, the voice, how labels are written, whether
empty states have personality.

**What the product refuses to do.** Often the sharpest finding. A product with no illustration
anywhere, or no color outside one accent, or no modals at all, is making a loud choice.

## Output Format

```text
# Teardown — <product>

Coverage: <n> screens across <n> passes · <what could not be found>
Platform: <ios / web>

## What this product is
<one paragraph: what it does, who for, and the design posture it takes>

## Screen inventory
| Area | Screen | URL | Note |
|------|--------|-----|------|

## Information architecture
<text tree>

## Component vocabulary
| Component | Rules observed |
|-----------|----------------|

## Tokens (estimated from screenshots)
| Role | Value | Coverage / rule |
|------|-------|-----------------|

## Density and rhythm
## Content strategy
## What it refuses to do

## What is worth taking
<the 3-5 transferable traits, and for each, what it would mean in the user's product>

## Coverage gaps
<what Mobbin did not have; do not imply completeness>
```

## After The Teardown

A teardown is research, not a plan. It ends by asking what the user wants to do with it:

- **Take principles** — the usual and best answer. Feeds a normal reference lock.
- **Match closely** — the user wants their product to feel like this one. Go to
  [fidelity-ladder.md](fidelity-ladder.md) and run the intake, because a high-fidelity build
  needs materials the user has not supplied yet.
- **Compete with it** — then the interesting output is what this product *refuses* to do,
  because that is the gap.

Do not slide from teardown into building a copy of the product you just inventoried. The
fidelity ladder exists to make that boundary explicit rather than accidental.
