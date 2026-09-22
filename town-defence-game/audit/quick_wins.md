# Quick Wins

Everything here is achievable **inside the existing Canvas 2D drawing code** — no new art assets, no engine change, no external libraries. Each is a localized edit to one `draw*()` function. Rough effort is noted assuming familiarity with the current codebase.

## 1. Add drop shadows under every entity (15 min)
Before drawing the player/enemy/tower circle, draw a soft dark ellipse (`ctx.fillStyle = "rgba(0,0,0,0.35)"`, squashed vertically) offset slightly below it. This single change does more to make things feel "placed in the world" than almost anything else on this list — it's the fastest fix for the "everything is floating" problem.

## 2. Switch flat fills to radial gradients (20 min)
Anywhere you currently do `ctx.fillStyle = "#58a6ff"` for a circle, swap it for a `ctx.createRadialGradient()` (lighter highlight near the top-left, darker toward the edge). This alone gives every circle a sense of volume/sphere instead of a flat disc — applies to player, both enemy types, gold pickups, towers.

## 3. Give the forge a glow (10 min)
Since it's literally a furnace, draw a soft orange radial glow behind it (a large low-alpha circle in `#f0883e`) and consider a subtle pulsing alpha over time (`sin(elapsed * 2) * 0.05 + 0.15`). Reinforces "hot/active" and makes it feel like a lit object rather than a painted circle.

## 4. Add a muzzle flash on every shot (15 min)
When `fireBullet()` is called, spawn a tiny short-lived flash particle (a small bright circle, alpha fading over ~0.08s) at the origin point. Reuse the existing `floatTexts`-style array pattern — a `particles` array with `{x,y,life,type}` is enough.

## 5. Add an enemy death burst (20 min)
Right now enemies just vanish. On death, spawn 4–6 small colored particles that fly outward and fade (reuse the enemy's own color). This is the single highest-leverage "juice" fix relative to effort — death is the most frequent event in the game and currently has zero payoff.

## 6. Bullet trails / glow (10 min)
Give bullets a short trail by drawing 2–3 fading circles behind their current position (store last 2 positions per bullet), or simply add `ctx.shadowBlur = 8; ctx.shadowColor = <bullet color>` before drawing them. Makes projectiles read as fast-moving light instead of static dots.

## 7. Screen-space vignette (10 min)
One `ctx.createRadialGradient` drawn as the very last step of `draw()`, transparent in the middle and `rgba(0,0,0,0.35)` at the corners. Instantly makes the scene look "composed" instead of "rendered," and is a one-line addition with zero risk to gameplay.

## 8. Replace the Unicode glyphs with drawn icons (20–30 min)
The ⌂ (core) and ⚒ (forge) glyphs are font-dependent and inconsistent across systems. Replace them with a few lines of `ctx.beginPath()`/`ctx.lineTo()` drawing a simple house roof-triangle for the core and a simple hammer shape for the forge. More consistent, more on-brand, and removes a cross-platform rendering risk.

## 9. Tint the ground differently inside vs. outside the town ring (10 min)
Draw the area inside the 230px town-boundary circle with a very slightly warmer/lighter fill than the wilderness beyond it (clip to the circle, or draw a large soft radial gradient centered on the core). Immediately gives the map a sense of "safe zone vs. frontier" for almost no cost.

## 10. Animate the HP bar fill and gold counter (10 min)
Currently these snap instantly to new values. Lerp `coreHpBarFill`'s width over ~0.3s (CSS `transition` is already partially there for width — extend it to color too) and briefly scale/flash the gold number when it increases. Small touch, but repeated constantly, so it compounds.

## 11. Add a subtle idle bob to the player and enemies (10 min)
Offset each entity's vertical draw position by a tiny `sin(elapsed * speed + offset)` amount. Kills the "everything is a frozen paper cutout" feeling with almost no code.

## 12. Tighten the tower/core/forge upgrade "pop" (15 min)
On any successful build/upgrade, briefly scale the object up 1.15x and back down over ~0.2s (a simple `flashScale` timer per object, same pattern as the existing `flash` timers). Makes spending gold feel rewarding instead of just swapping a label.

---

**Suggested order if doing these in one pass:** #1 (shadows) → #5 (death burst) → #2 (gradients) → #7 (vignette) → #4 (muzzle flash) → #6 (bullet glow) → #3 (forge glow) → #9 (zone tint) → #12 (upgrade pop) → #10 (bar/gold animation) → #11 (idle bob) → #8 (icon replacement).

Total estimated time for all 12: roughly half a day of focused work, and it would visibly close most of the gap between "prototype" and "polished indie game" without touching any gameplay code.
