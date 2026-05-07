---
style: found-footage
applies_to: [security-cam, dashcam, bystander-phone, accidental-witness, viral-clip]
camera_mode: amateur-witness
kb_refs: [kb/found-footage/boomer-trap-bystander.md]
---

# Style: Found Footage

Footage that looks like nobody intended to film it. Security cam catches the moment. Dashcam captures the crash. Bystander films from across the street with shaky hands. Anti-cinematic on purpose.

## When to load this style

- Security camera / CCTV footage simulation
- Dashcam clip
- Bystander phone footage (someone filming an event from far away)
- "Accidental witness" moment (someone's pulling out their phone too late)
- Viral-clip aesthetic (low quality, poor framing, no production values)
- Boomer-trap content (older person filming themselves badly without realizing)

NOT for: polished phone-amateur UGC (use `ugc.md`), cinematic narratives (use `cinematic-narrative.md`), product reveals (use `product-showcase.md`).

## Style-specific overrides on top of universal rules

### Override Rule 3 (camera body): name the camera type, not body or quality

Found footage cameras are recognizable by their characteristic flaws. Name the camera type and its signature flaw:

```
"static security camera, 480p surveillance grade, fisheye distortion, time-and-date overlay top-right"
"dashcam mounted to windshield, wide-angle lens, slight rolling shutter on movement, low-light grain"
"bystander phone footage from ~30m away, vertical 9:16, heavy hand shake, autofocus hunting"
"old camcorder VHS, interlace lines, low resolution, color bleed, date stamp bottom-right"
```

### Override Rule 8 (one truth source): found footage IS its mode

Pick the found-footage type and stick with it for the entire generation. Don't mix security cam + bystander phone unless you're explicitly making a montage of multiple sources (in which case use chained gens, not one prompt).

### Add Rule 11: Camera operator is plausible witness or fixed mount

The operator is NEVER a cinematographer. It's:

- **A fixed mount** (security cam on a building corner, dashcam on a windshield, doorbell cam, ceiling cam in a store)
- **A bystander** filming far away with reluctance, distraction, or technical incompetence
- **The subject themselves filming badly** (boomer trap pattern: holding the phone too low, framing on their chin, panning past the action they meant to capture)

Declare the operator in META: `Camera operator: ceiling-mounted security camera, 480p surveillance grade.` or `Camera operator: bystander filming with shaky hands from across the street, vertical phone in portrait mode.`

### Add Rule 12: Anti-composition is the composition

Found footage is BADLY framed on purpose:

```
"subject barely visible at the edge of frame because the camera operator was looking elsewhere"
"head cut off at the top of frame; hand visible but not the action"
"camera pointing at the wrong angle; subject only enters frame at 2.5s as the operator notices"
"focus drifts in and out as autofocus hunts in low light"
```

Don't write "perfectly framed close-up" - that breaks the illusion. Write the failure modes that make it look real.

### Add Rule 13: Diegetic overlays (timestamp, watermark, channel name)

Real found footage has on-screen artifacts. Bake them into the @image1 ref or describe them in the timeline:

```
"timestamp overlay top-right reading '12:34:07 AM' in white sans-serif"
"camera ID watermark bottom-left reading 'CAM-04' in green"
"YouTube playback bar partially visible at the bottom"
"battery icon flickering top-right corner"
"vertical phone aspect with iOS-style status bar at top"
```

These details ARE the genre signal. Without them, the output looks like polished cinema pretending to be found footage.

### Add Rule 14: Audio is messy or absent

Found footage rarely has clean audio. Either:

- **Silent**: `generate_audio: false` in API call. Pure visual.
- **Ambient room tone only**: HVAC hum, distant traffic, no dialogue.
- **Distant muffled audio**: subject is talking but too far for the mic to catch clearly.
- **Clipped phone audio**: low-bitrate, distorted on loud sounds.

Don't over-direct dialogue here. The dialogue is incidental, not staged.

## Pre-emit checks (found-footage-specific, in addition to universal)

- [ ] Camera type named (security cam / dashcam / bystander phone / camcorder / doorbell cam).
- [ ] Camera operator plausibility declared (fixed mount OR bystander OR boomer trap).
- [ ] Anti-composition described (frame failures, autofocus hunting, off-center, partial visibility).
- [ ] Diegetic overlays specified (timestamp, watermark, channel ID, status bar).
- [ ] Audio strategy declared (silent / ambient / muffled / clipped).
- [ ] No "cinematic" or "professional" language anywhere in prompt.

## KB references

- `kb/found-footage/boomer-trap-bystander.md` - the boomer-trap / bystander phone pattern with tested prompt language
