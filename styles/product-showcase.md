---
style: product-showcase
applies_to: [product-reveal, 360-orbit, hero-shot, packaging, e-commerce, beat-sync-ad]
camera_mode: cinema-camera-named
kb_refs: [kb/continuity/wardrobe-rules.md]
---

# Style: Product Showcase

Product as the subject. Multi-angle character refs of the product. Camera does the work, product holds still or rotates on a controlled rig. Often beat-synced to music.

## When to load this style

- Product reveal / hero shot
- 360 orbit
- Packaging close-up
- E-commerce visual (Apple keynote, sneaker drop, watch, headphones, gadget)
- Beat-synced ad with product as recurring beat
- Texture / material showcase

If the brief is "person USING the product" with the person as primary subject, load `cinematic-narrative.md` or `ugc.md` instead and tag the product as a secondary @ref.

## Style-specific overrides on top of universal rules

### Refine Rule 7 (legend): product gets multi-angle refs

Product consistency requires more refs than character consistency. Upload 3-5 refs:

```
@image1 = product, front view. ROLE: primary identity, hero angle. LOCK: shape, color, branding, materials. START FRAME: yes (or no if first frame is a wider scene).
@image2 = product, side view. ROLE: silhouette and proportions. LOCK: profile shape, depth.
@image3 = product, surface/texture detail. ROLE: material and finish. LOCK: matte/gloss, grain, color accuracy.
@image4 = product, packaging or backside (optional). ROLE: completeness for 360 reveal.
```

The legend explicitly assigns ANGLE roles. Don't just upload "5 photos of the product" - the model needs to know which is front/side/surface/back.

### Refine Rule 4 (camera vs subject motion): product holds, camera moves

For product showcase, the product is usually static or rotates on a fixed axis. The camera does the work. Always:

```
"Product holds fixed orientation on a black turntable. Camera orbits clockwise at moderate speed."
"Product floats centered in frame, motionless. Camera pushes in slowly toward the {detail}."
"Product spins on a vertical axis. Camera holds at eye-level, fixed framing."
```

If the product moves AND the camera moves, declare both motions explicitly in separate sentences.

### Add Rule 11: Beat sync requires a video or audio reference

Beat-synced product ads need a reference clip with the music's rhythm. Pass it as `@video1` or `@audio1`:

```
[META]
duration: 8s, audio on, beat-synced to @audio1.

[LEGEND]
@audio1 = music track. ROLE: rhythm anchor for cuts and product beats. LOCK: timing of beat-drops to scene-cut moments.

[TIMELINE]
0-2s: product hero hold...
At every beat-drop in @audio1: hard scene cut + new product angle.
```

Important: beat-sync and dialogue are mutually exclusive. Pick one.

### Add Rule 12: Keyword-triggered effects useful for product

Specific phrases trigger specialized model behaviors:

| Effect | Keyword |
|---|---|
| 360 orbit | `robotic arm multi-angle circular movement` |
| Push-in reveal | `slow push-in tracking shot toward {product}` |
| Particle scatter (luxury feel) | `golden sand particles scatter`, `particle dispersion effect` |
| Liquid metal transform | `gradually transforms into liquid metal form` |
| Holographic | `holographic portal appears with swirling energy` |
| Each beat = cut | `each change accompanied by a scene cut` |

Use sparingly. One keyword effect per generation.

## Pre-emit checks (product-specific, in addition to universal)

- [ ] 3+ multi-angle product refs with explicit ANGLE roles in legend.
- [ ] Camera vs product motion declared in separate sentences (Universal Rule 4).
- [ ] Lighting names source AND color temperature (Universal Rule 2 refined: "single 5600K key from camera-right, soft white seamless backdrop").
- [ ] If beat-synced: `@video1` or `@audio1` reference uploaded; "each change accompanied by a scene cut" in stability.
- [ ] If 360 orbit: "robotic arm multi-angle circular movement" keyword in timeline.
- [ ] Brand name / logo: bake into the @image1 ref. Never prompt typography.
- [ ] No human characters in frame unless brief explicitly requires them.

## KB references

- `kb/continuity/wardrobe-rules.md` - if a person is in shot, wardrobe applies
- `kb/sequencing/multishot-timeline.md` - timing for multi-angle reveals

## What's missing (TODO)

- Specific lighting setups by product category (jewelry vs apparel vs gadgets)
- Tested keyword combos for luxury, sport, tech
- E-commerce platform-specific aspect ratios and durations
