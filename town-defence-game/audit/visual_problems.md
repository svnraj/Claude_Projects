# Visual Problems — Detailed Findings

Organized by the audit categories requested. Each finding notes *where* it shows up in the current build and *why* it reads as low-end.

---

## 1. Overall visual impression

- **Uniform "circle-and-square" silhouette language.** Player, both enemy types, and all four tower levels are geometric primitives (circle, circle, circle, rounded square). At a glance, nothing but color tells you what you're looking at — there's no shape storytelling (e.g., a tank enemy that visually looks heavier/slower, a tower that looks more imposing at higher levels).
- **No sense of scale or weight.** Every entity has a flat fill and a thin stroke outline. A level-3 tower looks like a level-1 tower with a different number in the middle — upgrading doesn't feel visually rewarding.
- **The game currently looks "unstyled" rather than "stylized."** There's a difference between a deliberately minimal/geometric art style (which can look premium — e.g. Vampire Survivors' early builds, or geometric indie titles) and this, which reads as unstyled because there's no deliberate design language tying the shapes together (no consistent stroke weight logic, no consistent corner radius logic, no consistent shadow/light direction).

## 2. Art style

- **No defined style direction exists yet.** The build is functionally "whatever `ctx.arc()` and `ctx.fillRect()` produce," not an intentional pixel-art, vector-flat, or hand-drawn style.
- **Nothing is mismatched *per se*** (there's no art to clash), but that's itself the problem — there's no visual identity to protect or extend.
- **Iconography is placeholder-grade.** The core uses a raw Unicode "⌂" house glyph and the forge uses "⚒" — these render using the browser's default emoji/symbol font, so their weight, style, and even appearance will vary between Windows/Mac/Linux and between browsers. This is a real risk: what looks fine on your dev machine may render as a different-looking (or missing) glyph elsewhere.

## 3. Color and lighting

- **Palette is functional but flat.** Background `#1b2430`, core `#5c4a2e`/gold outline, forge `#6e3b23`/orange outline, towers `#2d4257`/blue outline — these are reasonable hue choices but every fill is a single flat color with no gradient, so nothing has volume.
- **Zero shadow-casting anywhere.** Not even a simple drop-shadow ellipse under the player/enemies to ground them to the floor. Right now everything appears to float.
- **No glow on anything that should glow** — bullets, the forge (it's literally a furnace/fire object and has no light emission), gold pickups, or the town core when critically low on HP.
- **Contrast is adequate but not tuned.** The grid lines (`rgba(255,255,255,0.03)`) are so faint they barely register, which is fine, but it also means the ground plane has almost no visual texture at all — it's close to a solid color.
- **No atmospheric variation.** No vignette, no radial darkening toward the edges, no color temperature shift between the "safe" town zone and the "danger" wilderness zone beyond the ring.

## 4. UI quality

- This is the strongest area, but still has gaps:
  - **System font only** (`Segoe UI, Roboto, Arial`) — functional, but a game with a name like "The Last Village" would benefit from even one distinctive display font for the title/timer to feel less like a web form.
  - **Icon inconsistency:** gold icon is a plain filled circle (`●`), weapon stats use plain bold text, core uses a house glyph — there's no unified icon set.
  - **Floating combat text uses default sans-serif bold** with a flat drop-shadow — functional but generic; no punch/scale animation on spawn.
  - **The hover tooltip bubble is plain and static** — appears/disappears with no transition, which feels abrupt next to the otherwise smooth HUD.

## 5. Animation and visual feedback

- **Hit feedback is a single white flash + HP bar update.** No knockback, no squash/stretch, no particle burst.
- **Enemy death has zero visual event.** The enemy is simply removed from the array and a gold coin appears — there's no death animation, no shatter/poof, no last-frame reaction. This is one of the most noticeable "cheap" moments in actual play: enemies just blink out of existence.
- **No muzzle flash or shot-origin effect** when the player or a tower fires.
- **No build/upgrade "pop."** Building a tower or upgrading the forge just swaps a color/label instantly — no scale-in, no particle burst, no light pulse, despite these being the core "reward" moments of the loop.
- **No core damage feedback beyond a color flash on the icon itself and a floating "-4" text.** A repeatedly-attacked core doesn't feel increasingly dangerous (no shake, no smoke, no cracking visual as HP drops).
- **No idle/ambient animation anywhere** — the player, towers, and forge are all perfectly static when not acting, which reads as "unfinished" even when nothing needs to be happening yet.

## 6. Backgrounds and environment

- **The world is a single flat-colored rectangle with a faint grid.** There is no ground texture, no terrain variation, no town silhouette (buildings, walls, paths) beyond the core/forge/towers themselves.
- **The "town boundary" is just a thin translucent circle outline** — it doesn't read as a wall, fence, or meaningful boundary; it looks like a debug/range-indicator line (and is easily confused with the tower range rings, which use the same visual language).
- **No distinction between "inside the walls" and "the wilderness enemies come from."** Nothing about the outer 2/3 of the map communicates danger, distance, or terrain — it's the same flat color and grid as the safe zone.
- **No parallax, depth layers, or environmental storytelling** (scattered rubble, campfires, banners, a horizon/skybox) that would sell the "last village under siege" premise the name promises.

---

## Root causes worth naming

Two structural choices are behind most of the above:
1. **Everything is drawn with raw Canvas primitives at render time**, with no sprite/texture pipeline. This is fine for an MVP, but it caps visual quality hard — flat fills can only ever look so good.
2. **There is one draw pass per frame with no layering system for effects** (particles, shadows, glows) — the draw functions (`drawCore`, `drawEnemies`, etc.) each fully own their own look, so there's no shared "juice" layer (screen shake, particle emitter, flash overlay) that every event can hook into.

Neither requires a rewrite to fix — see `quick_wins.md` and `high_impact_upgrades.md`.
