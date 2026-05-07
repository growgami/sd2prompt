---
style: cinematic-narrative
applies_to: [film, trailer, music-video, drama, action-scene, narrative-short]
camera_mode: cinema-camera-named
kb_refs: [kb/sequencing/multishot-timeline.md, kb/continuity/wardrobe-rules.md]
---

# Style: Cinematic Narrative

Cinema-grade footage. Named camera body, named lens, controlled lighting, professional composition. Single-take or chained gens via `return_last_frame` for sequences >8s.

## When to load this style

- Film / trailer / music video / drama
- Action scene with character continuity
- Narrative short (story arc within 4-15s)
- Anything where the operator says "cinematic", "Hollywood", "Sony Venice", "anamorphic"
- Director-style anchor mentioned (Wes Anderson, Fincher, Villeneuve, Doyle, Coppola)

If the operator asks for "phone footage", "amateur", "selfie", "UGC" - this is NOT the right style. Load `ugc.md` instead.

## Style-specific overrides on top of universal rules

### Refine Rule 3 (camera body): name the body AND the lens

Cinematic prompts get a body + lens combination. Cite both:

```
"Sony Venice 2 with anamorphic lens"
"ARRI Alexa 35 with 35mm prime"
"RED V-RAPTOR with vintage spherical 50mm"
"35mm film grain on Kodak Portra 400 stock" (for film look)
```

These shift output toward the cited combination's signature. "Sony Venice" alone gives a digital cinema look; "Sony Venice anamorphic" adds the horizontal lens-flare and oval bokeh. Director-style anchors (Wes Anderson symmetry, Fincher cold-precision) compound the camera signal.

### Refine Rule 2 (lighting): controlled three-point or hard motivation

Cinematic lighting names the source AND the motivation:

```
Good: "Hard 4K LED key from camera-right, soft fill bouncing off white wall camera-left, hair light from above and behind"
Good: "Single window source camera-left, tungsten practical lamp foreground, deep shadows on the right"
Bad:  "good lighting", "dramatic light"
```

If the brief is naturalistic, name the natural source ("late-afternoon sun through venetian blinds, warm 4500K cast"). Cinematic lighting is intentional even when it looks accidental.

### Add Rule 11: 8-second coherence is the wall

Identity stability is strong within 5-8 seconds. Past 8s, character drift becomes noticeable. For sequences >8s, plan as chained gens, not one stretched prompt. See `universal/chaining.md` for the two patterns (frame-chain and shared-reference) and reliability data. Cinematic narrative typically uses Pattern A (frame-chain) for continuous-feeling sequences.

### Add Rule 12: Dialogue with emotion+tone tag before the line

For any spoken line:

```
Good: she softly whispers "Just looking at you."
Good: he says with quiet certainty: "I told you they'd come."
Good: she shouts in panic: "Get out, now!"
Bad:  "Just looking at you." (with no tag)
```

The tag goes BEFORE the quote and includes both:
- emotion (panic / certainty / despair / amusement)
- volume/tone (whispers / shouts / mumbles / says flatly)

Without both, Seedance picks a flat default delivery. The tag is non-negotiable.

### Add Rule 13: Continuous take is the default

Cinematic narrative typically wants a single continuous take per generation. Always end the timeline with:

```
"No scene cuts throughout, one continuous shot."
```

Multi-shot cuts within one cinematic gen are allowed but require the same explicit cut markers as UGC (`Hard scene cut at Xs exact.`). Default is continuous.

## Style anchors that work (Universal Rule 6 expansion)

Tested anchors for cinematic narrative:

| Anchor | Signature |
|---|---|
| "Wes Anderson symmetry" | Centered framing, pastel palette, deadpan delivery |
| "Fincher cold precision" | Low-key blue-green, controlled handheld, tight close-ups |
| "Villeneuve vast" | Wide vistas, geometric architecture, blue-amber palette |
| "Christopher Doyle handheld" | Saturated, intimate, organic camera drift |
| "Sofia Coppola dreamy" | Soft natural light, pastel, lingering wides |
| "Apple keynote style" | Clean white seamless, controlled studio lighting, slow push-in |
| "1970s film grain" | Halation, color shifts, organic grain texture |
| "Kodak Portra 400 stock" | Warm naturalistic skin tones, soft shadow rolloff |

Compound with body+lens for strongest effect: "Wes Anderson symmetry, ARRI Alexa with 35mm anamorphic, deadpan center framing".

## Pre-emit checks (cinematic-specific, in addition to universal)

- [ ] Camera body + lens named in META.
- [ ] Style anchor present (director name OR aesthetic name from table above).
- [ ] Lighting source + motivation named (not just "good lighting").
- [ ] If single-take: timeline ends with "No scene cuts throughout, one continuous shot."
- [ ] If multi-shot within one gen: each cut has explicit timestamp + "Hard scene cut at Xs exact."
- [ ] If sequence >8s: planned as chained gens via `return_last_frame`, NOT one stretched prompt.
- [ ] Dialogue lines have emotion+tone tag before the quote.

## KB references

- `kb/sequencing/multishot-timeline.md` - timing patterns within one gen
- `kb/continuity/wardrobe-rules.md` - wardrobe across generations (chained gens use copy-paste, not legend OVERRIDE)
- `kb/sequencing/rapid-montage.md` - fast-pacing for trailer/music-video work

## What's missing (TODO as evidence accumulates)

This style file is currently underspecified for:
- Color grade language (LUT names, grade direction)
- Lens flares and optical artifacts
- Action choreography ("Hong Kong style" vs "Bourne shaky" vs "John Wick gun fu")

Add these as patterns crystallize from production runs.
