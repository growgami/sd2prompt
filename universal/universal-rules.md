# Universal Rules

These 10 rules apply to EVERY Seedance 2.0 prompt regardless of style. If a rule is universal, it lives here. If it's style-specific (e.g., "no device names" for UGC), it lives in `styles/{style}.md`.

## Pre-flight: in-prompt multishot vs chained gens (decide BEFORE drafting)

Before writing one word, decide gen architecture. The choice changes everything downstream.

**One Seedance generation can hold a multi-shot timeline (cut-montage WITHIN one prompt) up to 15s.** The model handles cuts internally. Single API call. Single seed. Single style. Identity drift is bounded by the 8s coherence window.

**Chained gens** = N separate Seedance calls stitched together. Used when ANY of:
- Total duration >15s
- Different camera modes per shot (e.g. cinematic + UGC inserts) — Rule 8 forbids mixing within one gen
- Want to iterate shot-by-shot (regen one without touching others)
- Need different LEGEND refs per shot (e.g. shot 1 needs character + product A, shot 2 needs character + product B — and the 12-file cap pinches)

| Signal | Choose |
|---|---|
| ≤15s, one camera mode, same refs throughout | **In-prompt multishot** |
| >15s | **Chained gens** |
| Mode shift mid-sequence (cinematic→UGC) | **Chained gens** |
| Want to retry one shot cheaply | **Chained gens** |
| Refs would exceed 9 images / 12 files combined | **Chained gens** |

If chained: load `universal/chaining.md` for Pattern A (frame-chain) vs Pattern B (shared-reference) mechanics. Each chained gen still follows ALL 10 rules below independently.

If in-prompt multishot: continue with rules below. Rule 9 governs cuts within the one gen.

## Rule 1: Front-load critical info AND respect the 400-word HARD CAP

Subject + action appear in the first 20-30 words of any prompt block. After ~120-150 tokens, later clauses lose attention weight. The model deprioritizes after that. Never bury the most important instruction at the end.

**HARD CAP — 400 words total prompt budget, regardless of duration.** Across META + LEGEND + TIMELINE + STABILITY combined. The Seedance attention window cannot reliably absorb more — directives past ~400 words are silently dropped, even though the API accepts up to ~1000 words.

**If the prompt exceeds 400 words BEFORE submitting:**
1. STOP. Do not submit.
2. Flag to the operator: "this prompt is at {N} words vs the 400-word cap — must refactor before submit." Wait for operator's call to refactor or proceed-anyway.
3. Refactor levers (in priority order):
   - Drop redundant negatives in STABILITY that are already covered by affirmative LEGEND OVERRIDE
   - Compress per-shot descriptions: keep subject + camera + action + lighting; drop fluff adjectives
   - Move repeated environmental description (office, brick, factory windows) to LEGEND once instead of per-shot
   - Trim STABILITY ban-list to ONLY the failure modes you have evidence the model fights (not every theoretical failure)
   - Cut adverbs and qualifying clauses ("very", "completely", "absolutely")
4. Re-count after refactor. Only submit when at or below 400 words.

Failure mode to know about: a prompt that runs past the cap will see late-block STABILITY directives ("MOUTH FULLY CLOSED", "ALL FIVE SHOTS MUST RENDER", "no transition effects") partially or fully ignored because they sit past the attention window. You pay full price for output where the back half of the prompt has near-zero weight. Counting words BEFORE submission is the cheapest possible safeguard.

## Rule 2: Lighting first

Lighting has the single highest impact on output quality of any prompt element. If you can only add ONE element to improve quality, add a lighting description. Be specific: "cold gray late-morning light from camera-left through factory windows" beats "good lighting".

## Rule 3: Camera body, not "cinematic"

Name the camera. "Sony Venice", "ARRI Alexa", "Sony A7S3", "anamorphic lens", "iPhone 15 Pro selfie cam", "VHS camcorder", "Canon DSLR handheld". Seedance learned distinct visual characteristics during training; naming a body shifts output toward that aesthetic far more reliably than abstract style words. The word "cinematic" is empty noise.

For UGC styles, the camera is described by quality (`handheld, no stabilization, light sensor noise, slight wide-angle distortion`) rather than device name to avoid putting a literal phone in frame.

## Rule 4: Separate camera motion from subject motion

Two clauses, two subjects. Mixing them causes shaky output.

```
Bad:  "spinning shot of the dancer"
Good: "The dancer spins slowly. Camera holds fixed framing."
```

Always one sentence for camera, one sentence for what the subject is doing. The model resolves them as independent control channels when they're separate.

## Rule 5: Affirmative framing beats negative framing

Seedance reads the ref image first and the prompt second. Negatives ("no tie, no jacket") fight the ref's visual signal and usually lose. Wardrobe and identity overrides go in the LEGEND block as `OVERRIDE: {affirmative description}`. Only the STABILITY block uses negatives, and only for failure-mode fences ("no extra limbs", "no third character").

## Rule 6: Style anchor (director or aesthetic name)

Phrases like "Apple keynote style", "Wes Anderson symmetry", "90s anime style", "Sofia Coppola dreamy", "Christopher Doyle handheld" give the model a strong stylistic prior in one phrase. Most effective shortcut after lighting. Pair with camera body name (Rule 3) for compounding effect.

## Rule 7: Legend is a contract; never re-describe what the ref owns

Every @ref in the LEGEND block has ROLE / LOCK / (optional) OVERRIDE / START FRAME. The TIMELINE block calls refs by @token only. Re-describing the ref's traits in the timeline (face shape, wardrobe, build) does NOT reinforce them — it causes drift.

If the final video must differ from the ref (e.g., the ref shows a peacoat but you want a white shirt), the difference goes in the LEGEND `OVERRIDE` field, not as a negative in the timeline.

## Rule 8: One truth source per clip

Pick ONE camera mode per generation: phone-amateur, cinema-camera-named, broadcast, security-cam, dashcam. Mixing modes within a single clip causes the model to "average" them into a low-quality look. If the brief needs both (cinematic ad with broadcast inserts), use chained generations, not one prompt.

## Rule 9: Declare cut-montage vs continuous take explicitly

Silence here is the #1 cause of perspective breaks in multi-shot prompts.

- **Cut montage**: Use explicit timestamp ranges (`0-4s:`, `4-7s:`) AND insert `Hard scene cut at Xs exact. Clean cut, no transition effects.` between beats. Reinforce in STABILITY: `Strict {N}s per shot. Each change accompanied by a clean hard cut. No fade, no crossfade, no motion blur transition, no CapCut-style swipe — only instant cuts.`
- **Continuous take**: Keep all beats in one timestamp range. End the timeline with `No scene cuts throughout, one continuous shot.`

Don't mix. A multi-shot prompt without explicit cuts gets jump cuts in unexpected places.

Watch for this trap: the phrase `Quick motion blur from the cut` placed between beats can be interpreted by Seedance as a CapCut-style transition effect (motion-blur swipe), not as a clean hard cut. Default is INSTANT cut. If you want a transition effect, ask for it explicitly by name (whip pan, crossfade, etc.).

## Rule 10: Stability boosters block names failure modes literally

The fourth block of every prompt names failure modes by name, not politely.

```
Bad:  "Maintain consistent character throughout."
Good: "No third character entering frame. No extra limbs. Wardrobe consistent across shots: {description}. No drone angles. No cuts within shots."
```

The polite version reads as decoration; the literal version is a fence. The model treats explicit failure-mode language as a hard constraint.

## Pre-emit self-check

Before sending a prompt, verify:
- [ ] Pre-flight gen architecture decided (in-prompt multishot vs chained gens) — see top of file
- [ ] Subject + action in first 20-30 words of each timeline beat (Rule 1)
- [ ] Lighting described, not assumed (Rule 2)
- [ ] Camera mode declared (body name OR quality-only for UGC) (Rule 3)
- [ ] Camera and subject motion in separate clauses (Rule 4)
- [ ] No negative framing in timeline; overrides in LEGEND (Rule 5)
- [ ] Style anchor present (Rule 6)
- [ ] Every @ref has ROLE / LOCK / START FRAME in LEGEND; timeline calls by @token (Rule 7)
- [ ] Single camera mode throughout (Rule 8)
- [ ] Cuts declared explicitly OR continuous-take declared (Rule 9)
- [ ] Stability block names failure modes literally (Rule 10)

If any check fails, rewrite before submitting.
