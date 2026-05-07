# Output Checklist (pre-emit gate)

Run this checklist before delivering any prompt to the operator. If any item fails AND the data supports a fix, regenerate. If a fix is impossible (e.g., the brief lacks a style anchor), flag it to the operator.

## Block-level structure

- [ ] **Four blocks present**: `[META]`, `[IMAGE REFERENCES / LEGEND]`, `[TIMELINE]`, `[STABILITY BOOSTERS]`. Block headers exactly as written.
- [ ] **No fifth block**. Sound design and scoring live inside META + TIMELINE, not in a separate block.

## META block

- [ ] Duration specified (4-15s). If brief needs >15s, plan chained gens.
- [ ] Aspect ratio specified.
- [ ] Audio on/off declared.
- [ ] Camera mode declared (one truth source: phone-amateur / cinema-camera-named / broadcast / security-cam / dashcam).
- [ ] Continuous-take vs cut-montage declared.
- [ ] Character count declared.
- [ ] One-line vibe present.

## LEGEND block

- [ ] Every uploaded ref has an entry in the legend.
- [ ] Each entry has ROLE / LOCK / START FRAME (yes/no). OVERRIDE only if ref differs from desired output.
- [ ] No ref is described twice (legend + timeline). Timeline calls by @token only.
- [ ] If wardrobe must differ from ref, the OVERRIDE field carries the affirmative description (NOT a negative in the timeline).
- [ ] If a ref is the start frame, its `START FRAME: yes` matches the API call's `first_frame_url`.

## TIMELINE block

- [ ] Subject + action in first 20-30 words of each beat (Universal Rule 1).
- [ ] Lighting described per shot or as scene-wide anchor (Universal Rule 2).
- [ ] Camera body name OR camera quality description (UGC) per shot (Universal Rule 3).
- [ ] Camera motion and subject motion in separate clauses (Universal Rule 4).
- [ ] No negative framing in timeline (Universal Rule 5).
- [ ] If multi-shot: hard cuts marked explicitly (`Hard scene cut at Xs exact.`) (Universal Rule 9).
- [ ] If single take: ends with `No scene cuts throughout, one continuous shot.` (Universal Rule 9).
- [ ] No re-description of legend-locked traits (Universal Rule 7).

## STABILITY block

- [ ] Timestamp discipline reinforced (`Strict {N}s on shot 1, {M}s on shot 2, {total}s total.`).
- [ ] Failure modes named literally (Universal Rule 10).
- [ ] Mode anchor present (one sentence describing overall feel).
- [ ] Wardrobe consistency declared if multi-shot.

## Style-specific checks

If the prompt uses a style file, also run that file's `## Pre-emit checks` section. Common style-specific items:
- **UGC**: physical distance in cm/m, foreground obstruction, no device names.
- **Cinematic-narrative**: camera body named, dialogue with emotion+tone tag.
- **Anime-character**: 5+ char refs uploaded, action language present.
- **Product-showcase**: multi-angle refs, beat-sync if music-driven.
- **Found-footage**: amateur camera operator declared, valid plausible witness.

## Word count gate (Universal Rule 1 HARD CAP)

This check is non-negotiable. Counting words before submit prevents paying for generations where the back half of the prompt is silently dropped.

- [ ] **Word count counted** (not estimated). Use a real count of META + LEGEND + TIMELINE + STABILITY combined.
- [ ] **Word count ≤ 400** (HARD CAP, regardless of clip duration).
- [ ] **If over 400**: STOP. Flag to operator with current word count vs the 400 cap. Do NOT submit. Refactor per Rule 1 levers, recount, then resubmit.

## Limits sanity

- [ ] Total duration <= 15s (single generation).
- [ ] <= 9 image refs.
- [ ] <= 3 video refs.
- [ ] <= 3 audio refs.
- [ ] <= 12 files total.
- [ ] <= 2 characters in same frame.
- [ ] Resolution per provider's published tier (commonly 480p / 720p / 1080p).

## Cost confirmation

- [ ] Operator confirmed cost estimate. Pricing varies by provider — check your provider's published rate per second × duration before firing.
- [ ] Operator approved iteration if regenerating an existing prompt.

## Submission gate

- [ ] All ref images uploaded to publicly reachable URLs (your provider must be able to fetch them).
- [ ] `reference_image_urls` (or provider equivalent) order matches LEGEND order.
- [ ] If any ref is start frame: `first_frame_url` (or equivalent) set, NOT in `reference_image_urls`.
- [ ] Output path specified.

If all pass, submit.
