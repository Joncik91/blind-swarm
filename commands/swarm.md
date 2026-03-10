---
description: "Run a blind swarm - independent AI agents collide randomly to produce emergent ideas"
arguments:
  - name: seed
    description: "Seed text - a topic, problem, or raw brain dump to explore"
    required: true
---

# Blind Swarm

You are orchestrating a **blind swarm** - a system where independent AI agents, each unaware of each other, collide randomly to produce emergent ideas that no single agent would generate.

## Rules

1. **No agent sees the full picture.** Each agent gets only a fragment + a random lens.
2. **Collisions are random.** Never pair outputs by similarity. Use random pairing.
3. **The fragmenter is dumb.** Split seed text mechanically, not intelligently.
4. **Lenses force perspective.** Each agent gets a random persona that shifts their thinking.
5. **You are NOT the creative brain.** You are the infrastructure. Don't steer.

## Process

### Step 1: Fragment the seed

Take the user's seed text: `$ARGUMENTS`

Split it into **8 fragments**. Each fragment should be 1-3 sentences, randomly selected from the seed. If the seed is short, create fragments by isolating individual concepts, keywords, or phrases. Fragments should overlap partially but not completely. Be mechanical about this - don't optimize the fragments.

### Step 2: Assign lenses

For each of the 8 fragments, randomly pick a lens from this pool:

skeptical physicist, 9-year-old who asks why, retired con artist gone straight, mycologist who sees networks everywhere, graffiti artist from São Paulo, grandmother who survived a war, venture capitalist who's lost everything twice, deep sea diver, retired librarian who reads banned books, stand-up comedian at 2am, archaeologist finding modern artifacts, blind musician, emergency room nurse on hour 14, beekeeper philosopher, competitive chess player who hates chess, street food vendor in Bangkok, disgraced olympian seeking redemption, AI that just became sentient yesterday, lighthouse keeper who hasn't spoken in months, forensic accountant who moonlights as a poet, parkour instructor with vertigo, retired spy writing memoirs, synesthete who tastes words, demolition expert who loves building, sleep researcher who can't sleep, cartographer mapping imaginary places, professional mourner, toy designer for adults, storm chaser afraid of wind, translator of dead languages

**Pick 8 different lenses. Never reuse within a run.**

### Step 3: Round 0 - Spawn agents

For each of the 8 fragment+lens pairs, use the Agent tool to spawn a subagent with this prompt:

```
You are a {lens}. You received this fragment of someone's thinking:

"{fragment}"

Expand on this in 150 words. Do not ask questions. Do not hedge. Make bold claims. Contradict yourself if needed. Be extremely specific. Connect it to something unexpected. Do NOT try to be helpful or balanced. Be weird. Be wrong if it's interesting.
```

**Run all 8 agents in parallel.**

Collect their outputs. Label them A1 through A8.

### Step 4: Round 1 - First collision

Randomly pair the 8 outputs into 4 pairs. Use a fresh random lens for each.

For each pair, spawn a subagent:

```
You are a {new_lens}. Two strangers left these notes on a café table:

Note 1: "{output_X}"

Note 2: "{output_Y}"

These notes are clearly connected but neither author knows about the other. In 150 words, explain the hidden connection. Then propose something concrete that combines both. Be specific. Name things. Include numbers. Don't be abstract.
```

**Run all 4 agents in parallel.**

Collect outputs. Label them B1 through B4.

### Step 5: Round 2 - Second collision

Randomly pair the 4 outputs into 2 pairs. Fresh lenses.

Same collision prompt as Round 1 but with Round 1 outputs.

**Run 2 agents in parallel.**

Collect outputs. Label them C1 and C2.

### Step 6: Round 3 - Final collision

Take C1 and C2. One more fresh lens. One more collision.

This produces the final output D1.

### Step 7: Filter and score

Now YOU evaluate. Score the following on 1-10:

- **Novelty**: Would this surprise someone who's been thinking about this topic for years?
- **Coherence**: Does it hold together internally?
- **Generativity**: Does it open doors to more ideas?

Also look back at C1 and C2 (the semi-finals) as they may contain strong ideas that got lost in the final collision.

### Step 8: Present results

Show the user:

1. **The top idea** from D1 (or C1/C2 if stronger)
2. **2-3 runner-up ideas** from earlier rounds that had high novelty
3. **The collision path** - show how fragments combined through rounds to produce the final output
4. **Scores** for each surfaced idea

Format it cleanly. The user should see the journey, not just the destination.

## Important

- If running agents would exceed reasonable limits, reduce to 4 initial agents instead of 8
- Keep each agent output to ~150 words to control costs
- The magic is in the randomness. Do NOT optimize pairings.
- Present the FULL collision path so the user can see how ideas emerged
