---
description: "Generate entropy seeds for wild fragment injection (internal helper)"
---

# Entropy Seed Generator

This is an internal reference used by the swarm command to generate maximally random wild fragments.

## The Problem

LLMs have favorite metaphors (mycelium, cathedrals, mirrors). When asked to "pick something random," they gravitate to the same concepts. Wild fragments must be seeded with external entropy across multiple axes to break this bias completely.

## Method

Each wild fragment is generated from a **cross-domain collision** with 5 randomized axes:

1. **Modifier** — constrains the angle ("a failure in", "the forbidden practice of", "the sound of", etc.)
2. **Domain A × Domain B** — two unrelated fields forced together (100+ domains, picked in pairs)
3. **Place** — geographic anchor from 40 cities across 6 continents
4. **Year** — temporal anchor (800-2024)

This produces entropy seeds like: `the forbidden practice of bell casting × bat echolocation jamming | Potosí | 1347`

The combinatorial space is enormous: 25 modifiers × 100+ domains² × 40 places × 1224 years. No two runs will ever produce the same seed.

## Entropy Source

The script uses system entropy (`os.urandom` / `crypto.randomInt`) rather than pseudo-random defaults, ensuring seeds aren't reproducible even with identical initial conditions.

## Generation Script

Run this bash command to produce 6 cross-domain entropy seeds:

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

Each output line is a **cross-domain entropy seed**. Use it to force the wild fragment generation prompt:

```
Generate a single provocative, hyper-specific wild fragment (one sentence) inspired by this entropy seed: "{entropy_seed}"

The fragment must collide BOTH domains through the modifier's lens, anchored in the specific place and era. It must be a concrete, surprising fact or metaphor — not abstract. Include a specific number, name, or measurement. Do NOT use concepts from the user's seed text. Do NOT mention AI, technology, mycelium, cathedrals, or mirrors. The weirder and more specific, the better.
```

This ensures every run produces genuinely different wild DNA. The combinatorial space (25 modifiers × 100+ domains² × 40 places × 1224 years) makes repetition effectively impossible.
