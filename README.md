# TFT Rolldown Trainer

**A browser-based practice tool for the shop in Teamfight Tactics. It uses the game's real odds, so players can drill rolldowns away from a live match.**

### [▶ Live Demo](https://eddieh80.github.io/tft-rolldown-app/)

![HTML](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![CSS](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
![Web Audio API](https://img.shields.io/badge/Web%20Audio%20API-4A4A55)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-2088FF?logo=githubactions&logoColor=white)

---

## The Problem

In TFT, a "rolldown" is a burst of spending where a player rerolls the shop over and over to find specific units, all inside a 30-second planning phase. Even top players lose value here through misclicks, missed units, and poor bench management. The usual ways to practice are a full live game, which adds a lot of mental load, or static odds tables, which aren't interactive. Before this project there was no dedicated simulator with accurate odds and the game's own keybinds.

## The Solution

This app recreates the TFT shop, board, and bench at full fidelity so that what players practice carries over to the real game:

- **Accurate shop simulation.** The app uses the real per-level shop odds and a shared, finite unit pool (30/25/18/10/9 copies by cost). Copies leave the pool when they appear in the shop or are bought, and return when the shop rerolls or a unit is sold.
- **In-game keybinds and controls.** `D` rerolls, `F` buys XP, `E` sells the unit under the cursor, and `W` moves a unit between board and bench. Units can also be dragged and dropped, including onto the shop to sell them.
- **Faithful game rules.** Three copies auto-combine into a 2★ or 3★ unit, and the merge happens on the board if a copy is there. A purchase is allowed on a full bench only when it would complete a combine. Units drop out of the shop once they reach 3★. Board size is capped by player level, and sell values and XP carry-over work as they do in-game.
- **Team planner with trait synergies.** Players pick target units before or during a session and can filter them by trait. Target units are highlighted in the shop, and live trait breakpoints show bronze, silver, and gold tiers.
- **Performance analytics.** At the end of each session the app reports rolls per second, total rerolls, units bought with timestamps, gold left, and every target unit that appeared in the shop but wasn't bought.
- **Configurable drills.** Players choose a starting level (1–10), starting gold or infinite gold, and a 30s, 60s, or unlimited timer.

## Technical Highlights

- **No dependencies at all.** The whole app is a single ~2,500-line HTML file in vanilla JavaScript and CSS: no framework, no build step, no backend. It runs anywhere a browser does.
- **Weighted random sampling over shared state.** Each shop slot first rolls a cost tier from the level's odds. It then picks a unit from that tier, weighted by how many copies are left in the pool, and skips units the player already has at 3★. This matches how the real game's shop works.
- **Cascading combine logic.** Merges are resolved in a loop until nothing else can combine, so a 3×1★ → 2★ merge that produces a third 2★ also triggers the 3★ merge. Board copies are kept over bench copies.
- **Procedural sound design.** All sound effects are generated live with the Web Audio API (oscillators, filtered noise bursts, detuned shimmer), so the app ships no audio files.
- **Responsive hex-grid rendering.** A `ResizeObserver` recalculates the hex board's layout whenever its container changes size.
- **CI/CD.** A GitHub Actions workflow deploys to GitHub Pages on every push to `main`.

## Product Process

I ran this project like a real product cycle rather than a weekend hack:

- **Written PRDs.** The requirements doc went through five versions (v1 → v1.4) and covers goals, non-goals, user stories, and acceptance criteria. See [`prd/`](prd/).
- **Formal readiness audit.** The v26 build was audited against PRD v1.4 and passed all six product goals ([audit](prd/TFT_Rolldown_Trainer_Audit_v26_vs_PRD_v1_4.md)). The audit found that two "bugs" were actually errors in the spec, not the code, so the PRD was corrected to match the implementation.
- **26 build iterations.** Each version's changes were driven by gameplay testing and recorded in [`versions.md`](versions.md).
- **Live content migration.** The app was moved from Set 17 to Set 18 (Enchanted Wilds), covering 65 units, their traits and breakpoints, and their art.

## Getting Started

No installation needed. Either:

1. Open the **[live demo](https://eddieh80.github.io/tft-rolldown-app/)**, or
2. Clone the repo and open `tft-rolldown-trainer.html` in any modern browser:

   ```bash
   git clone https://github.com/EddieH80/tft-rolldown-app.git
   open tft-rolldown-app/tft-rolldown-trainer.html
   ```

### Controls

| Key / Action | Effect |
|---|---|
| `D` | Reroll shop (2g) |
| `F` | Buy 4 XP (4g) |
| `E` (hover) | Sell unit under the cursor |
| `W` (hover) | Move a unit between bench and board |
| Click / drag shop slot | Buy unit |
| Drag unit to shop | Sell unit |

## Project Structure

```
tft-rolldown-trainer.html   # The entire application (UI, game logic, data, audio)
prd/                        # Product requirements docs (v1 → v1.4) and readiness audit
versions.md                 # Iteration-by-iteration changelog (V1 → V26)
.github/workflows/          # GitHub Pages deploy pipeline
```

## Roadmap

- Lazy-load champion art to speed up the first page load
- Cross-browser QA (Safari, Firefox)
- Session history that persists between visits, for tracking improvement over time

---

*Unit art and trait icons are sourced from CommunityDragon and tactics.tools. TFT Rolldown Trainer isn't endorsed by Riot Games and doesn't reflect the views or opinions of Riot Games or anyone officially involved in producing or managing Riot Games properties. Riot Games and all associated properties are trademarks or registered trademarks of Riot Games, Inc.*
