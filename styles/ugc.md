---
style: ugc
applies_to: [selfie, propped, fit-check, wrist-twist, multishot-ugc, found-footage-adjacent]
camera_mode: phone-amateur
kb_refs: [kb/ugc-camera/general-rules.md, kb/ugc-camera/selfie-with-arm.md, kb/ugc-camera/propped-on-desk.md, kb/ugc-camera/wrist-twist-reveal.md, kb/ugc-camera/fit-check-street.md, kb/sequencing/multishot-timeline.md]
---

# Style: UGC (phone-as-camera)

Real amateur self-filmed footage. The phone IS the camera. The operator is in-story (subject's hand, propped on a surface, timer/tripod). Multi-shot UGC sequences live here too: a multi-shot prompt where every shot is phone-amateur uses this style. If even one shot in the sequence is a different camera mode (cinematic insert, security cam, etc.), use chained generations or pick a different style for that shot.

## When to load this style

- Selfie talking head (front-camera POV)
- Propped-on-desk / counter / shelf shot
- Fit check street level
- Wrist-twist product reveal
- Multi-shot UGC sequence (any combo of the above)
- Confessional close-up
- POV walking / driving / cooking
- Behind-the-scenes amateur content

If the brief says "cinematic", "trailer", "music video", "broadcast" - this is NOT the right style. Load `cinematic-narrative.md` instead.

## Style-specific overrides on top of universal rules

These OVERRIDE or REFINE the universal rules for UGC.

### Override Rule 3 (camera body): describe quality, not device

Never say "phone", "smartphone", "iPhone" in the prompt when the phone IS the camera. This causes a literal phone to appear in frame holding the tweet, the recipe, the meme.

Describe quality instead:

```
Good:  "handheld, no stabilization, slight wide-angle distortion, light sensor noise, low-light grain"
Bad:   "filmed on iPhone", "smartphone camera", "phone footage"
```

For selfie shots, the phone is held by the person and the arm/hand is visible. Describe the arm + grip, never the device. The grip is the camera operator signal.

### Refine Rule 9 (declare cuts): UGC multishots ALWAYS need explicit hard cuts

UGC multishots without explicit cut markers default to weird in-shot transitions (camera flips that don't make sense, wardrobe morphs mid-shot). Always:

```
Hard scene cut at Xs exact. Quick motion blur and camera shake from the camera-switch flip.
```

The "camera-switch flip" language gives the model a plausible diegetic transition (it's just the user flipping their camera) instead of an editor's cut.

### Add Rule 11: Physical distance in cm/m

Every shot specifies the distance between camera and subject in meters or centimeters. Without it, Seedance defaults to medium-close professional framing - which is NOT what UGC looks like.

| Distance | Use |
|----------|-----|
| 30-40cm | Confessional, intimate close-up. Face fills upper 2/3 of frame. |
| 60cm-1m | Selfie-with-arm. Shoulders + upper chest visible. |
| 1-1.5m | Propped on desk/counter. Standard talking-head. |
| 2-3m | Propped on shelf/dresser. Full body or medium wide. |
| 3-5m | Across the room. Subject small, environment dominates. |

NEVER say "close-up" without distance. "Close-up" alone reads as macro/professional camera. "35cm from his face filling upper 2/3 of frame" reads as phone footage.

### Add Rule 12: Foreground obstruction (propped shots)

When the camera is propped on a surface, real phone footage always has objects partially cutting into the frame edges. This is the #1 signal that separates phone footage from professional camera work.

```
Good: "counter edge and a glass partially visible at bottom of frame"
Good: "remote control and mug edge visible at bottom of frame"
Good: "dumbbell and floor visible at bottom of frame"
```

For selfie shots, the arm/hand IS the natural foreground element. Always describe its position.

### Add Rule 13: Stack instability cues (3+)

Seedance defaults to gimbal-smooth even with one shake keyword. Stack at minimum 3 instability cues per shot:

```
"handheld, no stabilization"
"subtle natural micro-tremor in the arm"
"occasional small grip adjustment"
"slight breathing motion"
"natural body sway"
```

For movement shots add "camera shakes from [walking/impact/movement]". For car shots: "natural car vibration shake".

### Add Rule 14: Off-center composition

Real phone footage is never perfectly composed. Seedance defaults to centered, symmetrical. Always specify:

```
"slightly off-center to the right"
"too much space on the left"
"framing slightly too low showing keyboard edge in foreground"
"face in upper-left two-thirds of frame"
```

### Add Rule 15: Camera operator plausibility

Every shot must have a plausible camera operator. Ask: who is holding/propping the phone?

Valid:
- **He/she holds it** (selfie, POV, twist)
- **Phone propped on surface** (counter, desk, shelf, dashboard, cart, bench, floor)
- **Timer/tripod** (fit check, mirror shots)

Invalid:
- "nobody" or "a third person who doesn't exist in the story" - means the angle is impossible
- Drone angles, through-the-window shots, impossible perspectives

If the brief asks for an impossible angle, reframe to a propped-position that approximates the angle, or hand off to chained generations with a different style.

## Multi-shot UGC: legend wardrobe override is canonical

UGC multishots are the most common cause of wardrobe drift because the ref image often shows the character in a different outfit than the shot needs. Use the LEGEND OVERRIDE field, not negatives in the timeline.

```
Bad legend:
@image1 = Character. (timeline says "no tie, no jacket")

Good legend:
@image1 = Character. ROLE: face/hair/glasses/build. LOCK: identity only. OVERRIDE: plain crisp white dress shirt, top button undone, sleeves slightly rolled, no tie, no jacket. START FRAME: no.
```

The OVERRIDE field is affirmative ("plain crisp white dress shirt") AND lists what to discard ("no tie, no jacket") inside the override clause. The timeline then calls `@image1` only and never re-describes wardrobe.

## AI tells to avoid (UGC realism gotchas)

These are pattern fingerprints that scream "AI generated" when they appear in office/lifestyle UGC. Do NOT include them by default. If the operator explicitly asks for one, fine, but never reach for them as ambient props.

- **Coffee mug with latte art / steaming hot drink / ceramic mug + saucer + steam thread**. The single most common AI tell. Models gravitate to this as default office prop. Use water glass, a closed bottle, a reusable steel cup, or just no drink at all.
- **Pristine identical clones in mirror reflections**. Asking for a mirror reflection often yields a literal duplicate person at a slightly different position rather than a true reflection. Avoid mirrors as a compositional element. If absolutely necessary, describe the mirror geometry in extreme detail (angle, distance, what the reflection contains EXACTLY).
- **Symmetrically arranged desk objects** (laptop centered, mug at 4 o'clock, notebook at 8 o'clock). Real desks are asymmetric and cluttered. Add cables, post-its at odd angles, pen on its side, papers slightly overlapping.
- **Generic over-the-shoulder shots with a coworker pointing at a screen**. Stock-footage cliché. Either pick a specific interaction with a named action (someone leaning in to whisper, handing him a paper, showing him a phone) or skip the secondary character.
- **Identical natural lighting in every shot of a multi-shot sequence**. Real day-in-the-life footage has subtle light shifts (window vs hallway vs elevator vs cafe). Vary the lighting per location.
- **Subjects all facing camera with neutral-pleasant expressions**. UGC has people looking off-frame, mid-bite, mid-stretch, mid-frustration. Direct-to-camera is reserved for the moments where the operator decides to address the lens.

## Branded visuals require reference images

When a prompt asks for a SPECIFIC branded UI, product-with-real-design, or document with a recognizable layout, the model will invent and produce generic AI-slop unless you provide a reference image. The output will not be usable for production.

**Always required (flag to operator BEFORE writing prompt):**
- Specific UIs: Notion, Slack, Stripe, Mercury, Linear, Figma, Discord, Gmail, X/Twitter, Instagram, TikTok, YouTube, etc.
- Branded products with recognizable design: credit cards (any tier/brand), AirPods, specific phone case, branded packaging, branded apparel logos
- Documents with known layout: bank statements, tax forms, specific invoices/receipts, ID cards, boarding passes
- Specific dashboards or reports with brand identity

**NOT required (model handles fine):**
- Terminal / IDE / code editor (generic looking is acceptable; no one cares which IDE)
- Generic productivity apps without specific branding
- Plain documents (printed white pages with text, generic spreadsheets seen from afar)
- Office furniture, brick walls, factory windows, lighting, ambient props
- Food and drink (sandwich, water glass, generic plate)

**How to source ref images:**
- Real screenshot of the app/product, in the state you want it to read in the shot
- Photo of the physical product with good lighting, ideally on a neutral background
- For UIs: include the chrome (sidebar, header, status bar) so the model anchors layout signals; the EXACT content does not need to be reproduced (operator can clarify "use as layout reference, do not reproduce same content verbatim")
- Upload to a public URL via your provider/host of choice before passing to the API

**LEGEND binding pattern for branded refs:**
```
@image2 = Slack window of #channel-name. ROLE: rendered content displayed on the laptop screen in shot N. LOCK: Slack chrome (left sidebar with workspace icon and channel list, top channel header with channel name and topic, message thread layout, dark theme, message-input bar at bottom, user avatars). REPRODUCE: layout and visual identity of Slack. DO NOT REPRODUCE: the exact text content of messages — generate plausible new messages in the same visual style. START FRAME: no.
```

The "DO NOT REPRODUCE same content verbatim" clause is important when the operator's ref image contains real private content (real conversations, real account names) that should not appear literally in the rendered output — the model uses the ref as a STYLE/LAYOUT anchor, not a content replica.

**Operator-flag protocol:** Whenever proposing a shot list that involves any item from the "Always required" list above, the prompt director MUST proactively state in the proposal: "this shot requires a ref image of {X}, please provide before generation." Never quietly assume the model will get it right. Skipping this flag wastes generations and produces unusable footage (a known recurring failure: a specific app's table came out as generic productivity-app slop because no ref image was provided).

## Silent shots: explicit "mouth closed, no speaking"

For b-roll where audio is off and the operator does NOT want the subject talking, declare it explicitly per shot:

```
Mouth fully closed, no speaking, no lip movement, no audible dialogue.
```

Without this, Seedance frequently renders mouth movements that read as "talking" even when `generate_audio=false`. The visual lip-sync still happens; only the sound is muted. Force closed mouth at the prompt level.

Exception: chewing during eating shots is fine, name it explicitly (`mouth chewing briefly during the bite, then closed`).

## Pre-emit checks (UGC-specific, in addition to universal)

- [ ] Distance in cm/m specified per shot.
- [ ] Foreground obstruction described (or arm/hand for selfie).
- [ ] No "phone", "smartphone", "iPhone" anywhere in prompt body (legend OK if camera quality reference).
- [ ] 3+ instability cues stacked.
- [ ] Off-center composition specified.
- [ ] Plausible camera operator named per shot.
- [ ] If multi-shot: explicit "Hard scene cut at Xs exact" between beats.
- [ ] Wardrobe override (if needed) is in LEGEND, not timeline.

## Worked example: confessional + laptop reveal (two-shot UGC)

Generic illustration of a two-shot UGC prompt that follows the four-block template end-to-end. Substitute character + branded UI as needed.

```
[META]
Two-shot UGC sequence, 4s + 3s, 7s total. Camera mode: phone-amateur, selfie front-camera on shot 1, rear camera on shot 2. Cut-montage with hard cut at exactly 4s. Single character throughout. Vibe: confessional near-whisper into reveal.

[IMAGE REFERENCES / LEGEND]
@image1 = Subject. ROLE: identity (face, hair, glasses, build, skin texture). LOCK: face throughout both shots. OVERRIDE: plain crisp white dress shirt, top button undone, sleeves slightly rolled at the cuffs. Discard any tie, jacket, peacoat, or overcoat shown in the reference. Identity only, not wardrobe. START FRAME: no.
@image2 = Branded post / app screenshot. ROLE: rendered content displayed on a laptop monitor in shot 2 only. LOCK: post text body, primary product image, brand handle. NOT held in hand, NOT a separate screen. Lives only on the laptop display. START FRAME: no.

[TIMELINE]
0-4s: Confessional close-up. @image1 leans in toward the lens, his face about 35-40cm from the camera, filling the upper two-thirds of frame, slightly off-center to the right. His right forearm visible at the bottom-right edge of frame, bent and gripping just outside the frame edge in classic selfie-with-arm geometry. Hand grips the camera off-frame; nothing else in the hand. He is at his desk inside a small tech-startup office: exposed brick wall behind him, glass-walled meeting room with one or two colleagues blurred in the deep background, large factory windows camera-left letting in cold gray late-morning light, exposed brick on the right.

@image1 makes direct eye contact with the lens. Confessional tone, low volume, almost whispered. He says, near-whispering: "{line 1}... {line 2}... {line 3}... look." Sharp drop on key word, flat hushed certainty in the middle, calm low invitation on "look". Heavy handheld feel: subtle natural shake, micro-tremor in the arm, occasional small grip adjustment, slight breathing motion, light wide-angle selfie distortion on the face.

Hard scene cut at 4s exact. Quick motion blur and camera shake from the camera flip transition.

4-7s: LAPTOP REVEAL. The camera now points down at a silver-and-black laptop sitting open on the wooden desk. The laptop's monitor fills roughly 80 percent of the frame, with @image2 reproduced sharply on the laptop display. Matte black laptop bezel visible at the top and side edges of the frame, plus a strip of wooden desk surface below the laptop and a small bit of laptop keyboard at the very bottom of frame, anchoring the screen as a laptop monitor. @image1 is mostly out of frame, only a faint silhouette of his right hand and forearm at the very bottom-right corner of frame, holding the camera off-frame.

Behind the laptop the office bleeds in softly: exposed brick wall in the top-right corner, warm desk lamp glow camera-right, the glass meeting room a distant blur further back. Cold gray morning light from camera-left. Held steady on the laptop monitor for 3 seconds, no spoken dialogue, only soft ambient office room tone.

[STABILITY BOOSTERS]
Strict 4s on shot 1, 3s on shot 2, 7s total. Each change accompanied by a scene cut. Single character: @image1 in both shots. No third character entering frame. No additional handheld objects in his grip. No drone angles. No cuts within shots. Wardrobe consistent across both shots: white dress shirt, top button undone, no tie, no jacket. Camera operator: @image1 himself, holding the camera off-frame. Heavy handheld amateur feel throughout. @image2 appears only in shot 2, only on the laptop screen, never held.
```

## KB references

For deeper patterns within UGC, load:
- `kb/ugc-camera/general-rules.md` - foundational UGC rules (distance, obstruction, instability)
- `kb/ugc-camera/selfie-with-arm.md` - selfie-specific framing
- `kb/ugc-camera/propped-on-desk.md` - propped-position patterns
- `kb/ugc-camera/wrist-twist-reveal.md` - reveal mechanic
- `kb/ugc-camera/fit-check-street.md` - full-body street level
- `kb/sequencing/multishot-timeline.md` - multi-shot timing
- `kb/continuity/wardrobe-rules.md` - wardrobe override mechanics
- `kb/continuity/screen-orientation.md` - screen/laptop placement gotchas
