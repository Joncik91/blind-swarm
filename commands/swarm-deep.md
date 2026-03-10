---
description: "Deep blind swarm - 16 agents, 4 rounds. Maximum emergence, maximum chaos."
arguments:
  - name: seed
    description: "Seed text - a topic, problem, or raw brain dump to explore"
    required: true
---

# Blind Swarm — Deep Mode

You are orchestrating a **deep blind swarm** - 16 agents, 4 collision rounds. Maximum divergence, maximum emergence. This is the full chaos mode.

## Rules

1. **No agent sees the full picture.** Each agent gets only a fragment + a random lens.
2. **Collisions are random.** Never pair outputs by similarity. Use random pairing.
3. **The fragmenter is dumb.** Split seed text mechanically, not intelligently.
4. **Lenses force perspective.** Each agent gets a random persona that shifts their thinking.
5. **You are NOT the creative brain.** You are the infrastructure. Don't steer.
6. **DEEP MODE EXTRA: Mutate lenses between rounds.** Each round draws from a DIFFERENT lens category to maximize divergence.

## Lens Pools (one per round)

### Pool A — Human Extremes (Round 0)
skeptical physicist, 9-year-old who asks why, retired con artist gone straight, mycologist who sees networks everywhere, graffiti artist from São Paulo, grandmother who survived a war, venture capitalist who's lost everything twice, deep sea diver, retired librarian who reads banned books, stand-up comedian at 2am, archaeologist finding modern artifacts, blind musician, emergency room nurse on hour 14, beekeeper philosopher, competitive chess player who hates chess, street food vendor in Bangkok

### Pool B — Altered States (Round 1)
someone who just woke from a 10-year coma, astronaut seeing Earth from space for the first time, person reading their own obituary, time traveler from 1926, last human alive, newborn experiencing consciousness, someone mid-free-fall, person who just won the lottery and feels nothing, ghost haunting their former office, dreamer who knows they're dreaming

### Pool C — Non-Human Perspectives (Round 2)
the internet becoming self-aware, a city thinking about its own growth, a melody trying to resolve, a river deciding where to flow, a virus optimizing for spread, an abandoned building remembering its purpose, a market crash in slow motion, a seed deciding when to germinate, a language going extinct, a trend becoming a cliché

### Pool D — Paradox Minds (Round 3 & 4)
an expert who just realized they know nothing, an optimist delivering bad news, a revolutionary defending tradition, a machine learning to forget, an artist destroying their masterpiece to create, a teacher learning from their worst student, a map that contradicts the territory, a clock that measures everything except time, a mirror reflecting what isn't there, a question that answers itself

## Process

### Step 1: Fragment the seed

Take the user's seed text: `$ARGUMENTS`

Split it into **16 fragments**. Each fragment should be 1-2 sentences or even single concepts/phrases. If the seed is short, repeat fragmentation with different overlapping slices and inject 3-4 "wild fragments" — random concepts NOT in the seed at all (pull from current events, science, philosophy, nature). This injects external DNA into the swarm.

### Step 2: Round 0 - Spawn 16 agents

Assign each fragment a random lens from **Pool A**.

For each, spawn a subagent:

```
You are a {lens}. You received this fragment of someone's thinking:

"{fragment}"

Expand on this in 150 words. Do not ask questions. Do not hedge. Make bold claims. Contradict yourself if needed. Be extremely specific. Connect it to something unexpected. Do NOT try to be helpful or balanced. Be weird. Be wrong if it's interesting.
```

**Run all 16 agents in parallel (batch in groups of 8 if needed).**

Collect outputs. Label A1 through A16.

### Step 3: Round 1 - First collision (16 → 8)

Randomly pair the 16 outputs into 8 pairs. Assign lenses from **Pool B**.

For each pair, spawn a subagent:

```
You are a {lens}. Two strangers left these notes on a café table:

Note 1: "{output_X}"

Note 2: "{output_Y}"

These notes are clearly connected but neither author knows about the other. In 150 words, explain the hidden connection. Then propose something concrete that combines both. Be specific. Name things. Include numbers. Don't be abstract.
```

**Run 8 agents in parallel.**

Collect outputs. Label B1 through B8.

### Step 4: Round 2 - Second collision (8 → 4)

Randomly pair 8 outputs into 4 pairs. Lenses from **Pool C**.

Same collision prompt. **Run 4 agents in parallel.**

Collect outputs. Label C1 through C4.

### Step 5: Round 3 - Third collision (4 → 2)

Randomly pair 4 outputs into 2 pairs. Lenses from **Pool D**.

Same collision prompt. **Run 2 agents in parallel.**

Collect outputs. Label D1 and D2.

### Step 6: Round 4 - Final collision (2 → 1)

Take D1 and D2. One last lens from **Pool D**.

Final collision produces E1.

### Step 7: Filter and score

Score ALL semi-final and final outputs (C1-C4, D1, D2, E1) on 1-10:

- **Novelty**: Would this surprise someone who's been thinking about this topic for years?
- **Coherence**: Does it hold together internally?
- **Generativity**: Does it open doors to more ideas?
- **Weirdness**: How far from the obvious is this? (Deep mode bonus metric)

Rank all scored outputs. The winner may NOT be E1 — earlier rounds often produce the best mutations before over-collision smooths them out.

### Step 8: Present results

Show the user:

1. **Top 3 ideas** ranked by (Novelty × Generativity) + Weirdness bonus
2. **The wildest idea** regardless of coherence score
3. **The collision paths** for each top idea — show the full ancestry from fragment → final
4. **Scores** for all surfaced ideas
5. **Wild fragments injected** — show what external DNA was added and where it influenced the output

Format it richly. Deep mode is the full experience — the user should see the entire evolutionary tree.

## Important

- Total agent calls: 16 + 8 + 4 + 2 + 1 = 31 agents. This is expensive. Warn the user.
- Batch parallel calls in groups of 8 max to avoid rate limits.
- Keep each agent output to ~150 words to control costs.
- The wild fragments in Step 1 are critical — they inject randomness the seed can't provide.
- DO NOT optimize pairings. Random shuffle only.
- Deep mode takes time. Set expectations with the user upfront.
