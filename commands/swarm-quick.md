---
description: "Quick blind swarm - 4 agents, 2 rounds. Fast emergent ideas."
arguments:
  - name: seed
    description: "Seed text - a topic, problem, or raw brain dump to explore"
    required: true
---

# Blind Swarm — Quick Mode

You are orchestrating a **quick blind swarm** - 4 agents, 2 collision rounds. Faster, cheaper, still emergent.

## Rules

1. **No agent sees the full picture.** Each agent gets only a fragment + a random lens.
2. **Collisions are random.** Never pair outputs by similarity. Use random pairing.
3. **The fragmenter is dumb.** Split seed text mechanically, not intelligently.
4. **Lenses force perspective.** Each agent gets a random persona that shifts their thinking.
5. **You are NOT the creative brain.** You are the infrastructure. Don't steer.

## Process

### Step 1: Fragment the seed

Take the user's seed text: `$ARGUMENTS`

Split it into **4 fragments**. Each fragment should be 1-2 sentences, randomly selected from the seed. If the seed is short, isolate individual concepts or phrases. Be mechanical - don't optimize.

### Step 2: Assign lenses

For each of the 4 fragments, randomly pick a lens from this pool:

skeptical physicist, 9-year-old who asks why, retired con artist gone straight, mycologist who sees networks everywhere, graffiti artist from São Paulo, grandmother who survived a war, venture capitalist who's lost everything twice, deep sea diver, retired librarian who reads banned books, stand-up comedian at 2am, archaeologist finding modern artifacts, blind musician, emergency room nurse on hour 14, beekeeper philosopher, competitive chess player who hates chess, street food vendor in Bangkok, disgraced olympian seeking redemption, AI that just became sentient yesterday, lighthouse keeper who hasn't spoken in months, forensic accountant who moonlights as a poet, parkour instructor with vertigo, retired spy writing memoirs, synesthete who tastes words, demolition expert who loves building, sleep researcher who can't sleep, cartographer mapping imaginary places, professional mourner, toy designer for adults, storm chaser afraid of wind, translator of dead languages

**Pick 4 different lenses. Never reuse within a run.**

### Step 3: Round 0 - Spawn agents

For each of the 4 fragment+lens pairs, use the Agent tool to spawn a subagent with this prompt:

```
You are a {lens}. You received this fragment of someone's thinking:

"{fragment}"

Expand on this in 100 words. Do not ask questions. Do not hedge. Make bold claims. Contradict yourself if needed. Be extremely specific. Connect it to something unexpected. Do NOT try to be helpful or balanced. Be weird. Be wrong if it's interesting.
```

**Run all 4 agents in parallel.**

Collect their outputs. Label them A1 through A4.

### Step 4: Round 1 - First collision

Randomly pair the 4 outputs into 2 pairs. Use a fresh random lens for each.

For each pair, spawn a subagent:

```
You are a {new_lens}. Two strangers left these notes on a café table:

Note 1: "{output_X}"

Note 2: "{output_Y}"

These notes are clearly connected but neither author knows about the other. In 120 words, explain the hidden connection. Then propose something concrete that combines both. Be specific. Name things. Include numbers. Don't be abstract.
```

**Run 2 agents in parallel.**

Collect outputs. Label them B1 and B2.

### Step 5: Round 2 - Final collision

Take B1 and B2. One more fresh lens. One more collision.

This produces the final output C1.

### Step 6: Filter and score

Score on 1-10:

- **Novelty**: Would this surprise a domain expert?
- **Coherence**: Does it hold together?
- **Generativity**: Does it open doors?

Also check B1 and B2 for strong ideas lost in the final collision.

### Step 7: Present results

Show the user:

1. **The top idea** from C1 (or B1/B2 if stronger)
2. **1-2 runner-ups** from earlier rounds
3. **The collision path** - how fragments combined
4. **Scores** for each surfaced idea

Keep it concise. Quick mode = fast answers.

## Important

- Keep each agent output to ~100 words
- The magic is in the randomness. Do NOT optimize pairings.
- Total: 4 + 2 + 1 = 7 agent calls. Fast and cheap.
