---
name: Multishot Timeline Segmentation
category: sequencing
tags: [timeline, multi-shot, scene-cuts, structure, pacing]
tested: true
source: production-run
---

# Multishot Timeline Segmentation

Multiple distinct shots in one generation using explicit timestamps.

## Tested Range

- Up to 6 shots in 15s (our production runs)
- Up to 15 shots in 15s (external reference)

## Rules

1. Use `each change accompanied by a scene cut` to trigger cuts between shots
2. For continuous shots (no cut), keep them in a single timestamp range. Do NOT create a new timestamp if there's no cut
3. `One continuous shot, no scene cuts throughout` for segments that must flow without cutting
4. Every shot needs explicit wardrobe description, even if unchanged from previous shot (see wardrobe-rules)

## Gotcha

Complex instructions (@image2 references + continuous shot + scene cuts) in the same prompt can confuse the model. If a generation fails, simplify or split into separate clips.
