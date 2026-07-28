# Engineering Readiness Review — Re-Audit
## TFT Rolldown Trainer — v26 Build vs. PRD v1.4

**Prepared by:** Product Management
**Reviewed build:** `tft-rolldown-trainer-v26.html`
**Spec reference:** TFT Rolldown Trainer PRD v1.4 (July 2026)
**Review type:** Phase 1 (MVP) exit criteria re-audit, following the v25 review

---

## Verdict

**Pass — cleared for Phase 1 sign-off.**

Both items that blocked the previous review are closed. The `W` keybind is implemented, and the two accuracy gaps I flagged last time turned out to be a spec problem, not a code problem — the PRD's pool-size and odds numbers have been corrected to v1.4 to match what the implementation was already doing correctly. I re-verified all three directly against the code rather than taking the changelog's word for it; details below.

Two P2 items remain open from the last review (lazy-loaded art, cross-browser QA) — neither blocks sign-off, but I'd want them closed before we call this fully done.

---

## 1. Goals Scorecard (Section 3.1) — Delta from Last Review

| Goal | Last Review | Now | Notes |
|---|---|---|---|
| **G1** — Statistically accurate shop odds & pools | ⚠️ Partial | ✅ **Pass** | PRD corrected to match the code's already-accurate values. See §2. |
| **G2** — Reproduce in-game default keybinds | ❌ Fail | ✅ **Pass** | `W` now implemented. See §3. |
| G3–G6 | ✅ Pass | ✅ Pass | No regressions found. |

**6 of 6 goals now met.**

---

## 2. Odds & Pool Sizes — Verified Against Code

I pulled both structures directly from the current build rather than trusting the diff summary:

```
ODDS = {
  7:  [0.19, 0.30, 0.40, 0.10, 0.01],
  8:  [0.15, 0.20, 0.32, 0.30, 0.03],
  9:  [0.10, 0.17, 0.25, 0.33, 0.15],
}
POOL_SIZES = {1: 29, 2: 22, 3: 18, 4: 10, 5: 9}
```

This is an exact match to PRD v1.4 §5.3, including the corrected level 7–9 rows and the Appendix A pool-size headers. No further action needed here — this is closed, not just "improved."

One process note, not a code note: last review I flagged the 29/22 pool sizes as a *code* bug against a PRD that said 30/25. You've since told me the code was right and the PRD was wrong, and the PRD has been corrected accordingly. I want to name that explicitly so it's on record: **the spec was the defect here, not the implementation.** Worth a retro question for the team — how did the PRD's numbers diverge from the values the dev was actually building against in the first place? Not urgent, just worth understanding so it doesn't happen again on the Set 18 migration mentioned in Open Question #3.

---

## 3. `W` Keybind — Verified Implemented, One Spec-Language Note

The blocker is closed. `W` and `E` are both bound, and — importantly — I checked that this isn't cosmetic: `moveUnitViaW()` respects the same board-cap and bench-space rules as drag-and-drop, and it's wired to fire off of mouse **hover**, not just click-selection:

```js
else if (key === 'w') { moveUnitViaW(hoverCell || selectedCell); }
else if (key === 'e') { const cell = hoverCell || selectedCell; ... }
```

This is functionally correct and, honestly, a better match to how the real client feels (hover-and-press, no click required) than what the PRD literally describes.

That's the one thing worth flagging: PRD §5.4 currently says *"Left-click a unit on the bench or board to select it; press `W` to toggle it... or `E` to sell it"* — i.e., a click-to-select model. The shipped behavior is hover-priority with click-selection as a fallback. I'm not asking for a code change — the hover-first behavior is the right call and matches real TFT more closely than a click-select model would have. But the spec text is now describing a different interaction than what's built, and someone reading §5.4 cold would expect click-to-select as the primary path. I'd fold a one-line correction into the next PRD pass (v1.5, alongside Phase 2 scoping) so the doc stops undercounting what's actually a nicer implementation than what was speced.

I also checked the specific board-fill behavior for `W`: bench→board now fills the bottom row first, left to right, before moving up a row. That's a reasonable default and matches typical in-game placement instinct, but it's not something the PRD specifies either way — no compliance issue, just noting it's an implementation choice the spec is silent on.

---

## 4. Non-Functional Requirements — Still Open

Nothing new to report here since these weren't in scope for this round of fixes, but keeping them visible so they don't get lost:

| Req | Status | Notes |
|---|---|---|
| 6.1 Unit art lazy-loaded with placeholder | ❌ Still fail | Confirmed zero `loading="lazy"` attributes in the current build. Still a same-day fix whenever it gets prioritized. |
| 6.3 Cross-browser QA (Chrome/Firefox/Safari/Edge 120+) | 🔍 Still untested | Same note as last review — Safari's `AudioContext` handling is the specific risk area given the v25 sound additions. No evidence of a QA pass having happened between v25 and v26. |

---

## 5. Undocumented Additions Since Last Review

Two more features shipped that aren't in the PRD anywhere. Neither is a problem, but I want the list current:

- **"Owned" shop flash** — a brief white flash on shop slots for units the player already holds a copy of (bench or board), distinct from the existing gold/silver combine-pulse. Nice, low-cost signal-boosting for exactly the kind of fast-scanning G1/G2 are meant to train.
- **Always-buy click fix** — the shop's click/drag purchase handler was silently dropping fast clicks that crossed an 8px jitter threshold and got misread as a cancelled drag. That's now fixed so any mousedown-mouseup on a shop slot always purchases, regardless of movement or release location. This isn't a new *feature* so much as a correctness fix, but it's directly relevant to G2's "practice transfers to the game" goal — a trainer that eats your clicks during fast rolldowns was actively working against the thing it's meant to build.

Same recommendation as last time: at some point, decide whether the growing list of nice-to-haves (infinite gold toggle, owned-flash, combine sounds) gets formally folded into the PRD as shipped scope, or stays as undocumented extras. Not blocking, just growing.

---

## 6. Recommended Action Items

| Priority | Item | Owner |
|---|---|---|
| P2 | Add `loading="lazy"` + placeholder state to shop-slot art (6.1) | Engineering |
| P2 | Cross-browser QA pass, Safari especially | QA |
| P3 | Correct §5.4's "left-click to select" language to reflect the shipped hover-first model | Product |
| P3 | Fold undocumented extras (infinite gold, owned-flash, sound design) into the PRD as officially scoped, or explicitly mark them out-of-spec | Product |

None of these block sign-off. **Recommend closing out Phase 1 and moving planning attention to Phase 2 (pool contestation simulation).**

---

*This review covers the build as of v26. No Phase 2 or Phase 3 scope was in review.*
