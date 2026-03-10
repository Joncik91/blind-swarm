# Blind Swarm

A Claude Code plugin that runs blind swarm idea generation.

## What it does

Independent AI agents, each unaware of each other, collide randomly in a tournament bracket to produce emergent ideas. No orchestrator controls the creative direction.

## Commands

- `/swarm <seed text>` - Run a blind swarm with your seed text

## Architecture

- 8 agents spawn with random fragments + random perspective lenses
- Tournament bracket: 8 → 4 → 2 → 1 through random collision
- Final filter scores on novelty, coherence, generativity
- The randomness is the feature, not a bug

## Key principle

No single component ever holds the complete picture of the creative trajectory.
