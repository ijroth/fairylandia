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
