---
style: anime-character
applies_to: [anime, manga-to-anime, action-anime, super-saiyan, character-consistency-heavy]
camera_mode: cinema-camera-named
kb_refs: []
---

# Style: Anime Character

Anime / manga-to-anime workflows. Heavy character consistency requirements. Action language ("super saiyan", "transformation sequence", "speed lines"). Often paired with platform-level wrappers that have Seedance 2 under the hood with custom anime configuration.

## When to load this style

- Anime-style character animation
- Manga-to-anime conversion (still panels into motion)
- Action sequences with transformations
- Character emotion close-ups in anime style
- 90s anime / 80s anime / modern anime style anchors

If the brief is photoreal, load `cinematic-narrative.md`. If the brief is product-led with anime aesthetic as decoration, load `product-showcase.md`.

## Style-specific overrides on top of universal rules

### Refine Rule 7 (legend): character consistency needs MANY refs

Anime characters drift faster than photoreal because the model has more room to interpret stylized features. Established practitioners commonly upload ~20 references per main character ("each element separately") to constrain the look.

Minimum: 5 refs per main character. Recommended: 10-20 if budget allows (max 9 per generation, so split across chained gens).

Legend pattern:
```
@image1 = Character A, front-facing portrait. ROLE: face/hair/eyes lock. LOCK: hair color, eye shape, distinguishing marks.
@image2 = Character A, three-quarter angle. ROLE: face geometry from a different angle.
@image3 = Character A, full body. ROLE: build, costume, proportions.
@image4 = Character A, action pose. ROLE: motion language and silhouette.
@image5 = Character A, expression detail (intense/calm/angry). ROLE: emotional face range.
```

Pre-generate the character sheet via your image model of choice BEFORE any video generation. Each ref is a curated still, not a casual photo.

### Refine Rule 6 (style anchor): name the era/sub-style

Anime is a wide bucket. Be specific:

```
"90s anime style"   → cell-shaded, bold outlines, vivid colors
"80s anime style"   → softer palette, hand-drawn feel, more film grain
"modern anime style" → digital coloring, smoother gradients, sakuga moments
"Studio Ghibli style" → painterly backgrounds, gentle motion
"shonen action style" → speed lines, dynamic angles, impact frames
"shoujo style"     → soft lighting, sparkles, flowing hair detail
"seinen drama style" → muted palette, realistic proportions, heavy shadows
```

Compound with action language: "90s anime style, shonen action, super saiyan transformation sequence".

### Add Rule 11: Action keywords that trigger anime motion

Specific phrases that Seedance recognizes for anime action:

| Trigger | Effect |
|---|---|
| `super saiyan transformation` | Energy aura, hair levitating, color shift, screen shake |
| `speed lines from {direction}` | Radial motion blur lines from a focal point |
| `impact frame` | Frozen high-contrast pose at moment of impact |
| `power-up sequence` | Character holds pose, energy gathers, camera slow zoom |
| `transformation sequence` | Multi-frame morph, color burst, pose change |
| `manga panel motion` | Static panel slowly animates with parallax |
| `sakuga moment` | Brief burst of fluid high-frame-rate animation |

One trigger per generation. Stacking causes confusion.

### Add Rule 12: Platform wrappers may apply a custom Seedance config

If the operator is generating via a platform wrapper (some hosted UIs ship with anime-tuned wrappers around Seedance 2), the same prompt may behave differently than against the raw model. Note in production logs which platform was used so you can compare outputs.

## Pre-emit checks (anime-specific, in addition to universal)

- [ ] 5+ character refs uploaded with explicit ROLE per ref (face / body / pose / expression).
- [ ] Era anchor present ("90s anime style" / "modern anime style" / etc.).
- [ ] Action keyword if action sequence (one only).
- [ ] LOCK fields name distinguishing features (hair color, eye color, costume detail, scar, accessory).
- [ ] If transformation: declare it as a single timeline beat, not split across cuts.

## What's missing (TODO)

- Tested results for chained anime gens via return_last_frame
- Specific keyword combos that work for fight choreography vs slice-of-life
- Background style anchors (Madhouse vs Mappa vs Wit Studio)
- Voice direction for Japanese-language vs English-dub feel
