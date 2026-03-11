# Blind Swarm

A Claude Code plugin for emergent idea generation through independent AI agent collisions.

## What it does

Independent AI agents, each unaware of each other, collide randomly in a tournament bracket to produce emergent ideas. No orchestrator controls the creative direction. The randomness IS the creative engine.

## Commands

- `/swarm <seed>` — Standard run. 8 agents, 3 rounds. (15 agent calls)
- `/swarm-quick <seed>` — Fast run. 4 agents, 2 rounds. (7 agent calls)
- `/swarm-deep <seed>` — Full chaos. 16 agents, 4 rounds, wild fragment injection, 4 lens pools. (31 agent calls)
- `/swarm-acre <seed>` — ACRE-8 mode. Deep swarm + rejection tracking. Finds ideas the swarm can't kill. (~45 agent calls)

## How it works

1. Seed text gets fragmented mechanically (no AI)
2. Each fragment pairs with a random perspective lens
3. Agents expand fragments independently
4. Outputs randomly collide in tournament bracket rounds
5. Each collision round uses fresh lenses
6. Filter scores final outputs on novelty, coherence, generativity

## ACRE-8 (Adversarial Concept Resurrection Engine)

ACRE-8 adds a rejection-tracking layer to the deep swarm. After each collision round, extraction agents identify which ideas were killed. Scan agents then check if killed ideas independently resurface in later rounds. Ideas that survive rejection — "deep grafts" — get re-injected into later collisions with a tag marking them as load-bearing. The most important output is often not what the swarm kept, but what it tried to kill and couldn't.

## Key principle

No single component ever holds the complete picture of the creative trajectory. The system is creative, not any individual agent.
