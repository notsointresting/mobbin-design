---
description: Run Mobbin design research end to end and write a DESIGN.md lock, without writing implementation code.
argument-hint: "[what you are designing, for whom, on which platform]"
---

Run the full Mobbin research protocol for: **$ARGUMENTS**

Use the `mobbin-design` skill as the methodology. This command stops before implementation —
the deliverable is research and a committed lock, not code.

## Steps

1. **Brief.** Form the design brief. Ask only for information that would materially change the
   result; otherwise state reasonable assumptions and proceed. Resolve the platform (`ios` or
   `web`) — every screen and flow search requires it.

2. **Direction pass.** 3-5 `mobbin_search_screens` queries for aesthetic character, mostly by
   naming products whose design language is strong and known. No structural terms in these
   queries.

3. **Pattern pass.** Separate queries for the literal UI to be built: page type, component,
   state, on-screen text. No aesthetic adjectives here.

4. **Flow pass**, if the task has multiple steps. **Section pass**, if it is a marketing page.

5. **Batch discipline.** Run searches 2-3 at a time and write observations as text before the
   next batch — app name, `mobbin_url`, layout, type, sampled hex values, the one detail worth
   taking. Images leave context; text does not. Do not accumulate everything and synthesize at
   the end.

6. **Synthesize.** One primary direction, 1-2 borrowed details, nothing averaged. Sample real
   color values rather than describing colors as adjectives.

7. **Write `DESIGN.md`** at project root, following
   `skills/mobbin-design/references/design-md.md`. Fill every section: brief, reference lock,
   motion lock, tokens with role rules, decision ledger, research index.

   The motion lock cannot be filled from Mobbin — it returns stills. Source it from
   `references/motion-library.md` or derive it from the visual lock, and name the signature
   move.

8. **Report** the research summary with every screen linked by `mobbin_url`, then stop and
   confirm the direction with the user before any implementation begins.

## Do not

- Write implementation code. This command ends at the lock.
- Design from metadata. Examine the returned images.
- Average the references into a safe middle.
- Leave the motion lock empty because Mobbin had no motion data.
