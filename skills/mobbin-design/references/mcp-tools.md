# Mobbin MCP Tools Reference

MCP clients may namespace tool names, but the Mobbin tools are exposed with the `mobbin_`
prefix (for example `mcp__mobbin__search_screens`). Use the exact tool names shown by your
client.

Mobbin exposes **three tools, all of them searches**. There is no detail-fetch tool, no
style tool, no similarity tool, no color-extraction tool, no taxonomy/filter-facet tool, no
per-app drill-down tool, and no image-by-id tool. Search is retrieval: each call returns
rendered images inline together with metadata, and that is the complete result.

**Mobbin returns stills, and holds no motion data.** Nothing here can tell you how an
interface animates. Motion direction is sourced separately — see
[motion-signature.md](motion-signature.md). This gap is a sourcing instruction, never a
reason to ship a static build.

**Color must be sampled by eye.** With no extraction tool, read canvas, surface, text,
border, and accent values off the screenshot and commit them as hex or oklch, plus the
accent's approximate screen coverage. Adjectives do not survive into implementation; numbers
do.

| Layer | Tool | Platforms | Returns |
|-------|------|-----------|---------|
| Screens | `mobbin_search_screens` | ios, web | Screens with inline images, metadata, and `mobbin_url` |
| Flows | `mobbin_search_flows` | ios, web | Flows with evenly-spaced preview images and per-screen previews |
| Sections | `mobbin_search_sections` | web only | Website sections with inline images and metadata |

## The Rule That Governs All Three

Every tool description says the same thing, and it is the operating premise of this skill:

> Examine the returned images to understand actual content. Do not describe or summarize
> results based solely on metadata.

Metadata names the thing. Pixels are the evidence. Design decisions must come from looking.

## Screens

### `mobbin_search_screens`

Search real UI screens using natural language.

Use for both **visual direction** (run named-product and aesthetic-category queries) and
**concrete interface decisions** (page structure, component choices, content hierarchy,
copy, states). Run those as separate passes with different query styles.

Parameters:

| Parameter | Type | Required | Notes |
|-----------|------|----------|-------|
| `query` | string | Yes | Max 500 chars. Describe one screen in plain language. |
| `platform` | enum | Yes | `ios` or `web`. Do not put the platform in `query`. |
| `limit` | number | No | Default 20, min 1, max 30. Lower it to protect context. |
| `mode` | enum | No | `deep` (default), `standard`, or `fast`. |
| `image_format` | enum | No | `webp` (default) or `jpg` if the client cannot render webp. |
| `exclude_screen_ids` | string[] | No | Screen UUIDs to omit. Max 100. Use to expand past what you have seen. |

**There is no `page` parameter on this tool.** To go beyond the first result set, re-query
with `exclude_screen_ids` populated from the screens you already reviewed, and narrow the
query toward the traits that made the best result strong.

Search modes:

- `deep` (default) - AI-powered pipeline that interprets intent and scores each candidate
  for relevance. Use for nuanced taste and pattern queries. This is almost always what you
  want.
- `standard` - low latency, no relevance re-scoring. Use for obvious lookups where you just
  need examples fast.
- `fast` - deprecated alias for `standard`. Do not use it in new work.

Result fields worth using:

- `mobbin_url` - the canonical Mobbin link for that screen. **Always cite this as a markdown
  link when you mention a screen to the user.**
- screen UUID - feed into `exclude_screen_ids` on follow-up searches.

Good queries:

```text
login screen with biometric authentication
checkout page with promo code field and Apple Pay button
pricing page annual monthly toggle
dashboard empty state
billing settings cancellation modal
Spotify now-playing screen
Linear settings screen dark interface
data table with filters applied
```

Query guidance:

- Describe **one** screen and the elements you would see on it. Detail helps.
- Name a specific app to filter results to that app ("Duolingo profile screen").
- Do not combine multiple screens or intents in one query - search them separately.
- Do not use negations ("without ads"); the search cannot act on them.
- Do not rely on vague style words alone ("modern", "clean") - they match poorly. Name a
  product with that character instead.
- Do not pass disconnected keyword lists.
- Do not put `ios` or `web` in the query text; use the `platform` parameter.

## Flows

### `mobbin_search_flows`

Search multi-step user flows: connected screens showing how a user completes a task.

Use for journey logic: onboarding, checkout, signup, cancellation, upgrade, settings,
account deletion, password reset, and other before/after sequences.

Parameters:

| Parameter | Type | Required | Notes |
|-----------|------|----------|-------|
| `query` | string | Yes | Max 500 chars. Describe one user journey. |
| `platform` | enum | Yes | `ios` or `web`. |
| `limit` | number | No | Default 5, min 1, max 10. Flows carry many images; keep this small. |
| `page` | number | No | Default 1, min 1, max 20. Paginate for more examples. |
| `image_format` | enum | No | `webp` (default) or `jpg`. |

Note the limits differ from screens: flows cap at **10** per call, not 30, because each flow
returns several preview images. The default of 5 is usually right.

Good queries:

```text
onboarding with personalization steps
checkout with payment method selection
subscription cancellation with retention offer
account deletion with confirmation
password reset with email verification
Duolingo onboarding
```

Query guidance:

- Describe the steps and what you would see along the way.
- One journey per query. Two flows in one query returns weak results for both.
- Name a specific app to filter to it.
- No negations, no vague style words, no keyword lists.
- No platform in the query text.

Read the returned step images **in order**. The sequence is the finding: entry point, the
decisions offered, the system response at each step, and the exit state.

## Sections

### `mobbin_search_sections`

Search website sections - About, Pricing, Hero, Footer, Testimonials, FAQ, and similar.

Use when designing a marketing or landing page and you need section-level structure and page
rhythm. This is Mobbin's closest analog to a curated web-style source.

Parameters:

| Parameter | Type | Required | Notes |
|-----------|------|----------|-------|
| `query` | string | Yes | Max 500 chars. Describe one website section. |
| `limit` | number | No | Default 20, min 1, max 30. |
| `page` | number | No | Default 1. Paginate for more examples. |
| `image_format` | enum | No | `webp` (default) or `jpg`. |

**There is no `platform` parameter.** Sections are web only.

Good queries:

```text
pricing page with plan comparison table
hero section with signup form
customer testimonial section with company logos
FAQ accordion section
footer with sitemap columns
```

Query guidance:

- Describe the content and elements you would see in that one section.
- One section per query.
- No negations, no vague style words, no keyword lists.

## Mapping From Structured-Reference Workflows

If you are used to a design-research MCP that exposes search plus detail plus similarity
calls, here is how those steps translate:

| You want | Mobbin equivalent |
|----------|-------------------|
| Full detail for a result | Already in the search response. Look at the image. |
| A curated visual style system | Run named-product / aesthetic screen queries and derive the system by inspection. Use sections for web page-level style. |
| Similar screens to a strong one | Re-run `mobbin_search_screens` with a narrowed query plus `exclude_screen_ids` for what you have seen. |
| Raw screenshot for close inspection | Already returned inline. Set `image_format` and `limit` to control fidelity and volume. |
| Step-by-step flow breakdown | Read the per-screen preview images from `mobbin_search_flows` in order. |
| Page 2 of screen results | Not available. Use `exclude_screen_ids` instead. |

## Context Budget

All three tools return images, so context cost scales with `limit`:

- Exploratory screen searches: `limit` 8-12.
- Confirming a specific pattern: `limit` 5-8.
- Flows: leave `limit` at the default 5; raise only when comparing many products.
- Prefer several sharp queries over one broad query with a high limit.
- Stop searching once the reference lock is set. Extra references past that point pull the
  design toward an average.

## Common Mistakes

- Designing from metadata without examining the returned images.
- Mentioning a screen to the user without linking its `mobbin_url`.
- Passing `page` to `mobbin_search_screens` - that parameter does not exist there.
- Passing `platform` to `mobbin_search_sections` - sections are web only.
- Using `mode: "fast"` - deprecated; use `standard`.
- Putting `ios` or `web` inside the `query` string instead of the `platform` parameter.
- Combining two screens or two journeys into a single query.
- Using negations or vague aesthetic adjectives as the whole query.
- Setting `limit` to the maximum by default and burning context.
- Copying a single reference directly instead of synthesizing several.

## If Results Are Weak

- Broaden the query and remove extra adjectives or constraints.
- Swap a vague aesthetic word for a named product with that character.
- Search adjacent categories or a different platform.
- For screens, re-query with `exclude_screen_ids` to push past a homogeneous first set.
- For flows and sections, request a later `page`.
- Switch `mode` to `deep` if you had used `standard`.
- For sparse flows, search related screens and reconstruct the journey manually.
