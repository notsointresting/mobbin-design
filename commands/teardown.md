---
description: Inventory what a real product contains — screens, IA, component vocabulary, tokens — from Mobbin.
argument-hint: "[app or site name] [ios|web]"
---

Run a product teardown of: **$ARGUMENTS**

Follow `skills/mobbin-design/references/product-teardown.md`.

## Steps

1. **Resolve the platform** (`ios` or `web`) — required on every screen and flow search. Infer
   it if the product is obviously one or the other.

2. **Run the six passes** with named-app queries: core screens, structure, account surfaces,
   the edges (empty / error / loading / confirmation), journeys via `mobbin_search_flows`, and
   marketing sections for web products.

   Walk past each result set with `exclude_screen_ids`. Stop a pass when it returns screens
   you have already seen twice — that is the coverage ceiling.

3. **Batch of 2-3, notes as text between batches.** A teardown generates many images and the
   notes are the deliverable. Text survives compaction; images do not.

4. **Extract the system, not a gallery**: screen inventory with `mobbin_url` links, IA tree,
   component vocabulary with observed rules, token values sampled as hex or oklch, density and
   rhythm, content strategy, and what the product deliberately refuses to do.

5. **Report** using the template in `product-teardown.md`, and state coverage gaps explicitly.
   A teardown samples what Mobbin indexed; it is never a complete inventory.

6. **Ask what happens next**: take principles (usual), match closely (run the intake in
   `references/fidelity-ladder.md`), or compete (the gap is what the product refuses to do).

## Do not

- Claim completeness. Say what you covered and what you could not find.
- Report trending or popular apps — the MCP surface cannot provide that. Ask the user to
  browse Mobbin and paste names instead.
- Design from metadata. Examine the returned images.
- Slide from teardown into building a copy of the product you just inventoried.
