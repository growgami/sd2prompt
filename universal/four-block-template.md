# Four-Block Prompt Template (canonical)

Every Seedance 2.0 prompt uses this structure. The four blocks are not optional.

```
[META]
{duration, aspect, audio on/off, camera mode (one truth source), continuous-take vs cut-montage, character count, one-line vibe}

[IMAGE REFERENCES / LEGEND]
@image1 = {identity or content}. ROLE: {what it governs}. LOCK: {traits to preserve}. OVERRIDE: {only if differs from ref}. START FRAME: yes/no.
@image2 = {same shape}.
@imageN = {same shape}.

[TIMELINE]
0-Xs: {shot beat. Reference characters/content by @token only. Do NOT re-describe what the legend already locks. Camera move + subject move in separate clauses. Lighting + camera body name.}
X-Ys: {next beat. If a hard cut, mark it: "Hard scene cut at Xs exact." If continuous, no new timestamp.}

[STABILITY BOOSTERS]
{Timestamp discipline reinforcement. Failure modes named literally: "no extra limbs", "no third character entering frame", "no drone angles", "stable hand anatomy", "wardrobe consistent across shots: {description}". Mode anchor: "Heavy handheld amateur feel throughout" / "Cinematic single-take throughout" / etc.}
```

## Block-by-block intent

### `[META]` — set the contract
- **duration**: 4-15s. Seedance enforces this; longer needs chained gens via `return_last_frame`.
- **aspect**: 9:16 / 16:9 / 1:1 / 4:3 / 3:4 / 21:9.
- **audio**: true (native audio) or false (silent).
- **camera mode (one truth source)**: phone-amateur / cinema-camera-named / broadcast / security-cam / dashcam. Pick ONE. Do not mix.
- **continuous-take vs cut-montage**: declare explicitly. Silence here = the model invents perspective breaks.
- **character count**: 1, 2, or "single character per shot". Above 2 in the same frame causes identity confusion.
- **vibe**: one line that anchors tone (confessional, frenetic, cinematic-quiet, etc.).

### `[IMAGE REFERENCES / LEGEND]` — the binding contract
The legend is a contract. Every @ref gets four fields:

| Field | Purpose |
|---|---|
| **ROLE** | What this ref governs in the final video. Identity? Scene? Content on a screen? First frame? |
| **LOCK** | Traits the ref must preserve. Face, hair, glasses, build, brand colors, UI text. |
| **OVERRIDE** | Only when the final video must DIFFER from the ref (e.g., wardrobe). State the override. If no override, omit the field. |
| **START FRAME** | yes/no. If yes, this ref is `first_frame_url` in the API call. Otherwise it's a `reference_image_url`. |

**Reinforcement of Universal Rule 7:** never re-describe in the timeline what the legend already locks. Re-describing the same trait in both places causes drift, not reinforcement.

### `[TIMELINE]` — the beats
- One block per shot. Timestamp at the start: `0-4s:`, `4-7s:`.
- Reference characters and content by @token only.
- Camera motion and subject motion in **separate clauses**. Mixing them causes shake.
- Front-load: subject + action in the first 20-30 words of each beat.
- For hard cuts: insert one line between beats: `Hard scene cut at Xs exact. {Optional: motion blur description.}`
- For continuous take: keep beats in one timestamp range OR end with `No scene cuts throughout, one continuous shot.`

### `[STABILITY BOOSTERS]` — anti-drift fence
- Reinforce timing: `Strict 4s on shot 1, 3s on shot 2, 7s total.`
- Name failure modes literally. Don't be polite. "no extra limbs", "no third character entering frame", "no additional handheld objects in his grip", "no drone angles", "no cuts within shots", "wardrobe consistent across shots: {description}".
- End with the mode anchor: one sentence describing the overall feel ("Heavy handheld amateur feel throughout" / "Cinematic Sony Venice anamorphic single-take" / etc.).

## Why four blocks (not three, not five)

- META alone = no character/content binding.
- LEGEND alone = no scene direction.
- TIMELINE alone = drift, no anchor on identity, no safety net.
- STABILITY alone = empty rules without context.

Removing any block weakens the rest. Adding more blocks (sound design, scoring) is fine but lives INSIDE the existing four (sound goes in META + TIMELINE).

## Worked example (UGC two-shot)

See `styles/ugc.md` for a fully-worked two-shot UGC prompt that follows this template end-to-end.
