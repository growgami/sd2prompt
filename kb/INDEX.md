# Seedance 2.0 Knowledge Base

Field-tested patterns, techniques, and workflows. Each entry is production-proven unless marked otherwise. The KB is granular lookup; it is loaded on demand from `styles/{style}.md`, NOT from the top-level README directly.

## How to Use

- **Style files reference these.** UGC style file points to `kb/ugc-camera/*` patterns; cinematic style points to `kb/sequencing/*`; etc.
- **Ideation Mode**: browse by category or scan tags to find patterns that match a brief.
- **Prompt writing**: load the specific pattern file when a style references it.
- **Adding entries**: create a new `.md` file in the matching category (or create a new category). Update this index.

---

## ugc-camera/

Phone positioning and movement patterns for UGC-style footage. Loaded by `styles/ugc.md`.

| Pattern | Tags | File |
|---------|------|------|
| **UGC General Rules** | ugc, phone-footage, realism, framing, distance, shake, composition, rules | `ugc-camera/general-rules.md` |
| Propped-on-Desk | office, productivity, low-angle, phone-footage | `ugc-camera/propped-on-desk.md` |
| Selfie-with-Arm | selfie, front-camera, pov, direct-address | `ugc-camera/selfie-with-arm.md` |
| Wrist-Twist POV Reveal | reveal, product, transition, pov | `ugc-camera/wrist-twist-reveal.md` |
| Fit Check (Street Level) | fashion, outfit, full-body, street, low-angle | `ugc-camera/fit-check-street.md` |

## continuity/

Rules for maintaining consistency across generations. Loaded by `styles/ugc.md` and `styles/cinematic-narrative.md`.

| Pattern | Tags | File |
|---------|------|------|
| Wardrobe Rules | clothing, outfit, consistency, multishot, override | `continuity/wardrobe-rules.md` |
| Screen / Monitor Orientation | screen, monitor, ui, gotcha | `continuity/screen-orientation.md` |

## sequencing/

Multi-shot structure, pacing, and montage techniques. Loaded by `styles/ugc.md`, `styles/cinematic-narrative.md`, `styles/product-showcase.md`.

| Pattern | Tags | File |
|---------|------|------|
| Multishot Timeline | timeline, multi-shot, scene-cuts, structure | `sequencing/multishot-timeline.md` |
| Rapid Montage (15 in 15s) | montage, fast-pacing, day-in-life, hard-cuts | `sequencing/rapid-montage.md` |

## found-footage/

Amateur/accidental footage simulation. Camera behaves like a real witness, not a cinematographer. Loaded by `styles/found-footage.md`.

| Pattern | Tags | File |
|---------|------|------|
| Boomer Trap / Bystander Phone | T2V, amateur, camera-chaos, bystander, realism, found-footage | `found-footage/boomer-trap-bystander.md` |

---

## How to grow this KB

1. After a production run that uncovers a new pattern, write a new `kb/{category}/{slug}.md` file directly with frontmatter (`name`, `category`, `tags`, `tested`, `source`).
2. Add a row in the matching category table above.
3. If the pattern is style-specific, also reference it from the relevant `styles/{style}.md` file's `kb_refs` frontmatter.
4. If the pattern is universal (applies regardless of style), add it to `universal/universal-rules.md` instead.
