# Visual Priority Plan

## Top 5 visual problems
1. **No lighting, shadow, or volume anywhere** — every entity is a flat-filled shape with no depth cue, so the whole scene reads as "floating on a flat plane."
2. **No death/hit/build feedback beyond a color flash** — the most frequent player actions (killing enemies, spending gold) have no visual payoff.
3. **Flat, featureless background** — a static grid with no terrain, town layout, or sense of "a village under siege."
4. **Everything is a circle** — player, enemies, and gold are all the same silhouette family differentiated only by color/size, so nothing has a distinct visual identity.
5. **UI/world style mismatch** — the HUD looks like a modern app; the game world looks like a debug prototype. The two don't feel like the same product.

## Top 5 quick wins
1. Drop shadows under every entity (grounds everything instantly).
2. Enemy death particle burst (fixes the most-repeated "cheap" moment in the game).
3. Radial gradients instead of flat fills on all circles (adds volume for free).
4. Full-screen vignette as the last draw step (makes the frame feel composed).
5. Muzzle flash + bullet glow (makes combat feel like combat, not dot-collision).

*(Full list of 12 with effort estimates and code-level detail: see `quick_wins.md`.)*

## Top 5 high-impact upgrades
1. A real illustrated/textured background and town layout (single biggest lever on perceived quality).
2. Sprite-based art for player/enemies/towers instead of primitive shapes.
3. A unified particle/effects system feeding every game event (hit, death, pickup, build, upgrade).
4. Screen shake + hit-stop on impactful moments (core damage, tank kills, upgrades).
5. Custom display font + small custom icon set for the HUD, to match the world's new art direction once it exists.

*(Full list of 10 with rationale: see `high_impact_upgrades.md`.)*

## What to change first
Do the **quick wins pass first, in full**, before touching any high-impact item. Reasons:
- It's roughly half a day of work against a build that currently looks like a prototype — the return on investment is very high.
- Several quick wins (particles, glow technique, shadow logic) are literally the building blocks the high-impact items reuse, so nothing here is wasted effort even after the bigger upgrades land.
- It requires no new art assets and no decisions about final art direction, so it can start immediately without blocking on style decisions.

Recommended order within the quick wins (also listed in `quick_wins.md`):
**shadows → death burst → gradients → vignette → muzzle flash → bullet glow → forge glow → zone tint → upgrade "pop" → HP/gold bar animation → idle bob → icon replacement.**

## What can wait until later
- **Sprite art and a real background** (#1–#2 in high-impact) — these are the right long-term direction, but they require either commissioning/generating actual art assets or committing to a specific style direction (pixel art vs. flat vector vs. painterly), which is a bigger decision than anything else in this audit. Don't block shipping/testing the MVP loop on this.
- **Dynamic lighting pass and color-grading pass** — these are "final polish" steps that only pay off once the base art (background + sprites) exists to be lit/graded. Doing them earlier would mean redoing the work later.
- **Menu/transition juice (fades, animated start screen)** — nice, but purely cosmetic on the least-frequently-seen screens in the game. Lowest priority of everything listed in this audit.
- **Silhouette redesign per enemy/tower tier** (#7–#8 in high-impact) — worth doing, but naturally bundled with the sprite-art pass rather than as a separate effort.

## One-paragraph takeaway
The gameplay loop and HUD are already in reasonable shape — the UI in particular is close to shippable as-is. The gap between "prototype" and "premium" is almost entirely in the game world: no shadows, no lighting, no particles, no background detail, and no shape variety beyond color. None of that requires a rewrite or new art tools — the quick-wins pass alone (roughly half a day, no new assets) will visibly close most of the gap, and it directly sets up the two upgrades (real background art, sprite-based entities) that would take the project the rest of the way to feeling premium.
