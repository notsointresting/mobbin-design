# Drift Check — Mid-Build, Not Post-Build

The quality gate runs before handoff. By then, drift has been compounding for hours, and
fixing it means unpicking work that already depends on it.

Drift is not a taste failure. It is generation pressure: each individual accommodation is
reasonable, and the sum of them is a generic build nobody chose. The reject list stops it only
if something re-reads the reject list while there is still time to act.

This check is cheap, mechanical, and runs **during** implementation.

## When To Run It

- After the first screen or section is complete — the earliest point drift is visible
- Every time a new surface, page, or major component is finished
- Immediately after any "let me simplify this" or "a cleaner version" moment, which is where
  distinctive traits get quietly softened
- Before the handoff gate, as a pre-pass so the gate has less to catch

Roughly every 30-60 minutes of build work, or at each natural seam. Not continuously —
checking mid-component makes the component worse.

## The Check

Read `DESIGN.md` (or the in-conversation lock), then answer these against the code as it
actually stands. Not from memory of what you intended.

### 1. Reject list

Take each line of the reject list and grep for it. This is the one part of the check that is
literal and mechanical, and it catches the most.

```bash
# the model-default accent, in every common notation
grep -rniE '#(6366f1|818cf8|8b5cf6|a78bfa|7c3aed|4f46e5)' src/
grep -rniE '\b(indigo|violet|purple)-[0-9]{3}\b' src/

# gradient CTAs and decorative gradients that were not in the lock
grep -rniE 'linear-gradient|bg-gradient-to' src/

# uniform section rhythm — the tell for "no spatial system"
grep -roE 'py-(16|20|24)\b' src/ | sort | uniq -c | sort -rn | head

# every distinct hex in the build; compare against the token table
grep -rhoE '#[0-9a-fA-F]{3,8}\b' src/ | sort | uniq -c | sort -rn
```

That last one is the highest-value command in this file. A build with a locked seven-value
palette that greps twenty-three distinct hex codes has drifted, and the count tells you by
how much before you have looked at a single pixel.

### 2. Token roles

For every role rule in `DESIGN.md`, confirm the token has not escaped its role. The accent is
the usual casualty: locked as selection-only, it appears on a CTA, then a badge, then a hover
background — and each step looked reasonable in isolation.

Grep the accent value and check every call site against its rule.

### 3. Motion

- Is there still more than one easing curve in the build? Count them.
- Did the signature move actually get implemented, or is it still "to do later"?
- Does anything animate that the lock marked deliberately still?
- Do the `IntersectionObserver` callbacks `unobserve` after firing?
- Is there a reduced-motion path, and does it preserve feedback rather than deleting it?

The signature move is the one that quietly never happens. It is the most interesting work and
therefore the most deferred, and a build shipped without it is exactly the basic output this
skill exists to prevent.

### 4. States

Every interactive element needs `:focus-visible` styling distinct from `:hover`. Grep for
`focus-visible` and compare the count against the number of interactive components. A ratio
far below 1:1 means the keyboard path was never built.

Also confirm the states that only appear under real data: empty, loading, error, and
long-content overflow. These get skipped under generation pressure because the happy path
renders fine.

### 5. The distinctiveness question

Look at what has been built so far and answer honestly: **is this still the locked direction,
or has it become the safe version of the locked direction?**

Drift almost never looks like a violation. It looks like:

- a slightly softer radius than specified
- one more gray than the palette had
- the distinctive spacing rhythm regularized to a default scale
- the display type at default letter-spacing because it "looked fine"
- the accent used once more than the role rule allows, because that spot needed emphasis

Each is defensible. The sum is a template.

## Output

Keep it short. This is a checkpoint, not a report.

```text
Drift check — <surface built so far>

Reject list:   <clean / N hits: what and where>
Token roles:   <clean / N escapes: which token, which call site>
Motion:        <signature move: built / not built · easing curves: N · still-list respected: y/n>
States:        <focus-visible ratio · missing states>
Distinctive:   <still the locked direction / softened in these ways>

Fixing now: <the items being corrected before continuing>
Deferred:   <items, and why deferral is safe>
```

## The Rule About Fixing

Fix reject-list hits and token-role escapes **immediately**, before continuing. They are cheap
now and expensive later, because every subsequent component copies the drifted pattern from
the one next to it. That copying is the actual mechanism by which one small compromise becomes
the whole build's character.

Motion and state gaps can be deferred to a dedicated pass, provided they are written down. An
undocumented deferral is not a deferral; it is a thing that will not happen.

## Automating The Cheap Half

The greps above can run as a script in the project. If the build has a lint setup, the reject
list belongs there — a rule that fails CI on a forbidden hex value is worth more than any
amount of instruction, because it does not depend on remembering.

That is the general principle: anything in the lock a machine can check should be checked by a
machine, leaving judgment for the parts that need it.
