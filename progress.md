Original prompt: スマホだったら、それに応じた画面になるようにしましょう（index）
その場合、操作はどこでもいい（メーター以外の場所）を上下左右でドラッグからの固定などでその方向に移動できるように

Progress:
- Added a work log for mobile controls and responsive layout changes.
- Switched mobile layout detection to coarse-pointer / touch-aware logic so desktop browser windows are not treated as phones.
- Mobile swipe movement works in touch emulation; patched pointer capture to ignore synthetic-event errors in tests.
- Reserved the meter area on touch so it does not turn into movement input when empty.
- Verified desktop render plus mobile right-swipe and up-swipe behavior with Playwright touch emulation.
- Updated touch dragging so a sharp move in the opposite direction switches movement immediately.
- Added a stone-depth hollow chamber and a 2x2 cash machine made from `cash machine1-4.png`, plus suction-based dig power boosts.
- Increased dig power growth and tied gravity to dig power.
- Added floating green `+0.03` style delivery popups near the player.
- Capped mobile canvas DPR to avoid huge render surfaces on real phones.
- Mobile freeze was still present in touch emulation, so the render path was simplified again: no large offscreen world layers, mobile uses color-filled tiles instead of image-heavy tile rendering, and mobile DPR is now clamped to 1. This brought emulated mobile RAF back up to a usable range.
- Restored image-based tile/background rendering on mobile after confirming emulated mobile stays stable with DPR 1; the player and machine sprites were already loading correctly, so the earlier issue was the simplified tile fallback rather than asset loading itself.
- Restored the sky background image (`background.png`) after noticing the sky was being replaced by a flat fill.
- Added stronger mobile gesture suppression with `user-select: none`, `-webkit-touch-callout: none`, and explicit `touchstart`/`touchmove` `preventDefault()` handlers so drags do not become browser selection or navigation gestures.
- Added `coal` tiles in stone layers, with their own inventory slot, image rendering, and delivery reward (`+0.03`). Dirt now breaks faster and delivers `+0.01`, stone delivers `+0.02`, and climb collisions no longer trigger rapid wall-breaking while the player is in climbing mode.
- Added `biotite` and `agate` as rarer stone-layer resources, each using its own inventory slot and asset. They share coal-like hardness and award `+0.04` on delivery, while remaining visible in mobile/desktop rendering and `render_game_to_text`.
- Reduced ore spawn density and switched stone-layer ore placement to sparse region-based seeding so ores no longer appear in dense diagonal streaks.
- Consolidated the five separate inventory gauges into one stacked gauge with fixed layers for dirt, stone, coal, biotite, and agate. Each layer is still individually selectable for drag-out, but the UI now reads as a single meter.
- Added the new image assets to the repository commit set, including `assets/coal.png`, `assets/biotite.png`, `assets/agate.png`, and `assets/quartz.png`.
- Reworked the inventory gauge fill to use solid colored square blocks instead of resource images. Added `quartz` as a sixth stored resource layer so the gauge can show the requested `C7E1EA` color alongside the other materials.
- Clarified the gauge behavior: it is now a single stack that fills from the bottom upward regardless of resource type, with one combined capacity shared by all block types.
- Changed ore generation from isolated single tiles to 1-5 block clusters so ores are easier to discover in the large stone layer.
- Increased ore frequency further by allowing up to two clustered ore deposits per stone-region roll, since the stone layer is large enough that sparse single clusters were still hard to find.
- Relaxed the horizontal wall bounce lock so it only suppresses pushing further into a wall, not all lateral movement. Also kept the climb animation active while descending during climbing movement.
