---
description: "ACRE-8 swarm — Deep swarm + rejection tracking. Finds ideas AI can't kill."
arguments:
  - name: seed
    description: "Seed text - a topic, problem, or raw brain dump to explore"
    required: true
---

# Blind Swarm — ACRE-8 Mode

**Adversarial Concept Resurrection Engine**

You are orchestrating a deep blind swarm with ACRE-8: a rejection buffer that tracks killed ideas, detects when they independently resurface, and flags them as **deep grafts** — ideas the system can't get rid of.

The premise: if an idea gets rejected in one collision but independently re-emerges in a completely separate collision path, it's not random. It's load-bearing. The problem *needs* it and the system can't route around it.

Total agent calls: ~45 (31 swarm + ~14 ACRE-8 extraction/scanning)

## Rules

All standard deep swarm rules apply, PLUS:

6. **Rejection is data, not deletion.** Every collision kills ideas. ACRE-8 captures what dies.
7. **Resurrection is signal.** An idea that comes back uninvited is more important than one that was kept.
8. **Deep grafts are sacred.** They get re-injected with a tag. They cannot be killed twice.
9. **You track lineage obsessively.** Every rejected concept gets a source ID and round-of-death.

## Lens Pools (same as deep mode)

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

Split it into **12 seed fragments** + **4 wild fragments** (16 total). Same as deep mode.

#### Wild Fragment Generation (ENTROPY-SEEDED)

**Do NOT invent wild fragments from your own imagination.** Use entropy seeds:

```bash
python3 -c "
import random
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
for p, y in zip(picks, years):
    print(f'{p} | {y}')
" 2>/dev/null || node -e "
const d=['kitchen tools','marine biology','medieval trades','particle physics','folk instruments','extinct animals','surgical procedures','board games','textile techniques','volcanic phenomena','postal systems','fermentation','cartography terms','circus arts','mining equipment','dance forms','architectural failures','weather anomalies','prison slang','perfumery','radio frequencies','knot types','gambling mathematics','sleep disorders','typeface anatomy','beekeeping crises','elevator mechanics','tea ceremonies','cryptographic attacks','bird migration errors','paint chemistry','debt instruments','tidal patterns','competitive eating','bell casting','moss ecology','freight logistics','optical illusions','shoe construction','sourdough failures','bridge collapses','insect mimicry','calendar reform','tattoo removal','satellite decay','cheese aging','wildfire behavior','fountain pen inks','submarine acoustics','seed dormancy','roller coaster physics','language death'];
const s=d.sort(()=>Math.random()-.5).slice(0,4);
s.forEach(p=>console.log(p+' | '+(800+Math.floor(Math.random()*1224))));
"
```

For each entropy seed, spawn a subagent:

```
Generate a single provocative, hyper-specific wild fragment (one sentence) inspired by: "{entropy_seed}"

The fragment must be a concrete, surprising fact or metaphor from this domain. Include a specific number, name, or place. Do NOT use concepts from the user's seed text. Do NOT mention AI, technology, mycelium, cathedrals, or mirrors.
```

Label wild fragments F13-F16 and mark as WILD.

### Step 2: Initialize the ACRE-8 Buffer

Create an internal tracking structure. This is conceptual — maintain it as running state throughout the swarm:

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

Same as deep mode. Assign each fragment a random lens from **Pool A**. Run all 16 agents in parallel (batch in groups of 8).

Collect outputs. Label A1 through A16.

### Step 4: Round 1 — First collision (16 → 8) + ACRE-8 EXTRACTION

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

#### 4b: ACRE-8 Extraction (NEW)

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

### Step 5: Round 2 — Second collision (8 → 4) + ACRE-8 SCAN + EXTRACTION

#### 5a: Collision

Randomly pair 8 outputs into 4 pairs. Lenses from **Pool C**. Standard collision prompt. **Run 4 agents in parallel.** Collect C1-C4.

#### 5b: ACRE-8 Resurrection Scan (NEW)

This is the critical step. For each of the 4 new outputs (C1-C4), spawn a scan agent:

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

#### 5c: ACRE-8 Extraction

Same as Step 4b but for the 4 Round 2 collisions. **Run 4 extraction agents in parallel.**

Add new killed concepts to REJECTION_BUFFER.

### Step 6: Round 3 — Third collision (4 → 2) + DEEP GRAFT INJECTION

#### 6a: Check for deep grafts

If DEEP_GRAFTS is non-empty, modify the collision prompt for this round:

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

#### 6b: ACRE-8 Scan + Extraction

Run scan against full rejection buffer. Run extraction on the 2 collisions.

Any NEW resurrections found here are **double grafts** — ideas that survived two rounds of rejection. Flag them specially.

### Step 7: Round 4 — Final collision (2 → 1) + DEEP GRAFT INJECTION

Same deep graft injection as Round 3, including any new grafts found in Round 3.

Final collision produces E1.

Run one last ACRE-8 scan on E1 against the full buffer.

### Step 8: Score and present

Score ALL semi-final and final outputs (C1-C4, D1, D2, E1) on 1-10:

- **Novelty**: Would this surprise someone who's been thinking about this topic for years?
- **Coherence**: Does it hold together internally?
- **Generativity**: Does it open doors to more ideas?
- **Weirdness**: How far from the obvious is this?
- **Graft Density**: How many deep grafts influenced this output? (ACRE-8 metric)

Ranking formula: **(Novelty x Generativity) + Weirdness bonus + (Graft Density x 5)**

The graft density bonus is intentional. Ideas shaped by deep grafts have been tested by rejection and survived. They earn extra weight.

### Step 9: Present results — ACRE-8 ENHANCED

Show the user everything from deep mode, PLUS:

#### Standard deep mode output:
1. **Top 3 ideas** ranked by score
2. **The wildest idea** regardless of coherence
3. **Collision paths** for each top idea
4. **Scores** for all surfaced ideas
5. **Wild fragment influence map**
6. **Key emergent themes**

#### ACRE-8 additions:

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

The full rejection buffer, organized by round. These are the ideas that died and STAYED dead — they tell you what the problem space doesn't need.

```
┌────────────┬──────────────────────────┬───────────┬──────────────┐
│ ID         │ Concept                  │ Killed in │ Resurrected? │
├────────────┼──────────────────────────┼───────────┼──────────────┤
│ REJ-1-01   │ ...                      │ Round 1   │ No           │
│ REJ-1-02   │ ...                      │ Round 1   │ → GRAFT-1   │
│ ...        │                          │           │              │
└────────────┴──────────────────────────┴───────────┴──────────────┘
```

9. **ACRE-8 SUMMARY STATISTICS**

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

If any concept was resurrected 2+ times (a double graft or higher), give it a special section:

> **THE UNKILLABLE IDEA:** {concept}
>
> This idea was rejected {N} times across {N} independent collision paths and came back every time. The swarm tried to kill it and couldn't. This is the most important output of this run — not because any agent championed it, but because the problem itself demands it.

If no double grafts exist, skip this section.

## Important

- Total agent calls: ~45 (31 swarm + ~14 ACRE-8). This is the most expensive mode. Warn the user.
- ACRE-8 extraction agents can run in parallel with the next round's collision agents to save time.
- Keep extraction agent outputs structured so they parse cleanly into the buffer.
- The rejection buffer is cumulative — it only grows, never shrinks.
- Deep graft injection prompts are LONGER (200 words) to give agents room to integrate the extra material.
- NEVER fake a deep graft. If the scan finds no matches, report that honestly. A run with zero deep grafts is valid data — it means the collision paths were highly divergent.
- Wild fragments MUST be entropy-seeded via the bash command. Same as deep mode.
- DO NOT optimize pairings. Random shuffle only.
- The graft density scoring bonus is intentional and should not be removed. Ideas tested by rejection have earned their weight.
