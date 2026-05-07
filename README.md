# sd2prompt

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contributing)

**Turn fuzzy creative briefs into structured prompts that Seedance 2.0 actually obeys.**

## The problem

[Seedance 2.0](https://seed.bytedance.com/seedance) is a frontier text/image-to-video model from ByteDance. State-of-the-art instruction following, multi-shot timelines inside a single 15-second generation, identity lock across multiple references, native audio generation. It belongs in the same conversation as Veo 3 and Kling 2.

It is also unforgiving.

A prompt that runs 1,200 words silently drops half its directives past the model's 400-word attention window. A prompt that mixes camera modes inside one shot gets averaged into mush. A prompt that re-describes a reference image's identity in the timeline causes drift instead of reinforcement. Negatives ("no tie, no jacket") fight the reference image and lose. None of this is in the docs. You discover it by burning generations.

The gap is between a fuzzy creative brief — *"confessional UGC, 7 seconds, character holds up a laptop with a Slack message"* — and a prompt structured tightly enough that the model actually executes it.

## The fix

This library is the structured thinking that goes BEFORE the API call. It turns fuzzy briefs into prompts that survive Seedance's attention window:

- **Four-block template** (META / LEGEND / TIMELINE / STABILITY) — the canonical structure every prompt uses.
- **10 universal rules** — front-load critical info, lighting first, camera body not "cinematic", separate camera from subject motion, affirmative over negative, legend as contract. The 400-word HARD CAP is non-negotiable.
- **Per-style overrides** — UGC, cinematic-narrative, product-showcase, anime-character, found-footage. Each style refines the universal rules for its mode.
- **Field-tested patterns** — selfie-with-arm framing, propped-on-desk geometry, multishot-timeline pacing, wardrobe-override mechanics, found-footage camera-as-character technique, and more.
- **Pre-emit checklist** — a gate you run before submitting, so you never pay for a generation where the back half of the prompt was silently dropped.

Pure Markdown. No scripts. No SDK. Read the rules, write the prompt, submit it.

## Seedance 2.0 at a glance

What you're working with:

| | |
|---|---|
| **Duration** | 4–15 seconds per generation. Chain for longer via last-frame anchor, shared-reference image, or full prior video as `@video1` (reliable through 2–3 extensions, 30–45s+). |
| **Aspect ratios** | 9:16, 16:9, 1:1, 4:3, 3:4, 21:9. |
| **Image references** | Up to **9** per generation — character sheets, products, scenes, content-on-screen. |
| **Video references** | Up to **3** per generation — motion / continuity anchors. |
| **Audio references** | Up to **3** per generation — VO, music, ambient sound. |
| **Total file cap** | **12 files** combined across image + video + audio refs. |
| **First / last frame** | Optionally pin the start or end frame. Mutually exclusive with multi-image-reference mode — pick one mode per generation. |
| **In-frame characters** | 2 max per frame. Above 2, identity confusion compounds. |
| **Native audio** | Speech, room tone, and ambient SFX baked into the output. |
| **Multi-shot in one prompt** | Cut-montage with explicit timestamps and hard cuts inside a single ≤15s call. No external editor needed for short sequences. |

Failure modes you should know up front:

- Text from prompt fails ~90% of the time (bake it into a reference image instead).
- Logos from prompt distort (bake into ref).
- Mixing camera modes inside one prompt produces averaged-cheap output.
- Re-describing a reference's locked traits in the timeline causes drift, not reinforcement.

## How to use

1. **Read the brief.** Classify the style (UGC? cinematic? product? anime? found-footage?).
2. **Read the rules** in this order (don't skip):
   - `universal/universal-rules.md` — 10 rules that apply to every prompt + the 400-word HARD CAP
   - `universal/four-block-template.md` — canonical structure (META / LEGEND / TIMELINE / STABILITY)
   - `styles/{chosen-style}.md` — one style file
   - `universal/chaining.md` — only if the brief needs >15s or mode-shifts. Covers all three chaining patterns: last-frame anchor, shared-reference, and video-extend (prior gen as `@video1`).
3. **Draft the prompt** against the four-block template, applying universal rules + style-specific overrides.
4. **Count words** before submitting. HARD CAP = 400 across all four blocks combined, regardless of duration. Over 400 = silently dropped directives = wasted spend.
5. **Run `universal/output-checklist.md`** as a pre-emit gate.
6. **Submit.**

## Classifier (pick one style from the brief)

| Signal in brief | Style | Why |
|---|---|---|
| "selfie", "phone", "POV", "talking head", "confessional", "amateur", "TikTok", "UGC", "vlog" | `styles/ugc.md` | Phone-as-camera, in-story operator |
| "multi-shot UGC", "TikTok with cuts", "two-shot phone footage" | `styles/ugc.md` | UGC handles multishot |
| "cinematic", "film", "trailer", "music video", "Sony Venice", "ARRI", "anamorphic", director name (Wes Anderson, Fincher, etc.) | `styles/cinematic-narrative.md` | Named camera body, controlled lighting |
| "product reveal", "360 orbit", "hero shot", "packaging", "e-commerce", "beat sync ad", "Apple keynote" | `styles/product-showcase.md` | Product as subject, multi-angle refs |
| "anime", "manga", "super saiyan", "transformation", "90s anime", "shonen" | `styles/anime-character.md` | Heavy character refs, action language |
| "security cam", "dashcam", "doorbell", "bystander", "viral clip", "boomer trap", "found footage" | `styles/found-footage.md` | Anti-cinematic, plausible witness operator |

If unclear, pick one and start. Don't try to mix styles inside one generation — the model averages them and produces low-quality output. For genuinely cross-cutting briefs (cinematic ad with one UGC insert), use chained gens, generate each shot in its own style, then edit together externally.

## Repo layout

```
sd2prompt/
├── README.md                          # this file (human onboarding)
├── SKILL.md                           # agent router — drop the repo into ~/.claude/skills/sd2prompt/ to use as a Claude Code skill
│
├── universal/                         # always loaded for any prompt
│   ├── four-block-template.md         # canonical Meta/Legend/Timeline/Stability template
│   ├── universal-rules.md             # 10 rules that apply to every prompt
│   ├── output-checklist.md            # pre-emit gate
│   └── chaining.md                    # multi-gen workflows: frame-chain (A), shared-reference (B), video-extend (C). Load when total duration >15s OR the brief calls for b-roll across multiple gens with shared world.
│
├── styles/                            # load ONE per prompt
│   ├── ugc.md                         # phone-as-camera (selfie + propped + multishot UGC)
│   ├── cinematic-narrative.md         # film/trailer/music video, named camera body
│   ├── product-showcase.md            # product reveal, 360 orbit, hero shots
│   ├── anime-character.md             # anime/manga, character consistency heavy
│   └── found-footage.md               # security cam, dashcam, bystander, viral clip
│
└── kb/                                # granular tested patterns (load on demand from styles)
    ├── INDEX.md
    ├── ugc-camera/                    # selfie-with-arm, propped-on-desk, fit-check, wrist-twist
    ├── continuity/                    # wardrobe-rules, screen-orientation
    ├── sequencing/                    # multishot-timeline, rapid-montage
    └── found-footage/                 # boomer-trap-bystander
```

## Rules of thumb (the meta)

1. **Director, not dictator.** Serve the brief; don't impose the library's defaults if the brief asks for something different.
2. **Specificity over poetry.** Every word earns its place. "Cinematic" is empty.
3. **One style per prompt.** Crossing styles produces averaged-cheap output. Use chained gens for legitimate cross-cutting needs.
4. **One variable per iteration.** Diagnose before changing everything.
5. **Confirm cost before firing.** Per-second rates × duration adds up fast across iterations. Your time and money are real.
6. **Negatives are weak; ref overrides win.** Wardrobe and identity changes go in the LEGEND `OVERRIDE` field, not as "no X" in the timeline.
7. **The legend is a contract.** Never re-describe what the legend already locks.

## Built by

[Growgami](https://growgami.com) builds distribution and GTM systems for crypto products.

## License

[MIT](./LICENSE).

## Contributing

PRs welcome. New patterns belong in `kb/{category}/{slug}.md` with frontmatter (`name`, `category`, `tags`, `tested`, `source`). Update `kb/INDEX.md`. If a pattern is universal, add to `universal/universal-rules.md` instead. If it's style-specific, surface from the style file's `kb_refs` frontmatter.

Patterns lifted from public sources (X threads, blog posts, YouTube tutorials) should credit the source author and link the original.
