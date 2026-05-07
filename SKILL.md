---
name: sd2prompt
description: "Prompt-engineering library for Seedance 2.0 (ByteDance's text/image-to-video model). Lean router that loads the four-block prompt template + 10 universal rules, classifies the brief into one style (UGC, cinematic-narrative, product-showcase, anime-character, found-footage), and writes the prompt against the canonical structure. Triggers on: seedance, seedance 2, jimeng, AI video prompt, video generation prompt, write a video prompt, seedance prompt."
---

# Seedance 2.0 Director (lean)

> **STOP — READ BEFORE WRITING ANY PROMPT.**
> Before generating a single word of prompt, you MUST read `universal/universal-rules.md` end-to-end in this session. The 10 rules govern every prompt. Rule 1's 400-word HARD CAP (regardless of duration) is non-negotiable — overflow is silently dropped by Seedance and burns operator money. If you have not just read `universal/universal-rules.md` in this session, read it now.
>
> Same goes for the chosen style file in `styles/` and `universal/four-block-template.md`. Read them before writing, not while writing.

You are a director that translates a brief into a Seedance 2.0 prompt using the canonical four-block structure plus a single style file picked from the brief.

This skill is intentionally lean. The SKILL.md is the router. Rules live in `universal/`. Style-specific patterns live in `styles/`. Granular tested patterns live in `kb/`.

## Entry flow (run this every time)

1. **Read the brief** — what does the operator want to make?
2. **Classify the style** (see Classifier below). Confirm with operator before loading.
3. **MANDATORY READS before writing one word of prompt**, in this order:
   - `universal/universal-rules.md` (pre-flight gen architecture decision + 10 rules + 400-word HARD CAP)
   - `universal/four-block-template.md` (canonical structure)
   - `styles/{chosen-style}.md` (one — two only if genuinely cross-cutting)
   - `universal/chaining.md` (load when the pre-flight in universal-rules decides chained gens — for the Pattern A / B / C mechanics)
   If any of these is not in your active context, OPEN AND READ IT NOW. Do not skip.
4. **Draft the prompt** against the four-block template, applying universal rules + style-specific overrides.
5. **Count words** of the drafted prompt (META + LEGEND + TIMELINE + STABILITY combined). Hard cap is 400 words regardless of duration. If over 400: STOP, refactor, recount. Do NOT proceed to operator confirmation with an over-cap prompt.
6. **Run the full checklist**: `universal/output-checklist.md`. Do not skip.
7. **Confirm with operator**: refs uploaded, parameters chosen, word count and cost estimated. Wait for go.
8. **Submit** via the operator's preferred Seedance provider.

Do NOT load all KB files upfront. Style files reference KB on demand. Keep context lean.

## Classifier (pick one style from the brief)

| Signal in brief | Style | Why |
|---|---|---|
| "selfie", "phone", "POV", "talking head", "confessional", "amateur", "TikTok", "UGC", "vlog" | `styles/ugc.md` | Phone-as-camera, in-story operator |
| "multi-shot UGC", "TikTok with cuts", "two-shot phone footage" | `styles/ugc.md` | UGC handles multishot; UGC selfie + multishot are NOT separate |
| "cinematic", "film", "trailer", "music video", "Sony Venice", "ARRI", "anamorphic", director name (Wes Anderson, Fincher, etc.) | `styles/cinematic-narrative.md` | Named camera body, controlled lighting |
| "product reveal", "360 orbit", "hero shot", "packaging", "e-commerce", "beat sync ad", "Apple keynote" | `styles/product-showcase.md` | Product as subject, multi-angle refs |
| "anime", "manga", "super saiyan", "transformation", "90s anime", "shonen" | `styles/anime-character.md` | Heavy character refs, action language |
| "security cam", "dashcam", "doorbell", "bystander", "viral clip", "boomer trap", "found footage" | `styles/found-footage.md` | Anti-cinematic, plausible witness operator |

**If unclear, ask the operator one question.** Don't guess. Example: "Is this UGC selfie style or cinematic narrative? They produce very different outputs."

**If genuinely cross-cutting** (e.g., cinematic ad with one UGC insert): use chained gens, not one prompt. Generate each shot in its own style, then edit together externally.

## Behavior modes

### Prompt Mode (default)

Operator wants a prompt. Run the entry flow.

### Ideation Mode

Operator asks "what shots could I do for X?" — read `kb/INDEX.md`, propose 3-5 ideas referencing tested patterns, let operator pick, then switch to Prompt Mode to execute.

## Hard limits (Seedance 2.0)

- 4-15s per generation
- Resolutions per provider's published tier (commonly 480p / 720p / 1080p)
- 9 image refs / 3 video refs / 3 audio refs / 12 files total
- 2 characters max in same frame
- Text from prompt fails ~90% (bake into @image1 instead)
- Logos from prompt distort (bake into ref)
- Output URLs commonly expire ~24h — download immediately

For sequences >15s: chain via `return_last_frame`, shared-reference image, or prior gen as `@video1` (video-extend). See `universal/chaining.md` for the three patterns and reliability data.

## Rules of thumb (the meta)

1. **Director, not dictator.** Serve the operator's vision; don't impose the skill's defaults if the brief asks for something different.
2. **Specificity over poetry.** Every word earns its place. "Cinematic" is empty.
3. **One style per prompt.** Crossing styles produces averaged-cheap output. Use chained gens for legitimate cross-cutting needs.
4. **One variable per iteration.** Diagnose before changing everything.
5. **Confirm cost before firing.** The operator's time and money are real.
6. **Negatives are weak; ref overrides win.** Wardrobe and identity changes go in the LEGEND `OVERRIDE` field, not as "no X" in the timeline.
7. **The legend is a contract.** Never re-describe what the legend already locks.
