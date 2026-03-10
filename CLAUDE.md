# Blind Swarm

A Claude Code plugin for emergent idea generation through independent AI agent collisions.

## What it does

Independent AI agents, each unaware of each other, collide randomly in a tournament bracket to produce emergent ideas. No orchestrator controls the creative direction. The randomness IS the creative engine.

## Commands

- `/swarm <seed>` — Standard run. 8 agents, 3 rounds. (15 agent calls)
- `/swarm-quick <seed>` — Fast run. 4 agents, 2 rounds. (7 agent calls)
- `/swarm-deep <seed>` — Full chaos. 16 agents, 4 rounds, wild fragment injection, 4 lens pools. (31 agent calls)

## How it works

1. Seed text gets fragmented mechanically (no AI)
2. Each fragment pairs with a random perspective lens
3. Agents expand fragments independently
4. Outputs randomly collide in tournament bracket rounds
5. Each collision round uses fresh lenses
6. Filter scores final outputs on novelty, coherence, generativity

## Key principle

No single component ever holds the complete picture of the creative trajectory. The system is creative, not any individual agent.
