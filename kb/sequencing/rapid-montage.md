---
name: Rapid Montage (15 shots in 15s)
category: sequencing
tags: [montage, fast-pacing, day-in-life, routine, narrative-arc, hard-cuts]
tested: true
source: production-run
---

# Rapid Montage (15 shots in 15s)

Compress a narrative arc (morning routine, day-in-the-life, travel montage) into ~1s per shot.

## Key Traits

- ~1s per shot, hard cuts, zero transitions
- Mix of close-up, medium, wide, POV, fisheye angles
- Wardrobe changes between shots (each explicitly described)
- Narrative arc compressed (morning -> day -> night, or arrival -> explore -> departure)

## What Makes It Work

- `each change accompanied by a scene cut` + list all shots with timestamps
- Fisheye lens keyword (`central fisheye lens`) adds dynamic energy to wide shots
- The rapid pacing masks minor AI artifacts
- Final shot can be longer (2-3s) as a resolution beat

## Prompt Structure

```
[character anchor string]
0-1s: [shot 1 - wide establishing, specific wardrobe]
1-2s: [shot 2 - close-up detail]
2-3s: [shot 3 - POV action]
...
13-15s: [final shot - longer resolution beat]
Each change accompanied by a scene cut.
```
