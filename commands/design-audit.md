---
description: Audit an existing build against research and its design lock, and report classified, prioritized findings.
argument-hint: "[file, directory, URL, or screenshot to audit]"
---

Audit the design of: **$ARGUMENTS**

Follow `skills/mobbin-design/references/design-audit.md`. Read the build **before** searching
Mobbin — research done first anchors you to an imagined product and produces critique of what
is absent rather than what is there.

## Steps

1. **Read the build and infer its system.** Extract the values actually in use: every distinct
   color, the accent's roles and coverage, type steps and letter-spacing, spacing rhythm,
   radius and elevation, layout structure, every transition with its duration and easing, and
   which interaction states exist.

2. **State two findings before anything else.**
   - Is this a system, or a pile? Values that repeat and mean something, versus twelve grays
     and three easing curves that arrived independently.
   - What is the strongest existing trait? Find it before criticizing, because the job is to
     amplify it, not replace it.

3. **If `DESIGN.md` exists, read it in full** and record every divergence. Each is either a
   drift to fix or a lock to update — classify, do not merely list.

4. **Research the weaknesses you named**, not the product category in general. A targeted
   response to identified problems is what separates an audit from inspiration gathering.
   Batches of 2-3, notes as text between batches.

5. **Classify every finding**: Broken, Inconsistent, Drift, Below the bar, Opportunity, or
   Taste. Mark taste as taste or drop it — presenting preference as defect burns the authority
   needed for the real findings.

6. **Prioritize by effect per unit effort**, P0 to P3. Do not skip the P1 band (type scale,
   spacing rhythm, `:focus-visible`, `tabular-nums`, display letter-spacing) in favor of a
   dramatic P2 rewrite.

7. **Report** using the template in `design-audit.md`, including the "What I would not change"
   section — it tells whoever implements the fixes where the load-bearing walls are.

8. **If the build is a pile rather than a system**, offer to write a `DESIGN.md` from the
   inferred tokens plus the research. Without it, the same audit is needed again in three
   months.

## Do not

- Propose a rewrite when P1 fixes would do.
- Report findings without a specific fix for each.
- Ignore the strongest existing trait.
- Apply fixes unless the user asks — this command reports.
