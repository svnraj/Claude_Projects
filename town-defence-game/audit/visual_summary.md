# Visual Audit — Summary

**Project:** The Last Village
**Scope:** Visual quality, presentation, polish only (no gameplay/logic feedback)
**Reviewed:** `index.html` (current MVP build) — pure canvas 2D + primitive shapes, no sprites/images, no particle system, no custom font.

## Overall impression

Right now it reads as a **functional prototype, not a game** — the kind of thing a programmer built to test mechanics before an artist touched it. That's expected for an MVP, but it's worth naming plainly: every visual element is a flat-colored circle, square, or Unicode symbol (⌂ for the core, ⚒ for the forge). There is no sprite art, no texture, no shadow, no gradient, no particle, and no custom typography anywhere in the build. The HUD is a clean, modern *web app* panel — noticeably more polished than the game world it's sitting on top of, which is an odd mismatch: the chrome looks like 2024 SaaS, the world looks like a 1990s prototype.

**What already looks good:**
- The HUD panels (top bar, HP bar, gold counter, weapon stats) — dark glass panels, rounded corners, clean hierarchy, legible type. This is the strongest visual asset in the project.
- Color coding is functionally sound: blue = player/tower, orange = fast enemy, purple = tank enemy, gold = currency. Nothing is ambiguous.
- The overlay screens (start/win/lose) have decent typographic hierarchy and a believable "game menu" feel.

**What makes it feel cheap:**
- Every character and structure is a solid-color circle or square. There is no silhouette variation — player, tower, and enemies are all "circle with a slightly different color," so nothing has a distinct read at a glance beyond color.
- Zero lighting/shadow model. Nothing casts a shadow, nothing has volume — everything looks like it's floating flat against the background, including the town core, which should feel like the most important object on screen.
- The background is a static, uniform dark-blue grid. It doesn't communicate "town" or "wilderness" at all — it could be the background for any genre of game.
- No hit feedback beyond a white color-flash and a hp-bar tick. No death effect (enemies just vanish), no muzzle flash, no impact spark, no screen shake.
- Bullets are single-color dots with no trail, glow, or motion blur — they read as UI elements more than projectiles.

**What to improve first:** the two things doing the most damage to perceived quality are (1) the complete absence of light/shadow/depth anywhere in the world, and (2) enemies and structures having no distinguishing shape beyond a circle and a fill color. Both are fixable without new art assets — see `quick_wins.md`.

## Section scorecard (informal, 1–5)

| Area | Score | Note |
|---|---|---|
| Overall impression | 2/5 | Reads as programmer art / prototype |
| Art style consistency | 3/5 | Consistent, but the style itself is "placeholder" |
| Color & lighting | 2/5 | Good color coding, zero lighting/depth |
| UI quality | 4/5 | Genuinely close to shippable already |
| Animation & feedback | 1.5/5 | Present but minimal; nothing feels impactful |
| Background/environment | 1.5/5 | Flat grid, no sense of place |

See `visual_problems.md`, `quick_wins.md`, `high_impact_upgrades.md`, and `visual_priority_plan.md` for the detailed breakdown and an ordered plan.
