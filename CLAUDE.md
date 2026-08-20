# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:

- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:

- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:

- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:

- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:

1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 5. File Limits

**Codebase file limit: 500 lines per file.**

## 6. TCG Development Guidelines

**Technology stack: HTML, JavaScript, CSS only.**

### TCG Architecture

- Separate concerns strictly: HTML (structure), CSS (presentation), JavaScript (behavior)
- No inline event handlers
- No eval() or similar dynamic code execution
- Use semantic HTML5 elements
- CSS: Use BEM methodology for naming
- JavaScript: Use ES6+ modules, no global namespace pollution
- DEVELOPMENT FRAMEWORKS; phaser.js, tone.js, pizzicato.js, animation.css
- TCG is called ASUNDERED, a data-driven game made with html, js and css
- PLAYERS start with 33 VITALITY(health) and 2 MENTALITY(mana) with a maximum of 10
- PLAYERS use MENTALITY to cast cards of 5 types (UNITS, INVOCATIONS, RELICS, PACTS, REALMS)
- UNITS are attackers summoned to 1 of 4 UNIT ZONES, UNITS cannot attacked on same turn they are summoned (summoning sickness)
- INVOCATIONS are single use spells that go straight to VOID after use
- RELICS are equipped to friendly/enemy UNITS and are played to 1 of 4 RELIC SLOTS, behind UNIT ZONES
- PACTS provide powerful effects that require a recurring cost to upkeep its effects every turn during the CHARGE phase
- REALMS provide powerful benefits for its caster and/or negative effects for opponent and can only be one is play at any given time for both players, new REALMS overwrite ones in play
- PLAYERS WIN by reducing opponents VITALITY to zero or opponent decking out
- Game board is called a C.A.S.T BOARD
- C.A.S.T BOARD is a mirrored board with each player having LIBRARY/DECK, VOID/GRAVEYARD, 4 UNIT ZONES, 4 RELIC SLOTS, 1 PACT SLOT on each side of a C.A.S.T center bar
- C.A.S.T center bar has turn indicator, phase indicator with next phase ui button, battle log and a REALM card slot thats shared between both PLAYERS
- C.A.S.T represents the four phases in game, CHARGE, ACTIVATE, STRATEGY, TERMINATION
- CHARGE is the draw phase, MENTALITY generation (+1 per turn)
- ACTIVATE is when PACT upkeep cost is paid, players can spend MENTALITY to cast UNITS, INVOCATIONS, PACTS, REALMS and equip RELICS to UNITS
- STRATEGY is when you can attack the opponents UNITS, if they have no units then you can attack VITALITY directly
- TERMINATION is to indicate turn is over
- LIBRARY/DECK has 33 cards
- card stats are ATk (MAX 10), HEALTH (max 10), MENTALITY cost (max 10)
- PLAYERS coin flip to determine who goes first, first PLAYER skips card draw and cannot attack
- PLAYERS start with 5 cards in hand (max 7)
- ASUNDERED is defined by its choice of 8 FACTIONS to choose from, each have a unique style of playstyle/theme/lore
- C.A.S.T BOARD needs to act as the GOD/ENGINE/REFEREE with each FACTION acting as a PLUGIN that respects the game ENGINE
- FACTIONS as PLUGINS work as self contained folders, details listed below

### FACTION plugins

Every faction **must** have these files:

- `manifest.json` - Metadata and configuration
- `data.json` - Card database (array of card objects)
- `plugin.js` - Entry point with required exports
- `ability.js` - Faction ability class
- `handlers.js` - Card effect implementations
- `*-ai.js` - AI implementation (flexible naming)
- `theme.css` - Visual theme variables
- `music.js` - Audio pattern definitions
- `_back.png` - Card back artwork

### Plugin Interface Requirements

Each faction's `plugin.js` must export:

```javascript
export default {
  getManifest: () => manifest,
  getCards: () => cards,
  getHandlers: () => handlers,
  getKeywords: () => ({}), // Optional
  getAbilityClass: () => AbilityClass,
  getAIClass: () => AIClass,
};
```

### Manifest Structure

```json
{
  "name": "faction_internal_name",
  "displayName": "Human Readable Name",
  "version": "1.0.0",
  "description": "Detailed faction description",
  "mechanics": ["mechanic1", "mechanic2"],
  "essenceTrigger": "triggerName",
  "cardBack": "image.png",
  "atmosphereClass": "css-class",
  "difficulty": "easy|medium|hard",
  "unlockLevel": 1
}
```

### Card Data Structure

Each card in `data.json` must have:

```json
{
  "id": "unique_id",
  "name": "Card Name",
  "type": "Unit|Invocation|Relic|Pact|Realm",
  "faction": "Faction Name",
  "cost": 3,
  "attack": 2,
  "health": 3,
  "keywords": ["Haste", "Defender"],
  "rules": "Card effect description",
  "arrivalEffects": [{"type": "draw", "value": 1}],
  "essenceAbilities": [{"cost": 3, "effect": {...}}]
}
```

### Development Process

1. Start with HTML structure
2. Add CSS styling
3. Implement JavaScript behavior
4. Test across modern browsers
5. Validate accessibility (WCAG 2.1 AA)

### Code Quality

- Keep functions under 50 lines
- Use descriptive variable names
- Document complex logic
- No TODO/fixme comments without assigned owners
- Regular code reviews required

## 7. Verification

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, clarifying questions come before implementation rather than after mistakes, files stay under 500 lines, and all code uses only HTML/JS/CSS.

<!-- CHECKPOINT id="ckpt_mo9yylqy_124b8z" time="2026-04-22T11:28:23.194Z" note="auto" fixes=0 questions=0 highlights=0 sections="" -->
