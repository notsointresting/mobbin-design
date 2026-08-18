# Example Workflow

A complete walkthrough of the Mobbin research-first method on one realistic task. The point
is the shape of the process, not these specific results - your searches will return
different screens.

**Task from the user:** "Design the paywall and the onboarding that leads into it for my iOS
habit tracker. It's for people who've failed at habit apps before."

## 1. Brief

```text
Designing onboarding + paywall for lapsed habit-app users on iOS.
Goal: reach a confident subscribe decision without another shame spiral.
Tone: calm, forgiving, non-gamified. Competent rather than cute.
Main objection/risk: "I've paid for one of these before and quit in a week."
Must remember: the app should feel like it expects you to miss days.
Constraints: iOS native, solo dev, no illustration budget.
Research needed: screens (direction + pattern), flows (onboarding to paywall).
Path: visual exploration - three directions, user picks.
```

Note what the brief already rules out: streak-shaming and heavy gamification are off the
table because the audience is defined by having failed at exactly that.

## 2. Direction Pass (screens)

Aesthetic character only. Named products whose design language is known and strong. No
structural terms yet.

```
mobbin_search_screens  platform=ios  limit=10  query="Things 3 task list typography"
mobbin_search_screens  platform=ios  limit=10  query="Oak meditation timer calm minimal screen"
mobbin_search_screens  platform=ios  limit=10  query="Bear notes app writing screen"
mobbin_search_screens  platform=ios  limit=10  query="habit tracker weekly progress screen"
```

Then **look at all forty screenshots**. Not the app names - the pixels.

What inspection turns up (stated as reasoned estimates, because these are screenshots and
not token files):

- Things 3: generous line height, roughly 20-24px vertical rhythm between rows, one blue
  accent used *only* for the active/selected state, everything else near-black on white.
  Type carries the whole hierarchy; almost no borders.
- Oak: near-black canvas, very large thin numerals, a single warm amber accent, enormous
  negative space. Calm comes from emptiness, not from soft colors.
- Bear: warm off-white, serif display over sans body, hairline dividers, content-forward.
- Generic habit trackers: dense 7-column grids, multi-color streak dots, badge clutter.
  These are the anti-reference - exactly the shame machinery the audience quit.

The last query earned its place by showing what to reject. Searching for the thing you are
replacing is a legitimate use of the direction pass.

## 3. Pattern Pass (screens)

Now structure. Concrete UI language, no aesthetic adjectives.

```
mobbin_search_screens  platform=ios  limit=8  query="subscription paywall with annual and monthly plan toggle"
mobbin_search_screens  platform=ios  limit=8  query="paywall screen with free trial timeline explainer"
mobbin_search_screens  platform=ios  limit=8  query="onboarding screen asking user to pick a goal"
```

Wanting more paywall examples after the first set looks samey - screens has no `page`, so
exclude what you have seen and narrow:

```
mobbin_search_screens  platform=ios  limit=8
  query="paywall with restore purchases and terms links below plan cards"
  exclude_screen_ids=["<uuid-1>", "<uuid-2>", "<uuid-3>"]
```

Findings from looking:

- Plan toggle sits *above* the cards; annual is pre-selected and carries a savings badge.
- The strongest trial paywalls show a three-stage timeline (today / day 5 reminder / day 7
  charge). This directly answers "I'll forget and get charged."
- Restore + Terms + Privacy sit as small centered links under the CTA, never in a nav bar.
- Goal-selection onboarding screens use 3-5 large tappable rows, single-select, no icons
  needed.

## 4. Flow Pass

```
mobbin_search_flows  platform=ios  limit=5  query="onboarding with personalization steps leading to paywall"
mobbin_search_flows  platform=ios  limit=5  query="free trial subscription signup with plan selection"
```

Reading step images in order:

- Median is 4-6 onboarding steps before the paywall appears.
- The best ones ask 2-3 personalization questions, then **reflect the answers back** on the
  paywall ("Your plan: 3 habits, evening check-ins"). Commitment before price.
- A soft dismissal always exists - an X or "Maybe later" - and the ones that hide it feel
  like a trap.
- Permission prompts (notifications) come *after* the value is shown, never on step one.

## 5. Reference Lock

```text
Primary reference/direction: Things 3 - typographic hierarchy, near-white canvas,
  single-accent discipline  [link its mobbin_url]
Preserve: near-white canvas; type-driven hierarchy with no decorative borders;
  ONE accent reserved exclusively for selected/active state; generous 20-24px row rhythm;
  no icons where a word will do
Borrow only: Oak's oversized thin numeral for the single "days" figure on the paywall;
  the three-stage trial timeline pattern from the paywall pattern pass
Role rules: the accent is selection-state only - it never becomes a background, a badge
  fill, or the CTA color. CTA is near-black. Oak's numeral treatment applies to exactly
  one figure per screen or it stops reading as emphasis.
Media strategy: code-native only. No illustration budget, so zero illustration slots -
  type and space do the work. No stock photography.
Reject: 7-column streak grids; multi-color dot systems; confetti or badge rewards;
  gradient CTAs; indigo/violet defaults; countdown-timer urgency
Token commitments: bg #FCFCFA warm-neutral; text near-black #1A1A18; accent single blue
  #2E6BE6 (selection only); radius 10px; hairline #E8E8E4 dividers only, no shadows
```

The reject list is doing real work here: it is the shame machinery the audience already
quit, written down so it cannot creep back in during implementation.

## 6. Decision Ledger

| Decision | Source | Source rule / role | Why |
|---|---|---|---|
| Warm near-white canvas, type-led hierarchy | Things 3 direction screens | canvas + type own the system; no border chrome | Calm without going dark; audience wants competent, not moody |
| Single blue, selection state only | Things 3 accent discipline | accent never becomes CTA or badge fill | Keeps the one moment of color meaningful; blocks badge creep |
| Near-black CTA | Reference lock role rule | CTA is not the accent | Prevents the accent from being diluted into a button |
| Three-stage trial timeline on paywall | Paywall pattern pass | timeline is an explainer block, not decoration | Directly answers the "I'll get charged unnoticed" objection |
| Annual pre-selected, toggle above cards | Paywall pattern pass | toggle above, savings badge on annual | Convention users already read fluently; no invention needed |
| 3 personalization steps, answers echoed on paywall | Flow pass | reflect answers before showing price | Commitment before price; makes the paywall feel earned |
| Visible "Maybe later" dismissal | Flow pass | soft exit always present | A trapped user churns; audience is already trust-damaged |
| Notification prompt after value, not step 1 | Flow pass | permission follows demonstrated value | Higher opt-in and avoids an early hostile moment |
| Copy: "Miss a day? Nothing resets." | User brief + reject list | product promise, stated plainly | The memorable move; it is the whole positioning in four words |
| Zero illustration slots | User constraint | code-native only | No budget - an honest type-led screen beats a bad clipart one |

Every row traces to a source. A row that traced to nothing would come out.

## 7. Three Directions, User Picks

Because this is a high-visibility surface with several plausible answers, present three
reference-locked options rather than one:

1. **Quiet Ledger** - Things 3 foundation, pure type hierarchy, the oversized numeral on
   total days practiced. Calm, adult, no ornament.
2. **Single Card** - one elevated plan card on warm canvas, timeline inside it, everything
   else stripped. Decision-forcing, fewest choices.
3. **Evening Dark** - Oak-led near-black canvas, amber accent, for an app used at night.
   Departs from the primary lock; included because the use case genuinely suggests it.

Stop here and let the user choose. The chosen option becomes the build target.

## 8. Visual QA After Build

```text
Source truth: reference lock + selected direction (Quiet Ledger)
Implementation evidence: simulator screenshots, iPhone 15 / iOS 17, light + dark
Viewport/state: onboarding steps 1-3, paywall default, paywall annual-selected, dismissal
Final result: [passed / blocked]
```

Findings from a real pass usually look like:

- `P1` - accent blue leaked onto the subscribe button. Violates the accent role rule.
  Revert CTA to near-black.
- `P2` - row rhythm compressed to 14px on the goal-selection screen; the lock says 20-24px.
- `P2` - two oversized numerals on the paywall. The borrowed Oak treatment allows one.
- `P3` - orphan word in the paywall headline at 375pt. Apply balanced text wrapping.

Do not hand off with P0/P1/P2 open unless genuinely blocked.

## What To Carry Away

- Direction and pattern are **two separate query passes**. Mixing taste words with
  structural words in one query weakens both.
- The research is the **looking**. Four searches returning forty screenshots is worth
  nothing if you designed from the app names.
- Search for the thing you are rejecting. It sharpens the lock.
- The reject list prevents the slow slide back to generic during implementation.
- `exclude_screen_ids`, not `page`, is how you get more screens.
- Link every screen you cite so the user can check your reasoning.
