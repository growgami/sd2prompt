---
name: UGC General Rules
category: ugc-camera
tags: [ugc, phone-footage, realism, framing, distance, shake, composition, rules]
tested: true
source: production-run
---

# UGC General Rules

Universal rules for any UGC-style shot in Seedance. Apply these BEFORE writing any prompt. These are not patterns for specific shots. They are reasoning guidelines that apply to every shot.

## Rule 1: Always Include Physical Distance

Every shot must specify the distance between camera and subject in meters or centimeters. Without it, Seedance defaults to medium-close professional framing.

| Distance | Use |
|----------|-----|
| 30-40cm | Detail shots (hands, feet, objects). No face. |
| 60cm-1m | Selfie-with-arm, close personal shots showing shoulders + upper chest minimum |
| 1-1.5m | Propped on surface (coffee table, counter, desk). Standard couple/single shots |
| 2-3m | Propped on shelf/dresser. Full body or medium wide |
| 3-5m | Across the room. Subject small in frame, environment dominates |

**NEVER say "close-up" without distance.** "Close-up" alone = macro/professional camera = not phone footage. "1 meter from her face showing shoulders and upper chest" = phone footage.

## Rule 2: Always Describe Foreground Obstruction

When the camera is propped on a surface, real phone footage always has objects partially cutting into the frame edges. This is the #1 signal that separates phone footage from professional camera work.

```
Good: "counter edge and a glass partially visible at bottom of frame"
Good: "remote control and mug edge visible at bottom of frame"  
Good: "dumbbell and floor visible at bottom of frame"
Good: "cart edge visible at bottom of frame"
Bad: (no foreground mentioned)
```

Always mention 1-2 objects at the frame edge for propped shots. For selfie/POV shots, the arm or hand is the natural foreground element.

## Rule 3: Stack Instability Cues

Seedance defaults to gimbal-smooth stabilized footage. One shake keyword is not enough. Stack multiple cues:

```
Always use: "handheld, no stabilization, subtle natural shake"
For movement: add "occasional grip adjustment" or "slight drift"
For action: add "camera shakes from [impact/movement source]"
For car: "natural car vibration shake"
```

"Slight drift" alone = still too stable. Stack at minimum 2-3 instability cues per shot.

## Rule 4: Always Off-Center Composition

Real phone footage is never perfectly composed. Seedance defaults to centered, symmetrical framing. Always specify:

```
"slightly off-center framing"
"too much space on the left"
"framing slightly too low showing [object] in foreground"
```

For propped shots especially, the phone is propped wherever it fits, not placed by a cinematographer.

## Rule 5: Camera Operator Plausibility

Every shot must have a plausible camera operator. Ask: **who is holding/propping the phone?**

Valid operators:
- **He holds it** (selfie style, POV, twist)
- **She holds it** (same)
- **Phone propped on surface** (counter, table, shelf, dashboard, cart, bench, floor)
- **Timer/tripod** (fit check, mirror shots)

If the answer is "nobody" or "a third person who doesn't exist in the story", the shot is invalid. No drone angles, no through-the-window shots, no impossible angles.

## Rule 6: Describe Camera Quality, Not the Device

Never say "phone", "smartphone", or "iPhone" in the prompt when the phone IS the camera. This can cause a literal phone to appear in frame (validated in found-footage tests).

Describe the QUALITY instead:
```
Good: "low light grain", "slight wide-angle distortion", "handheld, no stabilization"
Bad: "filmed on iPhone", "smartphone camera", "phone footage"
```

For selfie-with-arm shots, the phone is held by the person and the arm is visible. In that case, describe the arm + grip, not the device.

## Rule 7: Wardrobe Per Shot in Multishot

Seedance does not carry wardrobe context between timeline segments. Every shot needs explicit clothing description, even if unchanged from the previous shot.

In multishot prompts with scene cuts, describe what each character is wearing in EVERY shot. Without it, Seedance defaults to the ref image clothing or hallucinates.

Different outfits across shots = variety = authenticity. Same outfit every shot = AI-generated look.

## Rule 8: Role Assignment Inline, Not Header

Don't waste tokens on a header block assigning roles. Tag characters inline in each shot description.

```
Bad (wastes 20+ tokens):
@image1 is the boyfriend. @image2 is the girlfriend. Both maintain appearance exactly consistent with references throughout.

Good (zero wasted tokens):
0-3s: Boyfriend @image1 in grey henley on couch...
3-6s: Girlfriend @image2 in cream sweater making coffee...
```

The "@image1" tag inline already assigns the role. "Maintain consistent" is redundant. The ref image does that by default.

## Rule 9: Timestamp Discipline

For multishot clips, reinforce timing at both start and end of prompt:

```
Start: "Strict [N] seconds per shot. Each shot must transition exactly at the timestamp marked."
End: "Each change accompanied by a scene cut. Strict [N] seconds per shot."
```

Without reinforcement at both ends, Seedance often extends the first shot and rushes the last ones.

When shot durations vary (3s, 4s, 3s), the timestamps themselves enforce it. But add the closing reinforcement regardless.
