# `@objectifthunes/whiteboard` — Claude skill

A [Claude skill](https://docs.claude.com/en/docs/agents-and-tools/claude-code/sub-agents) that teaches Claude how to wire up, design with, and extend the [`@objectifthunes/whiteboard`](https://www.npmjs.com/package/@objectifthunes/whiteboard) component library — a pan/zoom whiteboard canvas with floating panels, minimap, snap-to-grid, and 40+ themed UI primitives for React.

## What it contains

- **`SKILL.md`** — the skill itself. Trigger description, mental model, five-step wire-up, export catalogue map, and the ten non-obvious caveats.
- **`references/EXPORTS.md`** — full copy-pasteable import block + prop signatures for every export + type exports + subpath exports.
- **`references/TOKENS.md`** — full `--wb-*` design-token table (typography, surfaces, borders, text, actions, radii, gaps, effects) plus theming recipes.

## Install locally

Clone this repo into your skills directory:

```bash
git clone https://github.com/objectifthunes/whiteboard-claude-skill ~/.claude/skills/whiteboard-ui
```

Claude Code auto-discovers the skill via its `name:` frontmatter (`whiteboard-ui`). Trigger with `/whiteboard-ui` or by mentioning `@objectifthunes/whiteboard`.

## Live references

- Package: https://www.npmjs.com/package/@objectifthunes/whiteboard
- Source: https://github.com/objectifthunes/whiteboard
- Demo: https://objectifthunes.github.io/whiteboard-demo/
- Demo source: https://github.com/objectifthunes/whiteboard-demo

## Updating the skill

The skill is point-in-time for a given lib version. When the library publishes a new minor:

1. Re-read the demo + `node_modules/@objectifthunes/whiteboard/dist/index.d.ts`.
2. Update `SKILL.md` (current version + new caveats).
3. Update `references/EXPORTS.md` (new/removed exports, prop signature changes).
4. Update `references/TOKENS.md` (new tokens, recoloured defaults).
5. Commit + push.
