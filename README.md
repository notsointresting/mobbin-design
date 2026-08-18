# Mobbin Design

A Claude Code skill that makes design research mandatory before implementation, grounded in
real shipped product interfaces from [Mobbin](https://mobbin.com).

Without it, an agent asked to "design a pricing page" writes one from memory — and model
memory of good design converges on the same indigo gradient, the same hero → features →
pricing → FAQ → CTA skeleton, the same rounded card grid. With it, the agent researches
what real products actually shipped, locks a specific direction, and can tell you which
screen every decision came from.

## Install

### As a Claude Code plugin

```text
/plugin marketplace add notsointresting/mobbin-design
/plugin install mobbin-design@mobbin-design
```

This installs the skill and registers the Mobbin MCP server. Run `/mcp` and sign in to
Mobbin when prompted.

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

### Decision ledger

Every significant choice traces to a source before build starts:

| Decision | Source | Source rule / role | Why |
|---|---|---|---|
| Single blue, selection state only | Things 3 direction screens | accent never becomes CTA or badge fill | Keeps the one moment of color meaningful |

A row that traces to nothing does not ship.

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

## What is included

```
skills/mobbin-design/
  SKILL.md                        the methodology and tool routing
  references/
    mcp-tools.md                  full parameter reference for the three Mobbin tools
    example-workflow.md           an end-to-end worked example
    typography.md   color.md      craft references, loaded on demand
    motion.md       icons.md
    craft-details.md              forms, focus, images, touch, performance, a11y
    copywriting.md                copy and persuasion
    anti-ai-slop.md               generic-AI-design checks
    visual-workflow.md            image generation, visual options, visual QA
.mcp.json                         Mobbin MCP endpoint
.claude-plugin/                   Claude Code plugin + marketplace manifests
```

The craft references load only when relevant — the skill does not pull all of them into
context by default.

## Gotchas worth knowing

- `search_screens` has **no `page` parameter.** To get past a result set, re-query with
  `exclude_screen_ids` populated from what you already reviewed.
- `search_sections` has **no `platform` parameter.** Sections are web only.
- `mode: "fast"` is a deprecated alias for `"standard"`. Use `"deep"` (the default) for
  anything nuanced.
- Images cost context. Keep `limit` around 8–12 while exploring; several sharp queries beat
  one broad query with a high limit.

## Credits

The research methodology — reference locks, decision ledgers, anti-averaging rules, the
quality gate — is adapted from the MIT-licensed
[Refero design skill](https://github.com/referodesign/refero_skill), and the general design
craft references are reused from it with minor edits. The Mobbin research layer, tool
routing, and workflow were rewritten for Mobbin's tool surface, which is materially
different: Refero returns structured style documents, Mobbin returns screenshots.

See [NOTICE](NOTICE) for the per-file attribution.

This is an independent, unofficial integration. It is not affiliated with or endorsed by
Mobbin.

## Security

See [SECURITY.md](SECURITY.md). This repo ships no executable code and stores no
credentials.

## License

MIT — see [LICENSE](LICENSE).
