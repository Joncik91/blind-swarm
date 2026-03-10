---
description: "Generate entropy seeds for wild fragment injection (internal helper)"
---

# Entropy Seed Generator

This is an internal reference used by swarm commands to generate truly random wild fragments.

## The Problem

LLMs have favorite metaphors (mycelium, cathedrals, mirrors). When asked to "pick something random," they gravitate to the same concepts. Wild fragments must be seeded with external entropy to break this bias.

## Method

Before generating wild fragments, run this bash command to produce entropy seeds:

```bash
# Pull random words as entropy seeds
shuf -n 8 /usr/share/dict/words 2>/dev/null || python3 -c "
import random, string
domains = [
    'kitchen tools', 'marine biology', 'medieval trades', 'particle physics',
    'folk instruments', 'extinct animals', 'surgical procedures', 'board games',
    'textile techniques', 'volcanic phenomena', 'postal systems', 'fermentation',
    'cartography terms', 'circus arts', 'mining equipment', 'dance forms',
    'architectural failures', 'weather anomalies', 'prison slang', 'perfumery',
    'radio frequencies', 'knot types', 'gambling mathematics', 'sleep disorders',
    'typeface anatomy', 'beekeeping crises', 'elevator mechanics', 'tea ceremonies',
    'cryptographic attacks', 'bird migration errors', 'paint chemistry', 'debt instruments',
    'tidal patterns', 'competitive eating', 'bell casting', 'moss ecology',
    'freight logistics', 'optical illusions', 'shoe construction', 'sourdough failures',
    'bridge collapses', 'insect mimicry', 'calendar reform', 'tattoo removal',
    'satellite decay', 'cheese aging', 'wildfire behavior', 'fountain pen inks',
    'submarine acoustics', 'seed dormancy', 'roller coaster physics', 'language death'
]
picks = random.sample(domains, 4)
years = [random.randint(800, 2024) for _ in range(4)]
for i in range(4):
    print(f'{picks[i]} | {years[i]}')
" 2>/dev/null || node -e "
const domains = [
    'kitchen tools', 'marine biology', 'medieval trades', 'particle physics',
    'folk instruments', 'extinct animals', 'surgical procedures', 'board games',
    'textile techniques', 'volcanic phenomena', 'postal systems', 'fermentation',
    'cartography terms', 'circus arts', 'mining equipment', 'dance forms',
    'architectural failures', 'weather anomalies', 'prison slang', 'perfumery',
    'radio frequencies', 'knot types', 'gambling mathematics', 'sleep disorders',
    'typeface anatomy', 'beekeeping crises', 'elevator mechanics', 'tea ceremonies',
    'cryptographic attacks', 'bird migration errors', 'paint chemistry', 'debt instruments',
    'tidal patterns', 'competitive eating', 'bell casting', 'moss ecology',
    'freight logistics', 'optical illusions', 'shoe construction', 'sourdough failures',
    'bridge collapses', 'insect mimicry', 'calendar reform', 'tattoo removal',
    'satellite decay', 'cheese aging', 'wildfire behavior', 'fountain pen inks',
    'submarine acoustics', 'seed dormancy', 'roller coaster physics', 'language death'
];
const shuffle = arr => arr.sort(() => Math.random() - 0.5);
const picks = shuffle([...domains]).slice(0, 4);
const years = Array.from({length: 4}, () => Math.floor(Math.random() * 1224) + 800);
picks.forEach((p, i) => console.log(p + ' | ' + years[i]));
"
```

Each output line is an **entropy seed**. Use it to force the wild fragment generation prompt:

```
Generate a single provocative, specific wild fragment (one sentence) inspired by this random seed: "{entropy_seed}"

The fragment must be a concrete, surprising fact or metaphor drawn from the seed domain. Do NOT mention AI, technology, mycelium, cathedrals, mirrors, or any concept from the user's original seed. Be hyper-specific. Include a number, a name, or a place.
```

This ensures every run produces genuinely different wild DNA — the LLM can't fall back on favorites because the seed domain is chosen by entropy, not by preference.
