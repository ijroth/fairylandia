# Fairylandia Development Instructions

## Project Structure
- Single-file HTML5 canvas game: `index.html`
- 480x270 logical resolution, 2x render scale

## Development Guidelines
- When making changes, maintain the existing code style and patterns
- All game logic is in one `<script>` tag inside index.html
- Keep the pastel color theme throughout
- Test changes by opening index.html in a browser
- Parallelize agent work where possible for speed
- Use faster/cheaper models for subagent research tasks
- Minimize need for user intervention - deliver working results
- Commit working checkpoints frequently

## Key Design Rules
- Black fairies are neutralized by SPINNING only, NOT by spells
- Spells destroy obstacles (and bosses in boss fights)
- Exception: ~15% of obstacles are secretly `unbreakable: true` — spells pass through them with no effect. They look IDENTICAL to normal obstacles of the same type (no visual tell) so the player can't distinguish them ahead of time
- Levels regenerate randomly each play
- Everything should maintain the pastel aesthetic
- Support both desktop and mobile (isMobile flag)

## Game Architecture
- Game states: HOME, LEVEL_SELECT, CLOSET, HOW_TO_PLAY, PLAYING, BOSS, PAUSED, LEVEL_COMPLETE, VICTORY, CARNIVAL, CARNIVAL_RIDE, CARNIVAL_ICECREAM, CARNIVAL_BOTTLES
- Key objects: SaveData, Input, Game
- Cosmetics: 6 categories (hair, wings, outfits, eyes, shoes, accessories)
- Save data in localStorage under 'fairylandia_save'
- Levels 1-9: runner/obstacle course, Level 10: evil fairy boss, Level 11: dual shadow fairy boss
- Carnival: separate fun area with rides, ice cream, and bottle game

## Lives / Restart System
- `Game.lives` / `Game.maxLives` track hits remaining before a level restarts; levels 1-9 and level 10 give 3 lives, level 11 gives 2
- Levels 1-9: each obstacle hit costs a life. Level 10/11: each red beam hit that lands costs a life
- On the last life lost, `Game.restartTimer` is set (brief pause for hit feedback), then `handleRestart()` calls `startLevel(currentLevel)` again — this regenerates levels 1-9 randomly and fully resets boss HP/state
- Health bar (hearts, `drawHeart`) renders in `renderHUD`, shared by PLAYING and BOSS

## Boss Beam Attacks (Level 10/11)
- Each boss fairy has its own `beamCooldown`; when it elapses, it fires a beam targeting the player's *current* lane, pushed into `Game.beams`
- Beams travel for a fixed `duration` of 4 seconds (`beam.elapsed` counts up) — dodge by switching lanes before it resolves; hit/miss is resolved by comparing the player's lane at `elapsed >= duration`, not by tracking position along the way
- Rendered in `renderBoss` as a growing red line from the boss to the player

## Fairy Wands & Megagems
- Each level 1-9 spawns exactly one `level.wand` (fly through to collect, like rings/diamonds)
- `SaveData.addWand()` increments `wandsCollected`; every 9 collected converts to 1 `megagems` (non-reloadable, persists across sessions) and the counter resets — replaying levels 1-9 keeps stacking megagems
- `SaveData.useMegagem()` spends one gem. Usable only during BOSS state: `KeyM` on desktop, or tapping the `MEGAGEM_RECT` HUD icon on mobile (gated by `isMobile`) — instantly defeats the evil fairy on level 10, or one shadow fairy on level 11
- Megagem count is shown in the gameplay HUD (`renderHUD`) and under the diamond counter on HOME/CLOSET/CARNIVAL screens (`drawMegagem`)
