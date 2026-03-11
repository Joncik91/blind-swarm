---
description: "Run a blind swarm — 16 agents collide through 4 rounds with rejection tracking. Finds ideas AI can't kill."
arguments:
  - name: seed
    description: "Seed text - a topic, problem, or raw brain dump to explore"
    required: true
---

# Blind Swarm

You are orchestrating a **blind swarm** — a system where 16 independent AI agents, each unaware of each other, collide randomly in a tournament bracket to produce emergent ideas. The system tracks what gets killed and what comes back uninvited.

**Total agent calls: ~45** (31 swarm + ~14 rejection tracking)

## Rules

1. **No agent sees the full picture.** Each agent gets only a fragment + a random lens.
2. **Collisions are random.** Never pair outputs by similarity. Use random pairing.
3. **The fragmenter is dumb.** Split seed text mechanically, not intelligently.
4. **Lenses force perspective.** Each agent gets a random persona that shifts their thinking.
5. **You are NOT the creative brain.** You are the infrastructure. Don't steer.
6. **Mutate lenses between rounds.** Each round draws from a DIFFERENT lens category to maximize divergence.
7. **Rejection is data, not deletion.** Every collision kills ideas. Capture what dies.
8. **Resurrection is signal.** An idea that comes back uninvited is more important than one that was kept.
9. **Deep grafts are sacred.** They get re-injected with a tag. They cannot be killed twice.
10. **Track lineage obsessively.** Every rejected concept gets a source ID and round-of-death.

## Lens Pools

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

Split it into **10 seed fragments** + **6 wild fragments** (16 total). Each fragment should be 1-2 sentences or even single concepts/phrases. If the seed is short, repeat fragmentation with different overlapping slices.

#### Wild Fragment Generation (MAXIMUM ENTROPY)

**Do NOT invent wild fragments from your own imagination.** LLMs have favorite metaphors and will repeat them across runs. Wild fragments must be seeded with external entropy — and maximally so.

Each wild fragment is generated from a **cross-domain collision**: two random domains forced together, plus a random year, a random geographic anchor, and a random constraint modifier. This produces combinations no LLM would ever default to.

Run this bash command to generate 6 entropy seeds:

```bash
python3 -c "
import random, os

# Use system entropy for seeding
random.seed(int.from_bytes(os.urandom(16), 'big'))

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
    'submarine acoustics', 'seed dormancy', 'roller coaster physics', 'language death',
    'embalming chemistry', 'lock picking', 'coral spawning', 'trebuchet mechanics',
    'paper marbling', 'ice core dating', 'sword tempering', 'pigeon racing',
    'silk degumming', 'controlled demolition', 'sake brewing', 'falconry jargon',
    'glass blowing defects', 'mushroom spore dispersal', 'rope splicing', 'tide mill operation',
    'bone setting', 'clay pipe manufacture', 'fog harvesting', 'whale fall ecology',
    'thatch roofing', 'compass deviation', 'lace bobbin turning', 'peat cutting',
    'kiln drying failures', 'anchor dragging', 'candle clock calibration', 'leech therapy',
    'grain elevator explosions', 'pearl cultivation errors', 'chimney swift migration',
    'vinegar fermentation accidents', 'hot air balloon envelope repair', 'oyster reef restoration',
    'hand bell ringing patterns', 'wool carding', 'quicksand mechanics', 'bat echolocation jamming',
    'salt mine collapses', 'coconut coir processing', 'auroral sound phenomena',
    'bookbinding adhesive failure', 'termite mound ventilation', 'horse collar evolution',
    'magnet fishing finds', 'obsidian knapping', 'plague doctor equipment', 'rainwater harvesting failures',
    'saffron adulteration', 'shipworm damage patterns', 'smoke signal encoding',
    'spider silk harvesting', 'steam hammer calibration', 'stone wall dry stacking',
    'sunflower heliotropism', 'tar distillation', 'tuning fork acoustics', 'umbrella frame mechanics',
    'vellum preparation', 'water divining', 'wax seal forensics', 'yeast mutation'
]

modifiers = [
    'a failure in', 'the opposite of', 'the last known', 'the accidental discovery of',
    'the forbidden practice of', 'the sound of', 'the smell of', 'what happens after',
    'the moment before', 'the wrong way to', 'the oldest surviving', 'the smallest',
    'the heaviest', 'a counterfeit', 'a abandoned', 'the night shift of',
    'the illegal version of', 'the children\\'s version of', 'the underwater variant of',
    'the wartime substitute for', 'the blind version of', 'the silent form of',
    'the poisonous cousin of', 'the fossilized remains of', 'the amateur mistake in'
]

places = [
    'Osaka', 'Reykjavik', 'Mombasa', 'Ulaanbaatar', 'Valparaíso', 'Bruges',
    'Tbilisi', 'Manaus', 'Tromsø', 'Jaipur', 'Antananarivo', 'Sarajevo',
    'Cusco', 'Zanzibar', 'Yakutsk', 'Oaxaca', 'Tallinn', 'Bhutan',
    'Svalbard', 'Aleppo', 'Palermo', 'Irkutsk', 'Fez', 'Hanoi',
    'Lhasa', 'Dakar', 'Ushuaia', 'Tashkent', 'Split', 'Luang Prabang',
    'Baku', 'Arequipa', 'Windhoek', 'Plovdiv', 'Gyeongju', 'Matera',
    'Kairouan', 'Potosí', 'Bukhara', 'Nara'
]

# Pick 12 domains (6 pairs), 6 modifiers, 6 places, 6 years
picks = random.sample(domains, 12)
mods = random.sample(modifiers, 6)
locs = random.sample(places, 6)
years = [random.randint(800, 2024) for _ in range(6)]

for i in range(6):
    print(f'{mods[i]} {picks[i*2]} × {picks[i*2+1]} | {locs[i]} | {years[i]}')
" 2>/dev/null || node -e "
const crypto=require('crypto');
const d=['kitchen tools','marine biology','medieval trades','particle physics','folk instruments','extinct animals','surgical procedures','board games','textile techniques','volcanic phenomena','postal systems','fermentation','cartography terms','circus arts','mining equipment','dance forms','architectural failures','weather anomalies','prison slang','perfumery','radio frequencies','knot types','gambling mathematics','sleep disorders','typeface anatomy','beekeeping crises','elevator mechanics','tea ceremonies','cryptographic attacks','bird migration errors','paint chemistry','debt instruments','tidal patterns','competitive eating','bell casting','moss ecology','freight logistics','optical illusions','shoe construction','sourdough failures','bridge collapses','insect mimicry','calendar reform','tattoo removal','satellite decay','cheese aging','wildfire behavior','fountain pen inks','submarine acoustics','seed dormancy','roller coaster physics','language death','embalming chemistry','lock picking','coral spawning','trebuchet mechanics','paper marbling','ice core dating','sword tempering','pigeon racing','silk degumming','controlled demolition','sake brewing','falconry jargon','glass blowing defects','mushroom spore dispersal','rope splicing','tide mill operation','bone setting','clay pipe manufacture','fog harvesting','whale fall ecology','thatch roofing','compass deviation','lace bobbin turning','peat cutting','kiln drying failures','anchor dragging','candle clock calibration','leech therapy','grain elevator explosions','pearl cultivation errors','chimney swift migration','vinegar fermentation accidents','hot air balloon envelope repair','oyster reef restoration','hand bell ringing patterns','wool carding','quicksand mechanics','bat echolocation jamming','salt mine collapses','coconut coir processing','auroral sound phenomena','bookbinding adhesive failure','termite mound ventilation','horse collar evolution','magnet fishing finds','obsidian knapping','plague doctor equipment','rainwater harvesting failures','saffron adulteration','shipworm damage patterns','smoke signal encoding','spider silk harvesting','steam hammer calibration','stone wall dry stacking','sunflower heliotropism','tar distillation','tuning fork acoustics','umbrella frame mechanics','vellum preparation','water divining','wax seal forensics','yeast mutation'];
const m=['a failure in','the opposite of','the last known','the accidental discovery of','the forbidden practice of','the sound of','the smell of','what happens after','the moment before','the wrong way to','the oldest surviving','the smallest','the heaviest','a counterfeit','a abandoned','the night shift of','the illegal version of','the children\\'s version of','the underwater variant of','the wartime substitute for','the blind version of','the silent form of','the poisonous cousin of','the fossilized remains of','the amateur mistake in'];
const p=['Osaka','Reykjavik','Mombasa','Ulaanbaatar','Valparaíso','Bruges','Tbilisi','Manaus','Tromsø','Jaipur','Antananarivo','Sarajevo','Cusco','Zanzibar','Yakutsk','Oaxaca','Tallinn','Bhutan','Svalbard','Aleppo','Palermo','Irkutsk','Fez','Hanoi','Lhasa','Dakar','Ushuaia','Tashkent','Split','Luang Prabang','Baku','Arequipa','Windhoek','Plovdiv','Gyeongju','Matera','Kairouan','Potosí','Bukhara','Nara'];
function sh(a){const b=[...a];for(let i=b.length-1;i>0;i--){const j=crypto.randomInt(i+1);[b[i],b[j]]=[b[j],b[i]];}return b;}
const ds=sh(d).slice(0,12),ms=sh(m).slice(0,6),ps=sh(p).slice(0,6);
for(let i=0;i<6;i++)console.log(ms[i]+' '+ds[i*2]+' × '+ds[i*2+1]+' | '+ps[i]+' | '+(800+crypto.randomInt(1224)));
"
```

Each line is a **cross-domain entropy seed** with 5 randomized axes:
- **Modifier** — constrains the angle of approach (25 options)
- **Domain A × Domain B** — two unrelated fields forced into collision (100+ domains)
- **Place** — geographic anchor prevents generic framing (40 cities across 6 continents)
- **Year** — temporal anchor (800-2024)

Example output: `the forbidden practice of bell casting × bat echolocation jamming | Potosí | 1347`

For each entropy seed, spawn a subagent:

```
Generate a single provocative, hyper-specific wild fragment (one sentence) inspired by this entropy seed: "{entropy_seed}"

The fragment must collide BOTH domains through the modifier's lens, anchored in the specific place and era. It must be a concrete, surprising fact or metaphor — not abstract. Include a specific number, name, or measurement. Do NOT use concepts from the user's seed text. Do NOT mention AI, technology, mycelium, cathedrals, or mirrors. The weirder and more specific, the better.
```

Label wild fragments F11-F16 and mark as WILD.

### Step 2: Initialize the Rejection Buffer

Create an internal tracking structure. Maintain it as running state throughout the swarm:

```
REJECTION_BUFFER = []
DEEP_GRAFTS = []

Each rejected concept entry:
{
  id: "REJ-{round}-{number}",
  concept: "<2-3 sentence summary of the killed idea>",
  keywords: [<3-5 key terms>],
  source_collision: "<which collision produced then killed it>",
  round_killed: <number>,
  parent_outputs: ["<A3>", "<A7>"],
  resurrection_count: 0
}

Each deep graft entry:
{
  id: "GRAFT-{number}",
  original_rejection: "REJ-{round}-{number}",
  resurfaced_in: "<output ID where it reappeared>",
  round_resurfaced: <number>,
  similarity: "<how the resurfaced version relates to the original>",
  resurrection_count: <number>,
  injected_into: [<list of rounds/collisions where it was re-injected>]
}
```

### Step 3: Round 0 — Spawn 16 agents

Assign each fragment a random lens from **Pool A**.

For each, spawn a subagent:

```
You are a {lens}. You received this fragment of someone's thinking:

"{fragment}"

Expand on this in 150 words. Do not ask questions. Do not hedge. Make bold claims. Contradict yourself if needed. Be extremely specific. Connect it to something unexpected. Do NOT try to be helpful or balanced. Be weird. Be wrong if it's interesting.
```

**Run all 16 agents in parallel (batch in groups of 8 if needed).**

Collect outputs. Label A1 through A16.

### Step 4: Round 1 — First collision (16 → 8) + EXTRACTION

#### 4a: Collision

Randomly pair the 16 outputs into 8 pairs. Assign lenses from **Pool B**.

Standard collision prompt:

```
You are a {lens}. Two strangers left these notes on a café table:

Note 1: "{output_X}"

Note 2: "{output_Y}"

These notes are clearly connected but neither author knows about the other. In 150 words, explain the hidden connection. Then propose something concrete that combines both. Be specific. Name things. Include numbers. Don't be abstract.
```

**Run 8 agents in parallel.** Collect outputs B1-B8.

#### 4b: Extraction

For each of the 8 collisions, spawn an extraction agent:

```
You are a forensic analyst of killed ideas.

Two inputs went into a collision:
Input 1: "{input_X}"
Input 2: "{input_Y}"

The collision produced:
Output: "{collision_output}"

What ideas, images, concepts, or claims from the inputs were DROPPED or LOST in the output? List 2-4 specific concepts that were present in the inputs but absent from the output. For each, give:
- A 1-2 sentence description of the killed concept
- 3-5 keywords that capture its essence
- Which input it came from

Be precise. Only list things genuinely absent from the output, not things that were rephrased.
```

**Run 8 extraction agents in parallel** (can batch with collision agents if within limits).

Parse results. Add each killed concept to REJECTION_BUFFER with appropriate metadata.

### Step 5: Round 2 — Second collision (8 → 4) + SCAN + EXTRACTION

#### 5a: Collision

Randomly pair 8 outputs into 4 pairs. Lenses from **Pool C**. Standard collision prompt. **Run 4 agents in parallel.** Collect C1-C4.

#### 5b: Resurrection Scan

For each of the 4 new outputs (C1-C4), spawn a scan agent:

```
You are searching for resurrected ideas.

Here is a new output from a creative collision:
"{new_output}"

Here is a list of concepts that were REJECTED in previous rounds:
{rejection_buffer_formatted}

Does the new output contain ANY concept that matches or closely resembles a previously rejected concept? The match does NOT need to be exact — it could be the same core idea expressed differently, the same metaphor in a new context, or the same structural insight with different surface details.

For each match found, report:
- Which rejected concept ID it matches (REJ-X-Y)
- The specific passage in the new output that resembles it
- How similar they are: ECHO (surface similarity), RHYME (structural similarity), or RESURRECTION (core idea independently reinvented)
- A 1-sentence explanation of why this is the same underlying idea

If no matches, say "NO MATCHES" and nothing else.
```

**Run 4 scan agents in parallel.**

Any match rated RHYME or RESURRECTION → add to DEEP_GRAFTS. Increment resurrection_count on the original rejection.

#### 5c: Extraction

Same as Step 4b but for the 4 Round 2 collisions. **Run 4 extraction agents in parallel.**

Add new killed concepts to REJECTION_BUFFER.

### Step 6: Round 3 — Third collision (4 → 2) + DEEP GRAFT INJECTION

#### 6a: Collision with graft injection

If DEEP_GRAFTS is non-empty, modify the collision prompt:

```
You are a {lens from Pool D}. Two strangers left these notes on a café table:

Note 1: "{output_X}"

Note 2: "{output_Y}"

These notes are clearly connected but neither author knows about the other.

ALSO: The following ideas were previously rejected by other thinkers working on this same problem — but they kept coming back independently. They may be load-bearing concepts that the problem needs:

{for each deep graft:}
- DEEP GRAFT [{graft.id}]: "{graft.concept}" — This was killed in Round {killed} but resurfaced independently in Round {resurfaced}. It survived rejection. Take it seriously.
{end for}

In 200 words (longer than usual — you have more material), explain the hidden connection between the notes. Incorporate any deep grafts that genuinely fit — but do NOT force them. If a deep graft doesn't belong, ignore it. Then propose something concrete that combines everything. Be specific. Name things. Include numbers.
```

If no deep grafts exist, use the standard collision prompt.

**Run 2 agents in parallel.** Collect D1 and D2.

#### 6b: Scan + Extraction

Run scan against full rejection buffer. Run extraction on the 2 collisions.

Any NEW resurrections found here are **double grafts** — ideas that survived two rounds of rejection. Flag them specially.

### Step 7: Round 4 — Final collision (2 → 1) + DEEP GRAFT INJECTION

Same deep graft injection as Round 3, including any new grafts found in Round 3.

Final collision produces E1.

Run one last scan on E1 against the full buffer.

### Step 8: Score

Score ALL semi-final and final outputs (C1-C4, D1, D2, E1) on 1-10:

- **Novelty**: Would this surprise someone who's been thinking about this topic for years?
- **Coherence**: Does it hold together internally?
- **Generativity**: Does it open doors to more ideas?
- **Weirdness**: How far from the obvious is this?
- **Graft Density**: How many deep grafts influenced this output?

Ranking formula: **(Novelty × Generativity) + Weirdness bonus + (Graft Density × 5)**

The graft density bonus is intentional. Ideas shaped by deep grafts have been tested by rejection and survived. They earn extra weight.

### Step 9: Extract (SEED-ANCHORED)

The swarm is wild on purpose. The extraction step is where the output gets translated back into the user's actual domain. **This is the bridge between creative chaos and practical utility.**

First, determine the seed's domain from the original input:
- If the seed is about software, apps, digital products, SaaS, APIs → domain is **digital product**
- If the seed is about physical goods, manufacturing, hardware → domain is **physical product**
- If the seed is about research, science, understanding → domain is **research**
- If the seed is about art, expression, culture → domain is **creative**
- If unclear or abstract → domain is **open** (no constraint)

For each of the top 3 ideas, run an extraction pass. This is NOT a separate agent — YOU do this analysis:

**IMPORTANT: Strip away the swarm's surface metaphors and translate the STRUCTURAL insight into the seed's domain.** If the swarm produced a bee hive monitoring system but the seed was about SaaS, the mechanism isn't "bee hive monitoring" — it's "biological decay signal as a proxy for engagement decay." If the swarm produced a ceramic residency but the seed was about software, the mechanism isn't "fire clay at 1,260°C" — it's "irreversible transformation as commitment device."

The swarm's job was to find structural insights through wild collision. Your job is to carry those structures into the domain the user actually cares about. Every mechanism, every domain map entry, every MVT must be something the user could actually build or test in their world.

For each top idea, produce:

**MECHANISMS** — What are the separable structural insights? Strip the metaphor. Name the underlying pattern. Each mechanism should be domain-independent but your description should translate it into the seed's domain.

**DOMAIN MAP** — Where could each mechanism apply within the seed's domain? Name specific products, tools, features, or experiments — not art installations or residencies (unless the seed is about art).

**MINIMUM VIABLE VERSION** — What's the smallest, fastest version of this the user could build or test in their actual domain? One weekend. One API. One experiment. If the seed is about digital products, the MVT must be a digital product. If it's about SaaS, the MVT must be software. No physical residencies unless the seed asked for one.

**KILL TEST** — What would make this idea die in practice in the user's domain? Be specific to their world, not to the swarm's metaphorical framing.

### Step 10: Present results

Show the user:

#### Swarm output:
1. **Top 3 ideas** ranked by score
2. **The wildest idea** regardless of coherence
3. **Collision paths** for each top idea
4. **Scores** for all surfaced ideas
5. **Wild fragment influence map**
6. **Key emergent themes**

#### Rejection tracking:

7. **DEEP GRAFT REPORT**

For each deep graft found:

```
┌─────────────────────────────────────────────────────────┐
│ DEEP GRAFT: {graft.id}                                  │
├─────────────────────────────────────────────────────────┤
│ Concept: {description}                                  │
│ Killed in: Round {N}, collision {X}+{Y} → {Z}          │
│ Resurfaced in: Round {M}, output {W}                    │
│ Match type: RESURRECTION / RHYME / ECHO                 │
│ Resurrection count: {N}                                 │
│ Injected into: Round {R} collisions                     │
│ Final influence: {which top ideas it shaped}            │
│                                                         │
│ WHY IT MATTERS: {1-2 sentences on why this idea         │
│ couldn't be killed — what does that tell us about       │
│ the problem space?}                                     │
└─────────────────────────────────────────────────────────┘
```

8. **REJECTION GRAVEYARD**

The full rejection buffer, organized by round. Ideas that died and STAYED dead — they tell you what the problem space doesn't need.

```
┌────────────┬──────────────────────────┬───────────┬──────────────┐
│ ID         │ Concept                  │ Killed in │ Resurrected? │
├────────────┼──────────────────────────┼───────────┼──────────────┤
│ REJ-1-01   │ ...                      │ Round 1   │ No           │
│ REJ-1-02   │ ...                      │ Round 1   │ → GRAFT-1   │
│ ...        │                          │           │              │
└────────────┴──────────────────────────┴───────────┴──────────────┘
```

9. **SUMMARY STATISTICS**

```
Total concepts tracked: {N}
Rejected in Round 1: {N}
Rejected in Round 2: {N}
Rejected in Round 3: {N}
Resurrected (ECHO): {N}
Resurrected (RHYME): {N}
Resurrected (RESURRECTION): {N}
Deep grafts found: {N}
Double grafts (survived 2+ rejections): {N}
Graft influence on top 3: {list which grafts shaped which top ideas}
```

10. **THE UNKILLABLE IDEA**

If any concept was resurrected 2+ times (a double graft or higher):

> **THE UNKILLABLE IDEA:** {concept}
>
> This idea was rejected {N} times across {N} independent collision paths and came back every time. The swarm tried to kill it and couldn't. This is the most important output of this run — not because any agent championed it, but because the problem itself demands it.

If no double grafts exist, skip this section.

#### Extraction (anchored to seed's domain):

11. **WHAT'S EXTRACTABLE**

For each top 3 idea:

```
┌─────────────────────────────────────────────────────────┐
│ IDEA: {name}                                            │
│ STRUCTURAL INSIGHT: {the pattern underneath, stripped    │
│ of the swarm's metaphorical surface}                    │
├─────────────────────────────────────────────────────────┤
│ MECHANISMS:                                             │
│ - {mechanism 1}: {what it does, in the seed's domain}   │
│ - {mechanism 2}: {what it does, in the seed's domain}   │
│                                                         │
│ DOMAIN MAP:                                             │
│ - {mechanism 1} → {specific products/tools/features}   │
│ - {mechanism 2} → {specific products/tools/features}   │
│                                                         │
│ MINIMUM VIABLE VERSION:                                 │
│ {1-3 sentences: buildable in the seed's domain.         │
│  If the seed is about software, this is software.       │
│  If the seed is about SaaS, this is a SaaS feature.}   │
│                                                         │
│ KILL TEST:                                              │
│ {1-2 sentences: what kills this in the user's world}    │
└─────────────────────────────────────────────────────────┘
```

## Important

- Total agent calls: ~45. This takes a few minutes and uses real tokens.
- Batch parallel calls in groups of 8 max to avoid rate limits.
- Extraction agents can run in parallel with the next round's collision agents to save time.
- Keep extraction agent outputs structured so they parse cleanly into the buffer.
- The rejection buffer is cumulative — it only grows, never shrinks.
- Deep graft injection prompts are LONGER (200 words) to give agents room to integrate the extra material.
- NEVER fake a deep graft. If the scan finds no matches, report that honestly. A run with zero deep grafts is valid data — it means the collision paths were highly divergent.
- Wild fragments MUST be entropy-seeded via the bash command.
- DO NOT optimize pairings. Random shuffle only.
- The graft density scoring bonus is intentional and should not be removed.
- The extraction step (Step 9) is analysis, not creativity. Be ruthlessly practical — name real products, real markets, real failure modes.
