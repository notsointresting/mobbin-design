# Mobbin Design

A Claude Code skill that makes design research mandatory before implementation, grounded in
real shipped product interfaces from [Mobbin](https://mobbin.com).

Without it, an agent asked to "design a pricing page" writes one from memory — and model
memory of good design converges on the same indigo gradient, the same hero → features →
pricing → FAQ → CTA skeleton, the same rounded card grid. With it, the agent researches
what real products actually shipped, locks a specific direction, and can tell you which
screen every decision came from.

Research alone does not clear the bar, though. Mobbin returns **still screenshots**, so a
purely research-grounded build tends to arrive static and safe: correct structure, no
signature. The skill closes that gap with a motion lock, a required signature interaction,
and worked code exemplars that set the quality target with artifacts rather than adjectives.

## Install

### As a Claude Code plugin

```text
/plugin marketplace add notsointresting/mobbin-design
/plugin install mobbin-design@mobbin-design
```

This installs the skill, three slash commands, and registers the Mobbin MCP server. Run `/mcp`
and sign in to Mobbin when prompted.

### Commands

| Command | What it does |
|---------|--------------|
| `/mobbin-research <what you are designing>` | Runs the full research protocol and writes a `DESIGN.md` lock. Stops before implementation — the deliverable is research, not code. |
| `/design-audit <file, directory, URL, or screenshot>` | Reads an existing build, infers its system, researches its specific weaknesses, and reports classified, prioritized findings. |
| `/teardown <app or site> [ios\|web]` | Inventories what a real product contains — screen inventory, IA tree, component vocabulary, sampled tokens, and what it deliberately refuses to do. |

The skill also activates automatically on design work; the commands exist for when you want
research and building to happen on different days, or when the target already exists.

### As a standalone skill

Use this if you already have Mobbin MCP configured, or you only want the methodology:

```bash
npx skills add https://github.com/notsointresting/mobbin-design --skill mobbin-design
```

### MCP server only

```bash
claude mcp add --transport http mobbin https://api.mobbin.com/mcp
```

A Mobbin account is required for live research. The bundled craft references work without
one.

## How it works

Mobbin exposes three search tools, and the skill maps each to a research layer:

| Layer | Tool | Platforms | Answers |
|-------|------|-----------|---------|
| **Screens** | `search_screens` | iOS, web | What should this interface contain, and what should it feel like? |
| **Flows** | `search_flows` | iOS, web | What are the steps, and what happens at each one? |
| **Sections** | `search_sections` | web | How should a marketing page be structured? |

Every search returns **rendered screenshots inline**. That shapes the whole method: there is
no detail-fetch call, no style-extraction call, no similarity call. Search *is* retrieval,
and the research happens by looking at the images. The skill's central rule is that you may
not describe or design from metadata — app names and pattern tags tell you what a screen is
called, not how it works.

### The two-pass search

Mobbin has no style tool, so visual direction is derived from screens using a separate query
pass from the one that finds structure:

```
Direction pass:  "Things 3 task list typography"          → taste, density, accent discipline
Pattern pass:    "pricing page annual monthly toggle"     → structure, states, copy
```

Mixing them ("beautiful minimal pricing page") weakens both, because Mobbin matches on what
is visibly on the screen.

### Reference lock

Research only helps if it survives implementation. Before any code is written, the skill
commits to a lock:

```text
Primary reference:  [one dominant source]
Preserve:           [3-5 traits that must survive]
Borrow only:        [1-2 specific details from secondary sources]
Role rules:         [CTA-only, code-only, decorative-only …]
Reject:             [the defaults that would collapse this direction]
```

The `Reject` line is the one that earns its keep. It is where "no indigo gradient, no streak
grid, no countdown urgency" gets written down so it cannot creep back in at hour three.

### Motion lock

Mobbin holds no motion data, so motion gets its own lock with its own evidence rule — a
named product's motion language, or a character derived from the visual lock:

```text
Motion direction:   [named source, described concretely]
Surface tier:       [product UI / navigation / onboarding / marketing] → ceiling
Signature move:     [required — one interaction that is not a utility class]
Easing family:      [one curve + the duration scale derived from it]
Deliberately still: [what does NOT move, and why]
Memorable detail:   [the one thing a user could describe tomorrow]
```

Marketing and hero surfaces default **up** the motion tiers; dense product UI defaults
**down**. Applying product-UI restraint to a landing page is the most common cause of
basic-looking output, and an empty `Signature move` line means the build is not ready to
start.

### DESIGN.md — the lock that survives compaction

A lock that lives only in conversation dies at the first compaction or session boundary. Turn
40 then has no access to what turn 3 decided, re-derives from model priors, and lands back on
the average. That is how locked directions drift — not by being overruled, by being forgotten.

For work that outlives one session, the skill writes `DESIGN.md` to project root before
implementing: brief, reference lock, motion lock, a token table with **role rules**, the
decision ledger, and a research index of every screen reviewed with its one-line finding. It
is read back at the start of every subsequent design turn, and the reject list and role rules
are restated before any code is written.

The research index is the quiet win — without it, session three re-runs session one's
searches, gets a different result set, and locks a different direction by accident.

### How close is it supposed to be?

"Make it like Linear" is four different requests. The skill names them rather than answering
only one:

| Level | From the reference | From you |
|---|---|---|
| Inspired-by | one trait | everything else |
| **Genre-match** (default) | density, structure, conventions | brand, content, identity |
| High-fidelity | layout, spacing, type scale, motion character | brand, copy, imagery, content |
| Identical clone | — not built | — |

Level 3 is what most people mean by "identical", and it is legitimate. Level 4 is not built:
logos, wordmarks, custom typefaces, and illustration systems are protected, and a product
indistinguishable from a competitor gives nobody a reason to pick it.

The useful part is the **intake**. Most "make it like X" builds fail because the user supplies
a reference and nothing else, so the agent invents copy and content — and invented content is
what reads as a template no matter how precise the layout is. So a high-fidelity request
triggers one intake block up front: brand files and colors, real headlines and CTA text,
assets or an explicit "none", the surfaces needed, which 3-5 reference traits actually matter,
which traits to *drop*, and a `DO NOT CHANGE` line for anything already shipped. Everything
has a default, and every gap is named with the fidelity it costs.

### Teardowns, and what Mobbin can't do

`/teardown Linear web` runs six systematic passes — core screens, structure, account surfaces,
the edges, journeys, marketing — and returns a screen inventory, an IA tree, the component
vocabulary with observed rules, sampled token values, and what the product deliberately
refuses to do.

Two honest limits, stated in the output rather than papered over:

- **There is no "all screens for app X" call.** A teardown samples what Mobbin indexed, via
  repeated named-app queries walked forward with `exclude_screen_ids`. It reports its coverage
  gaps.
- **Trending and popular are not available.** The official server has three search tools and
  no browse, category, or popularity surface, and `mobbin.com` returns 403 to programmatic
  fetches. Asked for trending, the skill says so and asks you to paste names instead of
  inventing a list.

### Drift check, mid-build

The quality gate runs before handoff, by which point drift has been compounding for hours.
`drift-check.md` runs at each completed surface instead, and most of it is mechanical:

```bash
grep -rhoE '#[0-9a-fA-F]{3,8}\b' src/ | sort | uniq -c | sort -rn
```

A build with a locked seven-value palette that greps twenty-three distinct hex codes has
drifted, and the count says by how much before you have looked at a single pixel. Reject-list
hits and token-role escapes are fixed immediately, because every subsequent component copies
the drifted pattern beside it.

### Decision ledger

Every significant choice traces to a source before build starts:

| Decision | Source | Source rule / role | Why |
|---|---|---|---|
| Single blue, selection state only | Things 3 direction screens | accent never becomes CTA or badge fill | Keeps the one moment of color meaningful |

A row that traces to nothing does not ship.

### The quality gate names things

A yes/no checklist passes on a flat build — every answer can be "yes" while the work is
still generic. So the gate asks for names instead:

1. Name the signature interaction, and why it fits this product.
2. Name the motion system: easing family, duration scale, what deliberately does not move.
3. Name the one detail a user would describe from memory tomorrow.
4. Name the Failure Gallery row this build came closest to, and what you did instead.
5. Name the sources behind every major choice.
6. Name the first visible difference between this and a default starter template.

An answer you cannot write down does not exist. "The colors" is a failing answer to #6.

## What it deliberately does not do

- **Does not average references.** If one reference is dark, one is acid, and one is serif,
  the answer is not warm cream with a polite serif. One direction dominates; the others
  contribute named details only.
- **Does not copy one product.** Synthesis, not cloning.
- **Does not repurpose tokens.** A CTA color stays a CTA color; a syntax-highlight color
  stays inside code blocks.
- **Does not fake imagery.** If a direction needs a photograph and there is no budget for
  one, it keeps an honest sized placeholder with art direction rather than a CSS gradient
  pretending to be a hero image.
- **Does not accept "clean and minimal" as a finished answer.** Restraint is only valid when
  the surface tier calls for it, and the tier has to be named.
- **Does not treat Mobbin's missing motion data as permission to ship static work.** The gap
  changes where motion is sourced from, not whether it exists.
- **Does not assume greenfield.** Most real work has a build already. The audit path reads
  what exists and names its strongest trait before criticizing anything, because a redesign
  that discards a build's one good instinct is worse than no redesign.
- **Does not trust its own memory across sessions.** The lock is a file, not a recollection.

## What is included

```
commands/
  mobbin-research.md              /mobbin-research — research to a committed lock, no code
  design-audit.md                 /design-audit — audit an existing build
  teardown.md                     /teardown — inventory what a real product contains
skills/mobbin-design/
  SKILL.md                        the methodology and tool routing
  references/
    exceptional-bar.md            REQUIRED — worked code exemplars + Failure Gallery
    motion-signature.md           REQUIRED — surface tiers, signature moves, motion lock
    design-md.md                  the DESIGN.md lock template and its rules
    fidelity-ladder.md            how close a match is intended + the user intake spec
    product-teardown.md           inventory a named product: screens, IA, components, tokens
    design-audit.md               brownfield protocol — read the build, then research
    drift-check.md                mid-build mechanical check against the reject list
    motion-library.md             named motion languages, described concretely enough to cite
    mcp-tools.md                  full parameter reference for the three Mobbin tools
    example-workflow.md           an end-to-end worked example
    typography.md   color.md      craft references, loaded on demand
    motion.md       icons.md      motion.md is the safety/restraint half
    craft-details.md              forms, focus, images, touch, performance, a11y
    copywriting.md                copy and persuasion
    anti-ai-slop.md               generic-AI-design checks
    visual-workflow.md            image generation, visual options, visual QA
.mcp.json                         Mobbin MCP endpoint
.claude-plugin/                   Claude Code plugin + marketplace manifests
```

The two marked REQUIRED load before implementation on any surface with visible design
intent. The rest load only when relevant — the skill does not pull all of them into context
by default.

`exceptional-bar.md` is the one that does the most work. It holds five worked exemplars —
scroll-linked hero, coordinated card hover, shared-element tab morph, a restrained data
surface, and the memorable detail — each with a note on the transferable move, plus a Failure
Gallery cataloguing what "basic" actually looks like in output. Adjectives compress to
nothing during generation; a working artifact in context does not.

## Gotchas worth knowing

- `search_screens` has **no `page` parameter.** To get past a result set, re-query with
  `exclude_screen_ids` populated from what you already reviewed.
- `search_sections` has **no `platform` parameter.** Sections are web only.
- `mode: "fast"` is a deprecated alias for `"standard"`. Use `"deep"` (the default) for
  anything nuanced.
- Images cost context. Keep `limit` around 8–12 while exploring; several sharp queries beat
  one broad query with a high limit.
- **Analyze in batches of 2–3 and write notes as text before searching again.** Text survives
  context compaction; base64 images do not. Accumulating every result and synthesizing at the
  end means the images are gone when it matters, and the agent silently falls back to
  designing from metadata — the exact failure the skill exists to prevent.
- There is no color-extraction, taxonomy-filter, or per-app drill-down tool on the official
  server. Sample colors by eye and commit them as hex or oklch values.

## Credits

The research methodology — reference locks, decision ledgers, anti-averaging rules, the
quality gate — is adapted from the MIT-licensed
[Refero design skill](https://github.com/referodesign/refero_skill), and the general design
craft references are reused from it with minor edits. The Mobbin research layer, tool
routing, and workflow were rewritten for Mobbin's tool surface, which is materially
different: Refero returns structured style documents, Mobbin returns screenshots.

Two other Mobbin projects informed later revisions. The incremental batch-analysis rule and
the concrete-visual query formula are adapted from
[ddruids/mobbin-skill](https://github.com/ddruids/mobbin-skill). The manual color-sampling
discipline is adapted from
[pdcolandrea/mobbin-mcp](https://github.com/pdcolandrea/mobbin-mcp), which offered dominant
color extraction as a tool; that project is archived and now points users at Mobbin's
official server, so the technique is kept and the dependency is not.

See [NOTICE](NOTICE) for the per-file attribution.

This is an independent, unofficial integration. It is not affiliated with or endorsed by
Mobbin.

## Security

See [SECURITY.md](SECURITY.md). This repo ships no executable code and stores no
credentials.

## License

MIT — see [LICENSE](LICENSE).
