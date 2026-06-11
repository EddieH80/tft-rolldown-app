# Product Requirements Document
## TFT Rolldown Trainer — Browser App
**Version:** 1.3  
**Status:** Draft  
**Author:** Product Management  
**Last Updated:** June 2026  

---

## 1. Executive Summary

TFT Rolldown Trainer is a browser-based muscle-memory tool that simulates the Teamfight Tactics shop experience at full fidelity — accurate Set 17 unit pool odds, real unit art, a simulated hex board, and default in-game keybinds — so that players can drill rolldown execution in isolation from the cognitive overhead of an active game. The primary audience is intermediate-to-high-elo players who understand *what* to buy but want to improve *how fast* they can buy it.

The app's core value proposition is specificity: unlike generic "game sense" training tools, Rolldown Trainer targets one of the highest-leverage mechanical skills in TFT — the ability to execute a full-economy rolldown on a 30-second planning timer without misclicks, missed units, or incorrect purchases.

---

## 2. Problem Statement

### 2.1 The Skill Gap

Rolling down efficiently in TFT requires a player to simultaneously track five shop slots, compare each unit against a pre-configured team planner, click or keyboard-shortcut the correct purchases, and manage gold economy — all within a 30-second round planning phase. Even Challenger-level players lose value here through:

- Misidentifying a unit in a fast shop scan (e.g., confusing similarly-named units at the same cost tier)
- Buying a unit not on the team plan by accident
- Losing rhythm mid-roll and failing to complete the full rolldown before combat starts
- Running out of bench space and failing to manage the board efficiently during the rolldown

### 2.2 No Existing Tool Solves This

Current TFT training resources fall into two categories: (1) gameplay coaching, which embeds rolldown practice inside a full game's cognitive load, and (2) static reference sites, which list odds and unit pools but offer no interactive simulation. There is no dedicated, keybind-accurate, odds-accurate rolldown simulator that also recreates realistic board and bench constraints.

---

## 3. Goals and Non-Goals

### 3.1 Goals

- **G1.** Simulate the TFT shop at statistically accurate odds for all ten player levels using Set 17 unit pools.
- **G2.** Reproduce the in-game default keyboard controls so that practice in the tool directly transfers to the game.
- **G3.** Allow the player to configure a team planner (units they are targeting) before each session, mirroring the in-game TFT team planner with full trait synergy display.
- **G4.** Simulate a hex board and bench so that board management during a rolldown reflects real in-game constraints.
- **G5.** Provide post-session performance metrics focused on missed target units and roll speed.
- **G6.** Require zero installation — runs fully in-browser, no account needed.

### 3.2 Non-Goals

- This is not a full TFT simulator. Combat, items, and positioning optimization are explicitly out of scope for v1.
- This is not a tier list or strategy guide. The app does not tell players *what* to build.
- Mobile is not a target platform for v1. The keybind-centric design assumes a PC keyboard.
- Multiplayer / shared leaderboards are not in scope for v1.

---

## 4. User Personas

### 4.1 Primary: The Mechanical Grinder
**"I know the game — I just need my hands to keep up with my brain."**

High-elo player (Diamond+) preparing for a ranked session or tournament. Already knows which compositions they want to play. Wants 5–10 minutes of focused rolldown reps before queuing. Values missed-target metrics and rep volume.

### 4.2 Secondary: The Climbing Mid-Elo Player
**"I know I should be rolling faster, but I panic when I see units go by."**

Gold–Platinum player who has learned basic game concepts but whose rolldown pace is visibly costing them. Uses the tool to build pattern recognition for which shop slots to focus on and to practice board management under time pressure.

### 4.3 Tertiary: The Casual Enjoyer
**"I just like the feeling of rolling. It's satisfying."**

A player of any skill level who finds the rolling mechanic itself enjoyable — the rhythm of rerolling the shop, watching units appear, and snapping up finds. This player may not be drilling a specific comp and may use the app with no team planner configured. They are less metrics-driven and more likely to share the app socially or use it as a low-stakes way to wind down. Supporting this persona means ensuring the core loop feels good with no configuration required (free roll mode should be the zero-friction entry point).

---

## 5. Functional Requirements

### 5.1 Session Configuration (Pre-Roll)

Before each training session, the player configures:

| Setting | Options | Default |
|---|---|---|
| Player Level | 1–10 | 8 |
| Starting Gold | 10–100 | 50 |
| Target Units (Team Planner) | Multi-select from all Set 17 units via the in-app planner | None (free roll mode) |
| Roll Timer | 30s / 60s / Unlimited | 30s |
| Show Post-Session Stats | Checkbox (enabled / disabled) | Enabled |

The level selection drives shop probability weights. See Section 5.3. When Post-Session Stats is unchecked, the session ends immediately after the timer expires or the player manually stops — no summary screen is shown and the player is returned directly to the session configuration screen. This setting is intended for casual players who want to use the rolling loop without performance feedback.

### 5.2 Shop Simulation

The shop displays five unit slots per roll, matching the in-game layout. Each slot renders:

- The unit's splash art (sourced from the Riot CDN at `https://cdn.communitydragon.org/` or equivalent)
- The unit's name and gold cost
- A visual cost indicator (matching in-game color coding: gray/1-cost, green/2-cost, blue/3-cost, purple/4-cost, gold/5-cost)
- A small team planner icon on any unit that exists in the player's configured team planner

Units are drawn according to real Set 17 probabilities (see Section 5.3) and the correct shared-pool sizes. When a unit is purchased from the shop, it is removed from the pool for that session, reducing subsequent odds correctly. Units not purchased are returned to the pool on reroll.

### 5.3 Shop Odds — Set 17

The following probability distributions by player level govern slot generation:

| Level | 1-Cost | 2-Cost | 3-Cost | 4-Cost | 5-Cost |
|---|---|---|---|---|---|
| 1 | 100% | 0% | 0% | 0% | 0% |
| 2 | 100% | 0% | 0% | 0% | 0% |
| 3 | 75% | 25% | 0% | 0% | 0% |
| 4 | 55% | 30% | 15% | 0% | 0% |
| 5 | 45% | 33% | 20% | 2% | 0% |
| 6 | 30% | 40% | 25% | 5% | 0% |
| 7 | 19% | 30% | 35% | 10% | 1% |
| 8 | 18% | 25% | 32% | 30% | 3% |
| 9 | 10% | 20% | 25% | 35% | 10% |
| 10 | 5% | 10% | 20% | 40% | 25% |

**Pool Sizes (shared across all 8 simulated players):**
- 1-cost: 30 copies per unit
- 2-cost: 25 copies per unit
- 3-cost: 18 copies per unit
- 4-cost: 10 copies per unit
- 5-cost: 9 copies per unit

The simulation starts with a solo player drawing from a full pool. No other simulated players draw from the pool in v1 — this intentionally maximizes the training value of seeing target units appear, which is the correct focus for rolldown drilling.

### 5.4 Keyboard Controls

The app binds identically to TFT's default PC keybinds:

| Action | Keybind |
|---|---|
| Refresh shop (Reroll) | `D` |
| Buy XP | `F` |
| Move selected unit to board | `W` |
| Sell selected unit | `E` |

Mouse: Left-click on any shop slot purchases that unit and places it on the bench. Left-click a unit on the bench or board to select it; press `W` to toggle it between bench and board, or `E` to sell it.

All keyboard inputs must register without focus on any text field. The keybind layer captures inputs at the document level.

### 5.5 Team Planner

The team planner mirrors the in-game TFT team planner experience. It is accessible from the session configuration screen and as a persistent panel during the rolldown session.

**Planning interface behavior:**
- Displays all Set 17 units browseable by cost, trait, or name search
- The player can add up to 10 units to their planned composition
- As units are added, active trait synergies are displayed (e.g., "Arbiter 2/3," "Vanguard 2/6") matching the in-game trait counter format
- Trait counts update live as units are added or removed

**During the rolldown session:**
- The team planner panel remains visible as a sidebar
- Units added to the planner have a small icon indicator on their shop slot card when they appear in the shop, enabling rapid visual scanning without requiring the player to read names
- No copy-count tracking or star-level progress is shown — the planner's role is purely to guide purchase decisions, not to track completion state

### 5.6 Hex Board and Bench

The simulation includes both a hex board and a 9-slot bench, reflecting the realistic spatial constraints a player faces during a rolldown.

**Board:**
- A standard 4-row × 7-column hexagonal grid matching the in-game TFT board layout
- Units purchased from the shop land on the bench by default
- Pressing `W` with a unit selected moves it from bench to board, or from board to bench
- The board has a unit cap matching the player's current level (e.g., level 8 = 8 units on board)

**Bench:**
- 9 bench slots, matching the in-game bench
- Units stack into 2-star (3 copies) and 3-star (9 copies) automatically, matching in-game combine logic
- When the bench is full and a unit is purchased, the player must move a bench unit to the board or sell a unit before further purchases can land — identical to real game behavior

This constraint is intentional: practicing board management decisions under a rolling timer is a core skill the tool trains.

### 5.7 Gold Economy

Gold is displayed in the top bar. The following economy rules apply:

- Rerolling costs 2 gold (matching the game)
- Buying XP costs 4 gold and grants 4 XP (matching the game)
- Leveling XP thresholds match in-game values
- Purchasing a unit deducts its cost in gold

If gold reaches 0, the reroll and buy XP buttons are disabled (not hidden).

### 5.8 Post-Session Summary

After the timer expires or the player manually ends the session, a summary screen displays:

| Metric | Description |
|---|---|
| Time Elapsed | Total planning-phase time consumed |
| Units Purchased | Full list of all units bought, with timestamps |
| Missed Target Units | Target units that appeared in the shop but were not purchased |
| Rerolls Used | Total count of rerolls executed |
| Rolls Per Second | Average rerolls per second during the active rolling period |

The player can immediately start a new session with the same or updated configuration.

---

## 6. Non-Functional Requirements

### 6.1 Performance
- First contentful paint under 2 seconds on a standard broadband connection.
- Shop refresh (`D` key) must respond and render within 100ms — new units appear in shop slots instantly, matching the in-game behavior where units simply replace the previous shop without animation.
- Unit art should be lazy-loaded with a placeholder so the shop never blocks on image load.

### 6.2 Accuracy
- Shop odds must match the official Set 17 values within rounding precision.
- Pool depletion must correctly reduce per-unit odds (not just cost-tier odds) to simulate realistic late-roll sessions.

### 6.3 Browser Support
- Chrome 120+, Firefox 120+, Safari 17+, Edge 120+.
- No install, no login, no cookies required for core functionality.

---

## 7. Technical Architecture

### 7.1 Stack

The app is a single-page application with no backend dependency for v1. Recommended stack:

- **Framework:** React (functional components, hooks)
- **State:** Local React state + `useReducer` for economy/pool/board logic
- **Unit Data:** Hardcoded JSON derived from Set 17 unit list (name, cost, image URL, traits)
- **Images:** Riot CDN or Community Dragon for unit splash art, referenced by static URL pattern
- **Keyboard Events:** Document-level `keydown` listeners, unbound on unmount
- **Styling:** CSS Modules or Tailwind; no runtime CSS-in-JS for performance
- **Persistence:** `localStorage` for session config defaults only; no account system

### 7.2 Unit Data Schema

```json
{
  "id": "ezreal",
  "displayName": "Ezreal",
  "cost": 1,
  "traits": ["Timebreaker", "Sniper"],
  "imageUrl": "https://cdn.communitydragon.org/latest/champion/ezreal/splash-art/centered"
}
```

### 7.3 Pool State

The pool is a map of `unitId → remainingCopies`, initialized from the pool sizes in Section 5.3. Each purchase decrements the unit's count. The probability of a given slot drawing a specific unit is:

```
P(unit) = (remainingCopies[unit] / totalRemainingCopiesInCostTier) * P(costTier at level)
```

### 7.4 Shop Generation Algorithm

1. For each of 5 slots, sample a cost tier using the level's probability distribution.
2. Within the selected cost tier, sample a unit weighted by remaining pool count.
3. Mark sampled units as "reserved" for this roll (returned to pool on next reroll if not purchased).

---

## 8. UX and Design Direction

### 8.1 Aesthetic

The app should feel like a Challenger-tier training tool — not a fan site. The design language should be **dark, precise, and data-dense** — closer to a pro esports overlay than a colorful gaming wiki.

- **Color palette:** Near-black background (`#0B0B0F`), muted gold accent (`#C89B3C` — matching TFT's cost-5 color), cool gray text, cost-tier border colors matching in-game exactly
- **Typography:** A condensed technical font for UI labels; a slightly humanist font for unit names
- **Shop slots:** Rectangular, art-forward cards with the cost gem in the corner and a team planner icon overlay when applicable
- **Shop refresh:** No animation — units replace instantly in their slots on reroll, exactly as in the live game
- **Team planner panel:** Sidebar — compact, scannable, trait-synergy-forward

### 8.2 Layout

The layout follows the visual structure of the in-game TFT client: board and bench occupy the central area, the shop bar sits at the bottom, and the team planner panel is accessible as a persistent sidebar. The top bar holds economy and timer state.

```
┌──────────────────────────────────────────────────────────────────┐
│  LEVEL [8]   GOLD [47]   XP [64/100]   TIMER [0:23]             │
├────────────────────────────────────────┬─────────────────────────┤
│                                        │  TEAM PLANNER           │
│         HEX BOARD (4×7 grid)          │  Arbiter   ●●○○         │
│         [units placed via W]           │  Vanguard  ●●●○         │
│                                        │  Dark Star ●●○○         │
│                                        │  ─────────────────────  │
├────────────────────────────────────────│  Leona    [icon]        │
│         BENCH (9 slots)               │  Mordekaiser [icon]     │
│  [○][○][○][○][○][○][○][○][○]         │  LeBlanc  [icon]        │
│                                        │  Blitzcrank [icon]      │
├────────────────────────────────────────┴─────────────────────────┤
│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐                  │
│  │[art] │ │[art] │ │[art] │ │[art] │ │[art] │                  │
│  │      │ │      │ │ [★]  │ │      │ │      │  ← planner icon  │
│  │ Name │ │ Name │ │ Name │ │ Name │ │ Name │                  │
│  └──────┘ └──────┘ └──────┘ └──────┘ └──────┘                  │
│  [D] REROLL (2g)                           [F] BUY XP (4g)      │
└──────────────────────────────────────────────────────────────────┘
```

---

## 9. Rollout Plan

### Phase 1 — MVP (v1.0)
**Scope:** Core shop simulation, correct odds, keyboard controls, hex board + bench, team planner with trait synergies, post-session metrics.  
**Target:** 6 weeks from kickoff.  
**Success criteria:** A Challenger-level player can use the tool to complete 10 rolldown reps in a single sitting and report that shop behavior, board constraints, and pool depletion match the live game.

### Phase 2 — Pool Contestation Simulation (v1.5)
- Per-unit pool exclusion configuration: before a session, the player can specify how many copies of any given unit have been "taken" by other players, reducing that unit's available pool count accordingly. For example, setting Leona to "6 copies taken" simulates a heavily contested 1-cost in a full lobby. This directly trains the player's ability to adapt rolldown targets in real-time when their primary units are unlikely to appear.
- Configurable number of contested units (e.g., mark 0–7 units as contested with custom copy-removal counts)
- Visual indicator in the team planner panel showing expected remaining pool depth for each target unit given the configured contestation

### Phase 3 — Social and Analytics (v2.0)
- Account system with session history
- Leaderboard for rolls-per-second and missed target rate
- Shareable session summary cards (image export for Twitter/Discord posting)
- Coach mode: instructor configures the team planner, student completes the rolldown

---

## 10. Success Metrics

| Metric | Definition | Target (30 days post-launch) |
|---|---|---|
| Session Completion Rate | % of sessions that reach the post-session summary | >70% |
| Repeat Session Rate | % of users who complete 3+ sessions | >40% |
| Missed Target Rate (avg) | Avg missed target units per session | Baseline; track reduction over time |
| Rolls Per Second (avg) | Average roll speed across all sessions | Baseline; track improvement over time |
| Time-on-Site | Average session duration | >8 minutes |

---

## 11. Open Questions

1. **Image licensing:** Unit splash art from the Riot CDN is widely used by community tools under Riot's Legal Jibber Jabber / community guidelines. Legal review recommended before launch.
2. **Mobile scope:** Should a simplified click-only mobile version be added to Phase 2, given that some players review comps on mobile?
3. **Set versioning:** When Set 18 releases, what is the maintenance plan for updating unit rosters and odds? Consider a JSON-driven unit data file that can be swapped independently of application code.
4. **Audio:** Should the app include optional sound effects (shop refresh chime, purchase click) to strengthen the muscle-memory loop? Low effort, potentially high fidelity impact.

---

## Appendix A — Set 17 Unit Roster by Cost

### 1-Cost (14 units, 30 copies each)
Aatrox, Briar, Caitlyn, Cho'Gath, Ezreal, Leona, Lissandra, Nasus, Poppy, Rek'Sai, Talon, Teemo, Twisted Fate, Veigar

### 2-Cost (13 units, 25 copies each)
Akali, Bel'Veth, Gnar, Gragas, Gwen, Jax, Jinx, Meepsie, Milio, Mordekaiser, Pantheon, Pyke, Zoe

### 3-Cost (13 units, 18 copies each)
Aurora, Diana, Fizz, Illaoi, Kai'Sa, Lulu, Maokai, Miss Fortune, Ornn, Rhaast, Samira, Urgot, Viktor

### 4-Cost (14 units, 10 copies each)
Aurelion Sol, Corki, The Mighty Mech, Karma, Kindred, LeBlanc, Master Yi, Morgana, Nami, Nunu & Willump, Rammus, Riven, Tahm Kench, Xayah

### 5-Cost (9 units, 9 copies each)
Bard, Blitzcrank, Fiora, Graves, Jhin, Shen, Sona, Vex, Zed

---

## Appendix B — Keybind Reference Card

| Key | Action |
|---|---|
| `D` | Reroll (costs 2g) |
| `F` | Buy XP (costs 4g) |
| `W` | Move selected unit to board / bench |
| `E` | Sell selected unit |

---

*This document is a living spec. All requirements subject to revision based on playtesting feedback prior to v1.0 launch.*
