---
name: Wardrobe Rules
category: continuity
tags: [clothing, outfit, consistency, multishot, override]
tested: true
source: production-run
---

# Wardrobe Rules

How Seedance handles clothing across generations and within multi-shot prompts.

## Rule 1: Override from Ref Image

Seedance preserves the ref image's clothing by default. To override:

- The wardrobe description must be **specific** in garment type, fit, material, and color
- Vague descriptions that overlap with the ref (e.g. "white shirt" when the ref wears a white top) will be ignored
- Visually distinct descriptions override successfully: "dark navy blazer over white top with tailored black trousers", "oversized beige knit sweater and black leggings"
- The more visually distinct from the ref, the easier the model understands the change
- No start frame needed for overrides

## Rule 2: Per Shot in Multishot Prompts

Every shot in a timeline-segmented prompt needs explicit clothing description, even if unchanged from previous shot.

- Seedance doesn't carry wardrobe context between timeline segments
- Without explicit description, it defaults to the ref image clothing or hallucinates
- Different outfits across shots within the same generation work when each is explicitly described
