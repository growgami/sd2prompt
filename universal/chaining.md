# Chaining and Multi-Generation Workflows

When one Seedance generation isn't enough — duration >15s, mode shifts between shots, or you want to iterate shot-by-shot — you chain multiple generations. This applies to ANY style (UGC, cinematic, product, anime, found-footage). It is NOT cinematic-specific.

## When to chain (vs in-prompt multishot)

| | In-prompt multishot | Chained gens |
|---|---|---|
| Generations | 1 API call | N API calls |
| Total duration budget | 4-15s | 15s × N (reliable through 2-3 extensions) |
| Cost | rate × duration | rate × Σ duration |
| Continuity mechanism | model handles within 1 gen | last-frame, shared-ref image, OR prior video as @video1 |
| Mixing camera modes | NO (one truth source per gen) | YES (each gen can be different style) |
| Iteration | regen the whole prompt | regen only the failed gen |
| Identity drift | within 8s coherence | grows per extension; degrades at 4+ |
| Best for | tight 4-15s sequences in one mode | >15s, mode shifts, or shot-by-shot iteration |

Pick in-prompt multishot when the brief fits in 15s AND uses one camera mode. Pick chained gens when either condition fails.

## Three chaining patterns

There are three mechanically distinct ways to chain. Pick the one that matches your continuity need.

| Pattern | Anchor passed to gen N+1 | Continuity strength | Best for |
|---|---|---|---|
| **A. Frame-chain** | last frame of gen N as `first_frame_url` | strict pixel match at the seam, but only 1 frame of motion context | "moment continues" feel; sequences that read as one take |
| **B. Shared-reference** | same image ref(s) across all gens (no temporal link) | shared character / wardrobe / world; independent beats | b-roll for VO; montage cut together with hard cuts in post |
| **C. Video-extend** | full mp4 of gen N as `@video1` reference | motion + context + lighting + character carried across the seam | true 30s+ continuous-feeling sequences |

### Pattern A: Strict frame-chain (single-frame anchor)

The last frame of gen N becomes the first frame of gen N+1. Used when the sequence reads as ONE continuous moment that just happens to be split for technical reasons.

API mechanic:
1. Submit gen N with `return_last_frame: true` (or your provider's equivalent). Response includes a downloadable last-frame URL.
2. Pass that URL as `first_frame_url` in gen N+1. The legend marks that ref as `START FRAME: yes`.
3. Repeat for gen N+2.

Or, if gen N was already submitted without `return_last_frame: true` and you have the mp4:
- Extract the last frame with ffmpeg: `ffmpeg -sseof -0.04 -i in.mp4 -vframes 1 -q:v 2 last_frame.png`
- Upload to a public host of your choice.
- Use the returned URL as `first_frame_url` in gen N+1.

For frame-chain, copy-paste the EXACT character description across all gens. This is the ONE place where re-describing identity helps — each gen is a separate generation, not one prompt, so the "don't re-describe legend-locked traits" rule (Universal Rule 7) applies WITHIN a single prompt, not across chained gens.

**Trade-off:** a single still frame can't carry motion direction, lighting evolution, or wardrobe-fabric physics. Expect a small visual hiccup at the seam where motion appears to "restart" from a static pose. Pattern C avoids this.

### Pattern B: Shared-reference (separate beats, same world)

Each gen pulls from the SAME ref image (or images) but is otherwise independent. Used for b-roll sequences, montages, or any case where the beats don't need to read as one continuous moment but DO need to share a character + wardrobe + environment.

Mechanic:
1. Pick a reference image that locks character, wardrobe, and environment (often the first frame of an earlier gen, generated via an image model, or a real photo).
2. Pass the ref as `reference_image_urls[0]` in EACH gen. NOT as `first_frame_url`. The legend marks that ref as `START FRAME: no` and uses LOCK fields to bind identity, wardrobe, location.
3. Each gen has its own timeline — different beats, different actions, different camera moves. The shared ref keeps the world consistent.

Shared-reference is useful when:
- You're doing b-roll for narration that overlays later
- You want the same character in different micro-scenes that don't need to flow continuously
- Each gen will be edited together with hard cuts in post

For shared-reference, the LEGEND OVERRIDE field carries wardrobe consistency. Copy the same legend block into every gen (same ROLE / LOCK / OVERRIDE), even though the timeline differs.

### Pattern C: Video-extend (prior gen as @video1)

The previous generation's full mp4 is passed back as a video reference (`reference_video_urls[0]`, addressed in the prompt as `@Video1`). Seedance treats the prior clip as a motion + context + lighting anchor and continues from where it left off, for up to another 15s. Repeatable: gen 2's mp4 becomes gen 3's `@video1`, and so on.

This is the highest-fidelity chain. The seam reads as one continuous shot far more reliably than Pattern A, because the model has motion vectors, color, lighting, and character context to extrapolate from — not just a single frame.

API mechanic:
1. Submit gen 1 normally (any reference setup that fits the brief).
2. Download gen 1's mp4 immediately (output URLs commonly expire ~24h).
3. Upload gen 1's mp4 to a public host your provider can fetch.
4. Submit gen 2 with `reference_video_urls=[gen1.mp4]` plus any image refs you still need (character sheet, scene, etc.). Reference the video as `@Video1` in the prompt body and the images as `@Image1/2/3`.
5. Repeat: gen 2 → upload → `@video1` for gen 3.

Multimodal mode accepts `video_refs` and `image_refs` simultaneously, so you can keep your character/scene refs while also passing the prior clip. Mind the 12-file total cap and the 3-video-ref cap.

**Recommended prompt structure for gen 2+ (video-extend gens):**

Add an explicit `EXTEND CONTINUITY` block to the prompt that locks the visual contract to `@Video1`:

```
EXTEND CONTINUITY (match @Video1):
- Exposure, contrast, white balance: identical to @Video1.
- Shadow density and highlight rolloff: identical to @Video1.
- Time of day and light direction: identical to @Video1.
- Character wardrobe, hair, skin tone: identical to @Video1.
- Color grade: identical to @Video1.
Continue motion smoothly from the final frame of @Video1.
```

Without this block, Seedance applies slightly different color decisions per gen, producing visible contrast jumps at the seam.

**Trade-offs:**

- **Audio drift**: Seedance generates independent audio per gen, producing a "music restart" feel at the seam. Default workflow: strip audio entirely (`ffmpeg -an`) and add music in post.
- **Color drift residual**: even with the EXTEND CONTINUITY block, small contrast jumps can survive. Mitigations in order of strength: (1) the EXTEND CONTINUITY block in prompt; (2) unified ffmpeg color grade pass on the merged file; (3) per-clip luminance match using ffmpeg `signalstats` on seam frames + manual `eq` filter on gen 2 before concat.
- **File budget**: video refs count against the 3-video-ref cap and the 12-file total cap. If you're also passing image refs (character + scene + content), keep the count within budget.
- **Cost compounds**: video-extend gens are full-price gens, just like fresh ones. A 30s sequence is two full 15s gens.

When the prior-clip continuity carries cleanly, hard concat (no crossfade) at the seam is sufficient. The video reference does the work that a manual transition would otherwise hide.

## Reliability and degradation

- **2-3 extensions** (any pattern): reliable. Total ~30-45s if each gen is 10-15s.
- **4+ extensions**: degradation visible. Identity drifts, color shifts, costume morphs. Pattern C degrades slowest; Pattern A degrades fastest because each link in the chain has only 1 frame of context.
- **Per-gen sweet spot**:
  - Pattern A / B: 7-8s. Shorter gens drift less but cost more per second of final output.
  - Pattern C: 10-15s. The video ref carries enough context that you can run gens at full duration without much extra drift.

## Cost math

```
N gens × duration_per_gen × rate_per_second = total cost
```

Use your provider's published per-second rate. Always confirm the total cost with the operator before starting a chain. A failed mid-chain gen costs the full per-gen amount to retry, even if everything else worked.

## Iteration discipline

When chained gens fail:
1. Identify which gen failed (visual inspection).
2. Regenerate ONLY that gen, not the whole chain.
3. If the failed gen was using a frame-anchor or video-anchor from the previous gen, the input is stable — change ONLY the prompt or refs, not both.
4. If multiple gens fail in a row, suspect the upstream anchor quality. For Pattern A: regenerate the still ref via your image model with better LOCK direction. For Pattern C: regenerate the prior gen if its ending frames are visually unstable.

## Worked sketches

### Sketch 1 — confessional close-up + b-roll inserts (Pattern B)

Premise: a 4s confessional close-up (gen 1) followed by 3 b-roll shots of the same character in the same office (gen 2-4). Total ~12-15s after edit.

- **Pattern B (shared-reference)** is the right move. The b-roll shots don't need to flow continuously from gen 1; they share the character's wardrobe and the office environment. Narration will be overlaid later.
- Extract the FIRST frame of gen 1 (where the character is in the confessional close-up with the office visible behind). That frame locks: face, wardrobe (e.g. white dress shirt no tie no jacket), environment (e.g. exposed brick + factory windows + glass meeting room office).
- Upload to a public URL. Reference it in EACH subsequent gen's legend with `LOCK: identity (face, build, glasses), wardrobe ({affirmative description}), environment ({location description}).` and `START FRAME: no`.
- Each b-roll gen gets its own timeline (typing on laptop, looking at phone, walking past glass meeting room, etc.). The shared ref keeps the character + office stable across gens.

### Sketch 2 — 30s continuous flow sequence (Pattern C)

Premise: a 30s yoga flow with the same character on the same mat, same time of day, no scene cuts. Two gens of 15s.

- **Pattern C (video-extend)** is the right move. The brief reads as one continuous shot; the seam should be invisible.
- Submit gen 1 normally (character ref + scene ref + pose-sequence ref, 15s, full duration).
- Download gen 1's mp4. Upload it to a public host.
- Submit gen 2 with `reference_video_urls=[gen1.mp4]` + same character ref + same scene ref. Add the `EXTEND CONTINUITY` block to the prompt locking exposure / color / wardrobe / time-of-day to `@Video1`. Use `@Image1/2/3` for the still refs.
- Strip audio with `ffmpeg -an` on both clips before concat. Hard concat (no crossfade). Add music in post.

## Pre-emit checks (chaining-specific)

- [ ] Pattern declared: frame-chain (A), shared-reference (B), or video-extend (C).
- [ ] If Pattern A: `first_frame_url` set in API call, legend marks the ref as `START FRAME: yes`, character description copy-pasted across gens.
- [ ] If Pattern B: shared ref passed as `reference_image_urls[0]` in EVERY gen, legend marks `START FRAME: no` with explicit LOCK fields for identity + wardrobe + environment.
- [ ] If Pattern C: prior gen's mp4 uploaded and passed as `reference_video_urls[0]`, prompt addresses it as `@Video1`, EXTEND CONTINUITY block included, audio stripped before concat.
- [ ] File budget under 12 total (Pattern C: video ref + image refs combined).
- [ ] Total cost confirmed with operator (N × per-gen cost).
- [ ] Plan caps at 3 gens unless you've validated 4+ chains for this specific style.
- [ ] If wardrobe/environment must stay consistent: every gen's LEGEND has the same OVERRIDE/LOCK block, verbatim.
