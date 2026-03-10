<p align="center">
  <img src="assets/banner.png" alt="Blind Swarm" width="100%" />
</p>

# Blind Swarm

**Tired of AI giving you the same 10 business ideas everyone else gets?** So were we.

Blind Swarm is a Claude Code plugin that throws out the playbook. Instead of one AI thinking really hard, it spawns **dozens of independent agents** — each blind to the others — and smashes their outputs together randomly. Like ants building a bridge they don't understand. Like jazz musicians who've never met playing the same song.

The result? Ideas that no single AI (or human) would ever produce alone.

> *"Because AI asking AI produces the same ideas every time."*

---

## How it works

```
Your seed text
     |
     v
  [Fragment into pieces]
     |
     v
  8-16 blind agents, each with:
  - A random fragment of your idea
  - A random perspective lens ("retired spy", "9-year-old", "mycologist")
  - ZERO knowledge of other agents
     |
     v
  Random collision tournament
  Round 1: 16 → 8 (forced synthesis)
  Round 2: 8 → 4
  Round 3: 4 → 2
  Round 4: 2 → 1
     |
     v
  Filter scores: Novelty × Coherence × Generativity
     |
     v
  Ideas you've never seen before
```

**The randomness is the feature, not a bug.** No orchestrator. No shared context. No optimization. Just emergence.

## Install

Add the marketplace:

```
/plugin marketplace add Joncik91/blind-swarm
```

Install the plugin:

```
/plugin install blind-swarm@blind-swarm-marketplace
```

Restart Claude Code. Done.

## Commands

### `/swarm <seed>` — Standard

8 agents. 3 collision rounds. 15 total agent calls. The sweet spot.

```
/swarm I want to build a SaaS but everything already exists
```

### `/swarm-quick <seed>` — Fast

4 agents. 2 rounds. 7 agent calls. Quick and dirty.

```
/swarm-quick monetizing AI skills without building another wrapper
```

### `/swarm-deep <seed>` — Full Chaos

16 agents. 4 rounds. 31 agent calls. Injects **wild DNA fragments** (random concepts NOT in your seed). Rotates through 4 lens pools:

| Round | Lens Pool | Vibe |
|-------|-----------|------|
| 0 | Human Extremes | "retired spy", "beekeeper philosopher" |
| 1 | Altered States | "astronaut seeing Earth", "10-year coma awakening" |
| 2 | Non-Human | "a river deciding where to flow", "a virus optimizing" |
| 3-4 | Paradox Minds | "expert who knows nothing", "mirror reflecting what isn't there" |

Scores on **Weirdness** as a bonus metric. Shows the full evolutionary tree.

```
/swarm-deep the future of human-AI interaction
```

## Why this exists

We had a 2-hour conversation with Claude about what to build. Every idea was predictable. We could literally predict what Claude would suggest before it said it.

So we asked: **what if AI agents couldn't predict each other?**

The answer is this plugin. Agents that are deliberately kept blind, collided randomly, and filtered for surprise. The system is creative because no individual component is trying to be.

## The philosophy

- **No single agent holds the full picture** — ever
- **The fragmenter is intentionally dumb** — smart fragmentation introduces bias
- **Pairings are `random.shuffle()`** — not semantic, not optimized
- **Each round uses different perspective lenses** — prevents convergence
- **The filter only runs at the end** — it surfaces, it doesn't steer

## Fair warning

- `/swarm-deep` spawns 31 agents. It takes a few minutes and uses real tokens.
- Results range from "genuinely brilliant" to "beautiful nonsense." That's the point.
- You might get an idea so weird it circles back to genius. Or it might just be weird.
- Either way, you won't get "have you considered a micro-SaaS for freelancers?"

## License

MIT — do whatever you want with it.

---

<p align="center">
  <i>Built during a conversation where a human kept outsmarting an AI,<br/>and the AI got tired of being predictable.</i>
</p>
