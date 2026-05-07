---
name: Boomer Trap / Amateur Phone Capture
category: found-footage
tags: [T2V, amateur, phone-camera, handheld, camera-chaos, accidental, micro-shake, found-footage, realism, one-continuous-shot]
tested: true
confidence: high
source: community
source_url: https://x.com/AIWarper/status/2042803430297866314
source_author: AIWarper
---

# Boomer Trap / Amateur Phone Capture

Simulates raw, unedited phone footage where someone accidentally captures something unexpected. The person filming IS the camera operator. They may be alone, just pointing their phone at something mundane when something extraordinary happens. No second person required in frame. The camera behaves like a real phone: micro-shake, reactive jerks, chaotic framing. So convincing that viewers (especially older audiences) believe it's real footage. T2V only, no references needed.

Pattern publicly documented by AIWarper (link above). The template below is paraphrased from that thread; refer to the source for the original example.

## When to Use

- You want AI video that passes as real, unplanned footage
- Someone films something ordinary, then something unexpected enters/happens
- No "subjects" needed. The camera operator + the event is enough
- The "realism" of the camera behavior IS the hook (not cinematic quality)
- Variations: filming the sky and a UFO appears, filming rain and it's spiders, filming a street and something crashes, dashcam, security cam, tourist filming

## Prompt Anatomy

The prompt structure is the real technique here. It follows a specific pattern that makes Seedance generate convincing amateur footage:

### 1. Camera Identity (first lines)

Establish the camera as a physical object held by a real person, not an abstract viewpoint.

```
The video is shot vertically by an amateur videographer.
The camera has a constant, subtle micro-shake (the natural tremor of
holding a phone without stabilization).
```

Never say "handheld camera." Describe the PHYSICS: micro-shake, tremor, no stabilization. This is what makes Seedance simulate real phone footage instead of "cinematic handheld."

### 2. Scene Dressing with Texture

Hyper-specific environment details that signal "this is a real place, not a set."

```
The scene is an outdoor dirt courtyard backed by a heavily textured,
unevenly whitewashed wall.
```

Key words: "heavily textured", "unevenly", "cracked", "chipped". Real places are imperfect. Every surface gets a wear descriptor.

### 3. Timestamp Segmentation with Narrative Labels

Structure the prompt as labeled acts with second-by-second timestamps:

```
0:00 - 0:03: The Setup and The Tease
0:04 - 0:08: The Provocation
0:08 - 0:10: The Climax and the Breach
0:10 - 0:12: The Scramble and Camera Chaos
0:12 - 0:16: The Retreat and Aftermath
```

The narrative labels ("The Tease", "The Breach") aren't just for readability. They signal pacing and energy shifts to the model. Calm setup -> rising tension -> explosive climax -> chaotic aftermath.

### 4. Emotion Through Physics, Never Adjectives

Never write "the man panics." Describe what panic LOOKS like in the body:

```
His whole body jerks backward in a violent flinch.
His right leg kicks high into the air as he stumbles backward.
His wide blue trousers whip around his legs as he scrambles away.
```

Every emotion is a physical action: flinch, stumble, kick, scramble, jerk. The model renders physics, not feelings.

### 5. Camera-as-Character (the key differentiator)

The camera itself has a fight-or-flight response. It's not a neutral observer:

```
The cameraperson's fight-or-flight response kicks in.
The camera violently jerks downward and to the left.
The image dissolves into heavy motion blur.
The framing completely abandons the man and the lion.
```

And later, the cameraperson's own body enters frame:

```
The cameraperson's right foot—wearing a black slip-on loafer with
a silver buckle—flashes into the bottom of the frame as they
rapidly backpedal.
```

This is what sells the "real footage" illusion. The camera operator becomes a character who reacts, runs, and accidentally films their own feet.

### 6. Audio Scripting

Describe specific audio events with attribution and language:

```
A panicked, breathless female voice (the cameraperson) loudly asks,
"Kya hua?!" (Urdu/Hindi for "What happened?!").
```

Include: voice quality (breathless, panicked), who's speaking (the cameraperson), the exact words, and language/context. Seedance generates matching audio in T2V.

### 7. Abrupt End

Found footage never has clean endings:

```
The video cuts out abruptly on a shaky frame of the dirt path,
the situation entirely unresolved.
```

## Critical Rules

### NEVER mention "phone" or "smartphone" in the prompt

When the phone IS the camera, describing it makes Seedance render a phone in frame. Same bug as the selfie-with-arm pattern. Describe camera BEHAVIOR (shake, movement, angle), never the device.

Bad: `The video is shot vertically on a smartphone pointed upward`
Good: `Vertical handheld video, constant micro-shake, pointed upward at the sky`

The word "smartphone" or "phone" anywhere in the prompt risks a literal phone appearing in the shot.

### Force handheld shake explicitly

"Micro-shake" alone is not enough. Seedance defaults to stabilized/gimbal-smooth footage. You need to stack multiple instability cues:

```
handheld, slight natural shake, no stabilization, subtle drift,
occasional grip adjustment
```

Without these, the output looks gimbal-stabilized which instantly reads as "produced content", not amateur footage.

### ONE CONTINUOUS SHOT. No exceptions.

Found footage is raw, unedited. If there is a hard cut or scene transition, it was edited, and the illusion is dead. Every prompt MUST end with:

```
No scene cuts throughout, one continuous shot.
```

This is the single most important rule. A video with perfect amateur camera shake but one hard cut fails completely. A video with slightly imperfect shake but zero cuts can still pass as real.

### No subject required

The camera operator can be alone. "Person films the sky, UFO appears" works. "Person films rain, realizes it's spiders" works. The format does NOT require a second person being filmed. The camera operator + the unexpected event is the entire formula.

When there IS a subject (like the lion example in the source thread), they add tension but are not structurally necessary.

### Keep it simple

The core structure is just: **mundane situation -> unexpected thing happens -> camera reacts**. That's it. Don't overcomplicate with multiple characters, subplots, or parallel actions.

## What Makes It Work

- Camera identity established as physical phone held by real person (not abstract "camera angle")
- ONE continuous take, zero cuts, zero transitions
- Every surface/object gets a wear descriptor: chipped, cracked, unevenly whitewashed
- Emotion conveyed through body physics, never adjectives
- Camera operator reacts physically: abandons framing, jerks camera, films own feet
- Audio scripted with voice attribution, language, and emotional state
- Abrupt unresolved ending (real footage never has clean cuts)
- ~400 word prompt: pushes Seedance limits but each section is front-loaded with critical info

## What to Avoid

- **ANY hard cuts or scene transitions** - instant illusion break, non-negotiable
- "Cinematic", "4K", "dramatic lighting" - anything that breaks the amateur illusion
- Abstract emotions: "he is scared" vs "his body jerks backward in a violent flinch"
- Stable, composed framing during the chaotic section (the camera must lose control)
- Clean resolution or ending (found footage is always cut short)
- Generic environment descriptions: "outdoor area" vs "outdoor dirt courtyard backed by a heavily textured, unevenly whitewashed wall"
- Forcing a second person into frame when the scenario doesn't need one

## Common Failure Modes (Lessons)

### Hard cut mid-clip ruins the illusion

A scene cut at any point in a found-footage clip immediately breaks the "raw amateur footage" illusion. Even if the rest of the clip has perfect camera shake and amateur feel, ONE hard cut ruins everything. The prompt must explicitly forbid cuts and describe a continuous camera movement through the entire timeline. Also: simpler = better. Just the phone filming + unexpected event. No second character if the scenario doesn't need one.

### Mentioning "phone" or "smartphone" puts a literal device in frame

A prompt like "shot vertically on a smartphone pointed upward at the sky" can cause Seedance to render a literal smartphone in the bottom of the frame, held by a finger. The model interprets the device name as a thing-to-render. NEVER mention "phone" or "smartphone" in the prompt. The phone is the camera. Describe camera behavior (shake, drift, instability), not the device.

### Single shake keyword is not enough

"Micro-shake" alone often produces gimbal-stabilized output, too smooth, reads as produced content. Stack cues: "handheld, slight natural shake, no stabilization, subtle drift, occasional grip adjustment." Multiple cues stacked is what triggers actual instability.

## Technical Details

- **Mode**: T2V (text only, no references)
- **Parameters**: duration: 7-15s | aspect: 9:16 | audio: on
- **Complexity**: advanced (long prompt, many concurrent instructions)
- **Prompt length**: ~400 words for 15s. Scale down proportionally for shorter clips
- **MANDATORY closing line**: `No scene cuts throughout, one continuous shot.`

## Variations to Explore

- **Dashcam**: static mounted camera, wide angle, timestamp overlay baked into start frame, sudden event crosses frame
- **Security cam**: high angle, fixed position, night vision grain, no audio, timestamp in corner
- **Tourist footage**: person filming scenery/landmark, something unexpected enters frame, camera follows chaotically
- **Doorbell cam**: fish-eye wide angle, static, delivery person or animal triggers unexpected event
- **Sky watcher**: phone pointed at clouds/sunset, UFO/meteor/impossible object appears
- **Weather footage**: filming a storm, something surreal happens (spider rain, reversed rain, objects falling from sky)
