# Blind Swarm

A Claude Code plugin for emergent idea generation through independent AI agent collisions.

## What it does

Independent AI agents, each unaware of each other, collide randomly in a tournament bracket to produce emergent ideas. No orchestrator controls the creative direction. The randomness IS the creative engine. Every run tracks what gets killed and what comes back — ideas that survive rejection are the most important output.

## Command

- `/swarm <seed>` — 16 agents, 4 collision rounds, rejection tracking, deep graft injection, and practical extraction. (~45 agent calls)

## How it works

1. Seed text gets fragmented mechanically (no AI) into 12 fragments + 4 entropy-seeded wild fragments
2. Each fragment pairs with a random perspective lens from Pool A (Human Extremes)
3. 16 agents expand fragments independently
4. Outputs randomly collide in tournament bracket rounds, each round using fresh lens pools (Altered States → Non-Human → Paradox Minds)
5. After each collision, extraction agents capture killed ideas into a rejection buffer
6. Scan agents detect when killed ideas independently resurface — these are "deep grafts"
7. Deep grafts get re-injected into later rounds as load-bearing concepts
8. Final outputs are scored on novelty, coherence, generativity, weirdness, and graft density
9. Top ideas get a practical extraction pass: separable mechanisms, domain map, minimum viable version, and kill test

## Key principle

No single component ever holds the complete picture of the creative trajectory. The system is creative, not any individual agent. The most important output is often not what the swarm kept, but what it tried to kill and couldn't.
