---
name: history-shorts-microdoc
description: Use when creating, revising, or preparing upload metadata for a high-retention YouTube Short, Reel, or TikTok for a history channel built around tiny human mistakes causing massive consequences, especially with ElevenLabs voice/music, word-synced captions, HyperFrames, ~40 second loop-engineered vertical edits, or YouTube publish packs.
---

# History Shorts Microdoc

## Core Promise

Create a high-retention micro-documentary, not a lecture. The owned format is:

`One tiny mistake. Massive consequences.`

The video must focus on the exact small lapse, decision, door, delay, missed signal, ignored warning, or institutional failure that triggered consequences wildly larger than the mistake itself.

The signature line `One tiny mistake. Massive consequences.` is the format's brand, but it is **not spoken in the video and not shown on screen**. It lives in the title, description, and pinned comment. The narration ends on a loop bridge instead (see "Loop Engineering") — a spoken payoff or CTA marks the ending and kills replays.

### Format Invariants

The Shorts feed calibrates a channel's seed audience on format consistency. These markers are invariants — never change them between videos: the top context bar style, the gold/hot/cool caption palette, the channel voice, the `Rewind` cue, the consequence-first structure, and the loop-engineered ending.

## Target Format

- Platform: YouTube Shorts / Reels / TikTok.
- Duration: 38-44 seconds, target ~40. Every extra second past the story is a retention risk; the algorithm's push threshold for 30-60s Shorts is roughly 50% average retention.
- Hook: the first spoken word lands within 0.5s, the first caption group is visible within 0.3s, and the frame at `t=0` is the strongest of the 6 images (see "Hook — The First 2 Seconds").
- Ending: loop-engineered — final frame visually matches `t=0`, final line bridges into the opening line, no spoken CTA, audio tail ≤0.3s (see "Loop Engineering").
- Visual structure: 6 approved vertical images, one per major beat.
- Captions: every spoken word appears on screen.
- Caption grouping: 1-3 words max, large centered text.
- Voice: catchy reel tone, not flat documentary narration.
- Music: ElevenLabs instrumental bed, low under the voice.
- Sound effects: ElevenLabs SFX layer (riser, impact hit, whooshes, optional ticking) synced to beat changes and punch-ins (see "Sound Effects").
- Images: no artificial dark overlay, smoke overlay, or vignette layered on top of the shot. Natural scene lighting as generated — including moody, low-key, or night scenes — is fine; do not force every image to read as bright daylight.
- Opening frame: never black; the first scene must be visible at `t=0`.
- Top context bar: every final video must include a horizontal line near the top plus compact text that states the event place and date/year.

## Hook — The First 2 Seconds

The viewer decides whether to swipe in roughly 1.3-1.8 seconds; 50-60% of all drop-off happens inside the first 3 seconds. A 5-second hook is too slow — the hook must be fully deployed by ~2 seconds.

- The frame at `t=0` is the single strongest of the 6 images — the consequence at its most extreme, not an establishing shot. It also serves as the de-facto thumbnail in the Shorts grid, so the subject must read at thumbnail size. This is why it is always generated with `FLUX pro` while the rest use `FLUX dev` (see "Autonomous Start Rules").
- The first spoken sentence is ≤8 words and leads with the most extreme concrete fact: `70,000 soldiers. One forgotten door.`
- First word audible within 0.5s; first caption group on screen within 0.3s; Ken Burns motion already running at frame zero.
- The tease follows immediately and opens the curiosity loop: name what it *wasn't*, promise something smaller — `It wasn't the cannons. It was smaller.`
- No logos, no intro cards, no musical riser before the first word.

## Loop Engineering

Every replay counts as a view, and average retention above 100% — the video loops without the viewer noticing — is the strongest satisfaction signal a Short can send. The loop is designed, not hoped for:

- The final visual is the same consequence image as `t=0`, with its Ken Burns move landing on (or very near) the exact framing of frame zero, so the cut back to the start is invisible.
- The final spoken line is a **loop bridge**: it semantically completes into the opening line. Example: `...all of it, because of one open door.` → loops into `70,000 soldiers couldn't break these walls.`
- No spoken CTA. `Subscribe for more` is banned from narration; subscription asks live in the pinned comment and description.
- The signature `One tiny mistake. Massive consequences.` is also not spoken — it belongs to title, description, and pinned comment.
- Audio tail after the last word: ≤0.3s. No long music fade-out, no silence pad, no closing SFX — nothing that says "this is the end."
- QA loop test: play the junction (last second + first second) twice on a phone. If you can feel where the video ends, the loop fails.

## Autonomous Start Rules

For this channel format, the opening interaction must be fast and fully autonomous.

- Ask **no** preference questions at the start — including the image model. The image-model policy is fixed:
  - **The hook image (beat 1, the frame at `t=0`) is always generated with `FLUX pro`.** It is the swipe-stopper and the de-facto thumbnail; it gets the premium model.
  - **The other 5 images are always generated with `FLUX dev`.** They are supporting beats behind heavy motion and captions; dev quality is sufficient and keeps cost down.
- Do not ask for confirmation of concept direction, pipeline, render runtime, authoring mode, image model, voice, music, caption style, scene count, or publish-pack structure at the beginning. Use the defaults in this skill and move.
- Lock `render_runtime = "hyperframes"` and `composition_mode = "atelier"` for every history-shorts-microdoc production. Do not present Remotion or FFmpeg as alternatives for this format, even if they are available.
- Use HyperFrames for the final composition and publishable render. FFmpeg is allowed only for inspection, extraction, remuxing, loudness normalization, and image utility work after the HyperFrames render exists.
- If HyperFrames is unavailable or broken, stop and surface that as a blocker. Do not silently switch to Remotion or FFmpeg composition.
- Use the established channel defaults unless the user already supplied a contrary preference in the prompt: ElevenLabs voice/music, 6 vertical images, consequence-first structure, large kinetic centered captions, and YouTube publish pack.

## Production Workflow

1. Follow the OpenMontage pipeline and registry rules before generation.
2. Proceed autonomously with this skill's defaults — no opening questions. Image models are fixed: `FLUX pro` for the hook image (beat 1), `FLUX dev` for the other 5. Record this as the `provider_selection` decision in the decision log.
3. Use or generate up to 6 vertical images for the full short. **HARD LIMIT: never generate more than 6 images in a single session.** If the story seems to need more, adapt the beat plan, reuse an approved image for a later cut, or use internal motion/emphasis instead of asking for extra images.
4. Once images exist, freeze them. Do not regenerate or delete images unless the user explicitly asks or a generation failed technically.
5. Default to a consequence-first hook: the massive outcome is on screen at `t=0` and named in the first spoken sentence (≤8 words, fully landed within ~2 seconds), then rewind into context and hold the tiny-mistake reveal until the 60-70% mark.
6. Write an 80-95 word script for 38-44 seconds when using the consequence-first flow. Let pauses and punctuation create suspense; do not pad with lecture context. The last line must be the loop bridge, not a payoff or CTA.
7. Generate ElevenLabs TTS at the right duration directly. Do not plan to fix pacing later with FFmpeg `atempo`.
8. Generate ElevenLabs music to match the target duration.
9. Generate the ElevenLabs sound-effects layer (riser, impact hit, whooshes, optional ticking) once narration word timings exist, so hits can be synced to visual events.
10. Build the final composition in HyperFrames atelier, always.
11. Render, inspect frames, check transition frames, check the loop junction (last second → first second), check audio loudness, then normalize/remux only if needed.
12. Create a YouTube publish pack before final handoff, including the publishable MP4.
13. After publish, run the post-publish analytics check (see "Post-Publish Analytics") and record findings for the next production.

## Blocker And Non-Response Handling

If a paid-generation blocker occurs mid-production (exhausted provider credit, rate limit, auth failure) and a regeneration or provider swap is needed:

1. Surface the blocker immediately (what was attempted, what failed, root cause, options, recommendation) and ask the user once how to proceed.
2. If the user does not respond within the wait window: do **not** stall the whole production, and do **not** regenerate, spend more, or swap providers without an explicit response. Silence is not approval to spend or to substitute.
3. Proceed with whatever is already generated and paid for. Finish the pipeline (script through publish pack) rather than leaving it half-done.
4. Flag every known asset issue caused by the blocker clearly and specifically — in the decision log, `final_review.json`, and the YouTube `upload-checklist.md` — so the gap is visible before anyone publishes, not discovered after.
5. Tell the user exactly how to unblock it later (e.g. "top up billing, then ask me to regenerate scene-4") so resuming is a one-line request, not a re-investigation.

This only applies to in-flight blocker recovery. At the beginning of a normal history-shorts-microdoc run there are no required user choices: every default — including the FLUX pro (hook) / FLUX dev (other 5) image-model split — is locked by this skill.

## ElevenLabs Voice

Use ElevenLabs for voice unless the user explicitly chooses another provider.

- Preferred model: `eleven_multilingual_v2`.
- Preferred channel voice: use the user's configured channel voice. For the Constantinople short, the successful voice was Rachel, `voice_id=21m00Tcm4TlvDq8ikWAM`. If the user provides a different "voice XXX", use that exact voice name or voice ID and record it in the decision log.
- Tone: catchy YouTube Reel narrator, urgent, punchy, curious, not solemn lecture.
- Stability starting point: around `0.30-0.40`.
- Similarity boost starting point: around `0.70-0.80`.
- Style starting point: around `0.60-0.75`.
- Speed: adjust in the ElevenLabs request, usually `1.05-1.12`, so the generated narration lands in 38-44 seconds without post-speeding.

If the narration is too long, shorten the script and regenerate TTS. Avoid using FFmpeg to speed up narration unless the user explicitly approves a technical rescue.

## ElevenLabs With Timestamps

ElevenLabs has a TTS endpoint that returns audio plus alignment:

```text
POST /v1/text-to-speech/{voice_id}/with-timestamps?output_format=mp3_44100_128
```

Use it for final narration when word-level captions are required. It returns `audio_base64`, `alignment`, and usually `normalized_alignment`. Prefer `normalized_alignment` for word timings.

Important fallback lesson:

- Local HyperFrames transcription may return `whisper_unavailable`.
- ElevenLabs Scribe may fail if the key lacks `speech_to_text` permission.
- The TTS `with-timestamps` endpoint can still provide native word timings for the generated voice.

Save:

- final narration audio in `assets/audio/`
- raw ElevenLabs timestamp JSON in `artifacts/`
- normalized words JSON in `artifacts/`
- caption groups JSON and a HyperFrames-ready JS file in `hyperframes/assets/`

## Music

Use ElevenLabs music generation for this format when available.

Prompt pattern:

```text
Instrumental high-retention YouTube Shorts history reel, modern cinematic tension, dark pulse, punchy low drums, subtle ticking percussion, short risers, no vocals, no singing, no spoken words, no lead melody competing with narration, for voiceover ducking, no final cadence or outro, loop-friendly ending, [duration] seconds.
```

Mixing rules:

- Place music under narration, usually `data-volume="0.12"` to `0.18`.
- Do not fade music out over a long tail — a 1-2 second fade tells the viewer the video is ending and kills the loop. End the bed at level, or with a fade no longer than ~0.3s, so the junction back to `t=0` does not sound like an ending.
- After final render, check loudness. Shorts should not be quiet.
- If integrated loudness is below about `-18 LUFS`, normalize final audio to approximately `-16 LUFS`, true peak around `-1.5 dBTP`, while preserving video.

Useful FFmpeg check:

> **⚠️ Run via Bash (Git Bash / WSL), NOT PowerShell.** On Windows, `-f null NUL` returns exit code 183 (ERROR_ALREADY_EXISTS); Bash with `-f null /dev/null` works correctly for both the analysis pass and any re-encode. Always add `-ar 48000` when re-encoding audio — without it ffmpeg may double the sample rate to 96 kHz.

```bash
# Pass 1 — measure loudness
ffmpeg -hide_banner -nostats -i final.mp4 \
  -af loudnorm=I=-16:TP=-1.5:LRA=11:print_format=json \
  -f null /dev/null

# Pass 2 — normalize (substitute measured_* values from Pass 1 output)
ffmpeg -i final.mp4 \
  -c:v copy \
  -af "loudnorm=I=-16:TP=-1.5:LRA=11:measured_I=<I>:measured_TP=<TP>:measured_LRA=<LRA>:measured_thresh=<thresh>:offset=<offset>:linear=true" \
  -c:a aac -b:a 192k -ar 48000 \
  final_normalized.mp4
```

## Sound Effects

Use ElevenLabs sound-effects generation as a third audio layer. Voice carries the story, music carries tension, SFX carries pacing — synced SFX hits are what make a 6-image still edit feel fast-cut without extra images.

Standard kit per video:

- One riser (`1-1.5s`) building into the tiny-mistake reveal.
- One deep impact hit landing exactly on the reveal.
- A short whoosh on each scene crossfade and on the `Rewind` pattern interrupt.
- Optional low ticking loop under the tension beats (beats 3-5).

Mixing rules:

- SFX sit above the music bed but never over the voice: usually `data-volume="0.25"` to `0.35`.
- Sync every hit to its visual event (punch-in, color pop, scene change) within `±0.05s` — an off-sync hit is worse than no hit.
- No SFX in the last `0.5s` of the video: nothing may signal the ending at the loop junction.

## Caption Style

The reference style is not subtitle style. It is large kinetic text in the center.

Required:

- Every spoken word appears on screen.
- Max 3 words per group.
- Words appear exactly when pronounced.
- Use uppercase.
- Use centered text.
- Use dynamic fitting so long words like `CONSTANTINOPLE` and `KERKOPORTA` do not clip.
- Use compact dynamic SVG widths for short words too. Do not give every word the same wide SVG box; short words such as `ON`, `IT`, and `THE` should not spread across the whole caption row.
- Do not group words across sentence-ending punctuation. For example, `on it. That` must render as `ON IT` then `THAT`, never as `ON IT THAT`.
- Use selective color emphasis for important words. Do not leave every caption word white unless the piece has a deliberate monochrome reason.

Preferred implementation:

**CRITICAL — use ONE SVG per display line, not one per word.**

An SVG element without explicit `width` and `height` attributes defaults to 300px wide in headless Chrome (the HyperFrames renderer). With a narrow `viewBox` and a large `font-size`, this causes the renderer to scale the text by `300 / viewBoxWidth`, producing enormous unreadable glyphs that overflow vertically in the flex container. Setting only `height: 1em; width: auto` triggers the same bug.

Rules:
- One `<svg>` per display line. Each `<text>` holds all words on that line as `<tspan>` elements.
- Always set explicit `width` and `height` attributes on the `<svg>` element.
- Use `<tspan class="cg-high">WORD</tspan>` (or `cg-hot`, `cg-cool`) for color emphasis. Stroke and `paint-order` inherit from `.cg-text` on the parent `<text>`; only `fill` changes per tspan.
- SVG sizing formula: `height = round(font_size * 1.45)`, text baseline `y = round(font_size * 1.10)`.

**Line splitting for caption groups:**
- The SVG container is 960 px wide (matches the `left:60px; right:60px` margins on the 1080 px frame). Every sizing target below is against 960 px, not 1000 — sizing against 1000 px overflows the actual container.
- 1 word → 1 SVG line.
- 2 words → 1 SVG line if estimated width ≤ 960 px, else 2 × 1-word lines.
- 3 words → always 2 SVG lines. Split rule: estimate line-1 as `est(words[:2])`. If ≤ 900 px use **2+1** (first two words on line 1); if > 900 px use **1+2** (first word alone on line 1).
- Width estimate at 96 px: `sum(len(w) for w in words) * 70 + max(0, len(words)-1) * 30`.
- Per-line font size: if estimated width ≤ 960 px use 96 px; else `max(60, int(960 * 96 / estimated))`.
- Safety fallback (long-word pairs): after sizing, if a 2-word line's rendered width (`estimated * font / 96`) still exceeds 960 px even at the 60 px floor — e.g. `CODEBREAKERS DECODED`, two long words that only look short by character count — split that line into two separate 1-word lines instead of forcing an overflow. A 3-word group can render as 3 stacked lines in this rare case; that's correct over clipping.

**Caption position — center of screen, not bottom:**
- Use `top: 0; bottom: 0` spanning the full height with `flex-direction: column; justify-content: center` to center content vertically.
- Do not use a fixed `top: Npx` or `bottom: Npx` value; those push captions to a corner and look like subtitles.

Pattern:

```html
<!-- 1-word group -->
<div class="caption-group" id="cg6">
  <svg class="cg-svg" width="960" height="139" viewBox="0 0 960 139" overflow="visible">
    <text class="cg-text" x="480" y="106" text-anchor="middle" font-size="96">
      <tspan class="cg-high">REWIND</tspan>
    </text>
  </svg>
</div>

<!-- 3-word group, 2+1 split: "MOUNTAINS WERE" | "WRONG" -->
<div class="caption-group" id="cg23">
  <svg class="cg-svg" width="960" height="139" viewBox="0 0 960 139" overflow="visible">
    <text class="cg-text" x="480" y="106" text-anchor="middle" font-size="96">
      <tspan class="cg-norm">MOUNTAINS </tspan><tspan class="cg-norm">WERE</tspan>
    </text>
  </svg>
  <svg class="cg-svg" width="960" height="139" viewBox="0 0 960 139" overflow="visible">
    <text class="cg-text" x="480" y="106" text-anchor="middle" font-size="96">
      <tspan class="cg-high">WRONG</tspan>
    </text>
  </svg>
</div>
```

```css
/* Full-height container: content centered vertically in the 1920px frame */
.caption-group {
  position: absolute;
  left: 60px;
  right: 60px;
  top: 0;
  bottom: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 16px;
  opacity: 0;
  will-change: opacity;
  z-index: 10;
}

/* ALWAYS set explicit width + height — never omit them on SVG elements */
.cg-svg { display: block; overflow: visible; }

.cg-text {
  font-family: "Montserrat", "Inter", Arial, sans-serif;
  font-weight: 900;
  stroke: #050607;
  stroke-width: 7px;
  stroke-linejoin: round;
  stroke-linecap: round;
  paint-order: stroke fill;
}

.cg-norm { fill: #FFFFFF; }
.cg-high { fill: #FFD700; }  /* tiny-mistake, key reveal words, REWIND */
.cg-hot  { fill: #FF6048; }  /* danger, death, consequence */
.cg-cool { fill: #B8D7FF; }  /* system, process, order */
```

### Caption Entrance And Exit Motion

Use the kinetic caption motion from `projects/the-door-that-ended-rome` as the house effect. It is a punchy GSAP pop-in with a slight overshoot, followed by a quick upward fade-out.

Reference behavior:

- The caption group starts slightly low and small: `opacity: 0`, `y: 26-30px`, `scale: 0.96-0.97`.
- At the group's start time, animate the whole group to `opacity: 1`, `y: 0`, `scale: 1` over about `0.08s` with `power2.out`.
- Each word/line starts lower and smaller: `opacity: 0`, `y: 40-46px`, `scale: 0.90`.
- Animate each spoken word or display line at its word timestamp, usually `word.start - 0.015`, to `opacity: 1`, `y: 0`, `scale: 1` over about `0.12s` with `back.out(1.9)`. This is the visible "pop" effect.
- At the group end time, animate the group out to `opacity: 0`, `y: -30px`, `scale: 0.97` over about `0.10s` with `power2.in`, then set it hidden.
- Use GSAP timelines inside HyperFrames for this effect. Keep timing deterministic from ElevenLabs word timestamps.

Pattern:

```js
tl.set(groupSelector, { visibility: "visible" }, Math.max(0, group.start - 0.01));
tl.to(groupSelector, {
  opacity: 1,
  scale: 1,
  y: 0,
  duration: 0.08,
  ease: "power2.out",
  overwrite: "auto",
}, group.start);

group.words.forEach((word) => {
  tl.to(`#${word.id}`, {
    opacity: 1,
    scale: 1,
    y: 0,
    duration: 0.12,
    ease: "back.out(1.9)",
    overwrite: "auto",
  }, Math.max(0, word.start - 0.015));
});

tl.to(groupSelector, {
  opacity: 0,
  scale: 0.97,
  y: -30,
  duration: 0.10,
  ease: "power2.in",
  overwrite: "auto",
}, group.end);
tl.set(groupSelector, { opacity: 0, visibility: "hidden" }, group.end + 0.11);
```

Do not use `text-shadow` as a fake extrusion. It creates an ugly ghost/duplicate behind the text.

Do not use CSS `-webkit-text-stroke` for final large HyperFrames captions. In rendered MP4 frames, it can drop parts of vertical outlines on T-shaped glyphs (`THE`, `NOT`, `TINY`). Use SVG text outlines instead.

### Caption Color Emphasis

Use color as a retention tool, not decoration.

- Keep most words in the main off-white caption color.
- Highlight only 1-3 important words per beat, or fewer when the beat is already visually busy.
- Use a stable semantic palette:
  - hot red/orange for danger, death, fire, attack, impact, or irreversible consequence.
  - warm gold for the tiny mistake, key reveal words, and the `Rewind` cue.
  - cool blue/steel for procedural/system words such as route, driver, order, gate, signal, lock, or briefing.
- Preserve the same black SVG stroke on all colors so colored words remain readable over bright images.
- Avoid random color changes, rainbow captions, gradients inside text, and coloring every word in a group.
- Review color frames on the actual rendered MP4. A color that looks good on a dark preview may disappear over a bright historical image.

### Number And Date Display

Captions must show numbers, dates, currency, and percentages as digits, not as the spelled-out words the narrator speaks. The audio narration is unaffected -- ElevenLabs still speaks the natural word form; only the on-screen caption text changes.

Examples:

| Spoken (audio) | Caption (on screen) |
|---|---|
| "nineteen seventy-six" | `1976` |
| "three hundred billion dollars" | `$300 BILLION` |
| "eight hundred dollars" | `$800` |
| "fifteen thousand dollars" | `$15,000` |
| "fifteen hundred" | `$1,500` |
| "ten percent" | `10%` |
| "twelve days" | `12 DAYS` (only the number word converts; keep the following noun as its own word) |

Rule: identify the run of consecutive spoken words that together form a single number/date/currency/percentage phrase, and replace that whole run with **one merged caption token** carrying the digit form, timed from the first spoken word's start timestamp to the last spoken word's end timestamp. Do not spell out a unit word a second time once a symbol already implies it (e.g. use `$300 BILLION`, not `$300 BILLION DOLLARS`). A merged token still counts as exactly one word for the max-3-words-per-group and line-splitting rules -- do not count the original number of spoken words it replaces.

## Visual Style

Avoid the old failed look:

- Huge `Arial Black`.
- Over-heavy outline.
- Dark overlay over every scene.
- Smoke overlay.
- Vignette.
- Title cards that are not spoken.
- Text that clips at top/left edges.

Use the revised look:

- Stable font such as Montserrat or Inter.
- Dynamic centered captions.
- No artificial dark overlay or vignette layered on top of a scene.
- Images at full opacity.
- Brightness/contrast correction is optional and only for genuinely underexposed renders — do not apply it as a default "house style." Moody, low-key, or dramatic-weather scenes can stay as generated when that lighting fits the beat. If a render truly is underexposed (not stylistically moody, just badly lit), a reasonable starting filter is `brightness(1.08-1.12) saturate(1.03-1.05)`.
- Clean SVG stroke, no shadow.
- First scene visible from frame zero; do not rely on a fade-in from `opacity: 0` at `t=0`.

### Top Context Bar

Every history short must include a top horizontal context bar for orientation. This is not optional.

Required:

- A thin horizontal rule near the top of the 1080x1920 frame.
- Compact uppercase metadata text above or aligned with the rule.
- The metadata must explicitly state the event's place and date/year. Use the most useful compact form, e.g. `CONSTANTINOPLE · 1453`, `S-80 ISAAC PERAL` / `2013 · SPAIN`, `CITIGROUP · USA` / `2020`, or `GERMANY · 1917`.
- If both the specific place and larger country/region matter, include both when space allows: `BALAKLAVA · CRIMEA` / `1854`.
- The top bar must be visible from `t=0` and remain visible through the very last frame. The loop junction needs it in the same position at the final frame as at `t=0`; a bar that fades near the end both signals the ending and breaks the loop.
- The top bar must sit above image scenes, grain, and transitions, but below or away from the central caption area so it never collides with word captions.
- Keep it small and informational, not a title card. Do not duplicate the main title or final slogan.

Preferred HyperFrames structure:

```html
<!-- data-duration = full video duration (e.g. 40.8 for a ~41s short) so the bar reaches the loop junction -->
<div id="tag-bar" class="tag-bar clip" data-layout-allow-occlusion data-start="0" data-duration="40.8" data-track-index="13">
  <span id="meta-left" data-layout-allow-overlap>CONSTANTINOPLE</span>
  <span id="meta-right" data-layout-allow-overlap>1453</span>
</div>
<div id="rule-line" class="rule-line clip" data-start="0" data-duration="40.8" data-track-index="14"></div>
```

```css
.tag-bar {
  position: absolute;
  top: 58px;
  left: 52px;
  right: 52px;
  bottom: auto;
  width: auto;
  height: auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: rgba(247, 244, 232, 0.88);
  font-size: 22px;
  font-weight: 800;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  z-index: 18;
  pointer-events: none;
}

.tag-bar span {
  display: inline-flex;
  align-items: center;
  padding: 8px 14px;
  border: 2px solid rgba(247, 244, 232, 0.28);
  background: rgba(5, 6, 7, 0.52);
  backdrop-filter: blur(3px);
  border-radius: 2px;
}

.rule-line {
  position: absolute;
  left: 52px;
  right: 52px;
  top: 114px;
  bottom: auto;
  width: auto;
  height: 3px;
  background: linear-gradient(90deg, rgba(255,96,72,0.85), rgba(255,215,0,0.72), rgba(184,215,255,0.75));
  transform-origin: left center;
  z-index: 18;
}
```

If a custom absolute-positioned top bar also uses the shared `.clip` class, explicitly set `bottom: auto; width: auto; height: auto;` on the custom class. Otherwise `.clip { inset: 0 }` can stretch the bar to full-frame height and vertically center it in the middle of the screen.

Animate the rule and labels subtly:

```js
tl.fromTo("#rule-line", { scaleX: 0.15 }, { scaleX: 1, duration: 0.65, ease: "power3.out" }, 0.08);
tl.fromTo("#tag-bar span", { y: -18, opacity: 0 }, { y: 0, opacity: 1, duration: 0.4, stagger: 0.1 }, 0.22);
```

Record the chosen place/date string in the scene plan or edit decisions so compose has a fixed value, not an ad-hoc guess during rendering.

All on-screen words must come from the narration. Do not add unspoken title cards, persistent footers, format lines, slogans, or lower-third tags.

The signature line `One tiny mistake. Massive consequences.` and any subscribe ask are **not spoken and not shown on screen**. They live in the publish pack only (title, description, pinned comment). The last spoken words are the loop-bridge line, and nothing on screen may signal the ending (see "Loop Engineering").

## Still-Image Motion And Transitions

For still-led Shorts, approved images are not enough. The edit must feel alive.

Timing rules:

- Avoid highly uneven image holds. In a ~40 second short with 6 images, most visual beats should last roughly 5-8 seconds. The cold-open beat is the deliberate exception: it may run ~2.5 seconds because its image returns at the end for the loop.
- Do not leave a single approved still on screen for more than about 10 seconds under any treatment.
- Do not make any other image hold so briefly that the viewer cannot read the beat; outside the cold open, usually avoid holds under about 4 seconds.
- Align scene changes to narration beat boundaries, but keep the visual cadence balanced. If the word timings create one very long beat, split the motion or add an internal emphasis animation rather than leaving the image static.

Visual event rule (retention-critical):

- **Something perceptible must change at least every 3 seconds.** A frame that stays visually identical for more than ~4 seconds triggers the swipe. Qualifying events: a scene change, a Ken Burns punch-in (a quick `4-6%` scale step over `0.25-0.35s` synced to a key word and an SFX hit), a pan direction change, or a color-pop caption word.
- Punch-ins are the main tool for stretching a 6-image edit into fast-cut pacing: land them on the beat's most important word, paired with the SFX layer.
- The `Rewind` cue is a mandatory mid-video pattern interrupt: pair the spoken word with a fast reverse-zoom (or a 2-3 frame micro-glitch) plus a whoosh. It is the retention reset between the hook and the context section.

Transition rules:

- Do not use hard opacity cuts between full-frame stills. They can create visible flashes or abrupt brightness jumps in the rendered MP4.
- Use short crossfades, usually around `0.35-0.55s`, between images.
- Keep scene layers ordered so captions, toplines, date stamps, grain, and UI labels stay above both images during the crossfade.
- Review transition frames before delivery. Snapshot or contact-sheet frames just before, during, and just after every scene boundary. Confirm there is no white flash, black flash, missing image, or caption layer going behind the image.

Ken Burns rules:

- Default Ken Burns must be visible on a phone, not just technically present. A tiny `scale: 1.00 -> 1.04` often reads as static.
- Use meaningful motion per still, e.g. `scale: 1.03-1.06 -> 1.18-1.24` plus `x/y` pan of roughly `40-110px`, adjusted to avoid exposing edges.
- Vary direction by scene so the video does not feel like the same zoom repeated six times.
- Keep motion slow and continuous across each hold. Avoid abrupt resets at scene changes; hide changes inside the crossfade.

## Six-Image Beat Map

Use 6 approved images for a 38-44 second short. Default to a consequence-first flow. The opening consequence image **must** return as the final cut (a seventh visual cut reusing one of the 6 unique images) — this is what makes the loop junction invisible.

| Beat | Time | Purpose |
|---|---:|---|
| 1 | 0-2.5s | Cold open shock: strongest consequence frame + ≤8-word hook line |
| 2 | 2.5-7s | Tease the smaller cause ("it wasn't X — it was smaller"), then the `Rewind` pattern interrupt |
| 3 | 7-13s | Context: the system was still holding |
| 4 | 13-19s | Obvious large pressure; show what everyone expects to matter |
| 5 | 19-25s | System resilience + re-hook negations ("Not the walls. Not the fleet.") |
| 6 | 25-33s | Tiny mistake revealed at the 60-70% mark; disputed-history caveat after the impact |
| 7 | 33-41s | Consequence in motion → return to the opening image, Ken Burns landing on the `t=0` framing; loop-bridge line |

The reveal must never land before ~60% of the runtime — everything earlier is curiosity build. Adjust timings to actual ElevenLabs word timestamps, not the other way around, but preserve a balanced visual cadence. Treat the table as a rhythm target, not a permission to create one very short hold and one very long hold.

## Script Pattern

Use short sentences. Target 80-95 words for 38-44 seconds. Default pattern:

```text
[Huge consequence, ≤8 words, most extreme concrete fact first].
It wasn't [obvious threat]. It was something much smaller.
Rewind.
For [time], [place/system] survived [obvious pressure].
[Threat] hit/cracked/shook. The [system] held.
The defenders/operators kept patching the gaps.
Not the main [thing]. Not a grand [thing].
Someone missed [specific tiny thing].
A small [thing] left [wrong state].
[Opposing force/problem] slipped through.
Panic/failure moved faster than orders.
At least, that's what one account says.
Would you have caught it?
All of it — because of one [tiny thing].
```

Structural rules:

- The reveal (`Someone missed...`) lands at 60-70% of the runtime. Everything before it builds the open loop: the early tease (`It was something much smaller.`) promises the answer, the negations (`Not the main... Not a grand...`) re-hook right before it.
- The disputed-history caveat comes **after** the reveal's impact, never before. `Then, according to one account, someone missed...` deflates the biggest moment of the video; reveal first, then `At least, that's what one account says.` Keep the caveat in the description too. Do not overclaim disputed history.
- The comment-trigger question (`Would you have caught it?` / `Would you have noticed?`) is optional but recommended: comments outweigh subscriptions in the Shorts feed.
- The final line is the **loop bridge** (see "Loop Engineering"): it must read as the setup of the opening line, not as an ending.
- `One tiny mistake. Massive consequences.` and `Subscribe for more` are never in the narration.

## YouTube Publish Pack

For every final YouTube Short, create `publish/youtube/` inside the project folder.

Required files:

| File | Purpose |
|---|---|
| `<slug>-youtube-short.mp4` | Publishable final video copied into the YouTube pack; use a speaking filename, never `final.mp4` |
| `metadata.json` | Structured upload metadata for automation or copy/paste |
| `upload-fields.md` | Human-readable title, description, tags, settings, and notes |
| `description.txt` | Description ready to paste into YouTube Studio |
| `tags.txt` | Comma-separated tags |
| `pinned-comment.txt` | First pinned comment |
| `upload-checklist.md` | Manual QA and YouTube Studio checklist |

Publish asset rules:

- Copy the final publishable MP4 into `publish/youtube/` with a descriptive kebab-case filename based on the primary title, e.g. `the-door-that-ended-rome-youtube-short.mp4`. Never leave the upload copy named `final.mp4`.
- Keep the canonical render in `renders/` too; the publish pack gets its own upload-ready copy.
- Include the publish-pack video path in `metadata.json` as `final_video_path`.

Include in `metadata.json`:

- final video path
- primary title and 3-5 alternate titles
- description and short description
- hashtags and tags
- thumbnail text options
- pinned comment
- upload settings: category, language, made-for-kids, age restriction, paid promotion, altered/synthetic content, license, embedding, remixing, visibility, playlist
- factual-safety notes
- official-policy notes when AI disclosure or platform settings matter

Metadata rules:

- Keep titles under YouTube's 100-character limit **and put the curiosity gap in the first ~40 characters** — mobile truncates early. Working formulas: `One [tiny thing] ended [huge thing]`, `The $[X] billion mistake nobody noticed`, `[Huge number] couldn't do it. One [tiny thing] did.`
- The title must never resolve the mystery: do not name the specific tiny mistake in the title or thumbnail text.
- Use 3-5 relevant hashtags only. `#Shorts` is no longer needed (YouTube classifies Shorts by aspect ratio and duration); prefer `#History`, the event, and the era or place, e.g. `#History #Constantinople #ByzantineEmpire`. Never exceed 15 hashtags — YouTube then ignores all of them.
- The first line of the description is the strongest topical signal for YouTube's NLP: event keywords plus the curiosity gap. Push credits and disclosure to the bottom of the description.
- Include the signature `One tiny mistake. Massive consequences.` in the description and/or pinned comment — it brands the format without costing runtime.
- The pinned comment must be a debate question (`Would you have noticed the open gate? Be honest.`) or a legitimate controversy prompt (`Historians still argue about this — which account do you believe?`), never a generic thanks. Comments outweigh subscriptions in the Shorts feed. Put the subscribe ask here, not in the video.
- Keep factual caveats from the script in the description, e.g. `according to one account`.
- Do not overclaim disputed historical details in title or description.
- Set `altered_or_synthetic_content` to `true` when the video uses realistic AI reconstruction, synthetic narration, or AI-generated music.
- Add a short AI/tool disclosure sentence when appropriate, e.g. `AI-generated historical reconstruction with synthetic narration and music. Created with FLUX for images, ElevenLabs for narration and music, and OpenMontage: https://github.com/calesthio/OpenMontage.`
- Default tool credits for this channel: FLUX for images, ElevenLabs for narration/music, and `calesthio/OpenMontage` with `https://github.com/calesthio/OpenMontage`.
- Use `Education` as the default category for history shorts.
- Mark `made_for_kids` as false unless the video is explicitly produced for children.

Upload checklist must include:

- final phone check after private or unlisted upload
- first-frame check: not black, and strong enough to work as the grid thumbnail
- loop check on phone: let the Short replay once; the junction must be invisible
- T-heavy caption check
- audio loudness check on phone speakers
- AI/tool disclosure in the description: AI-generated reconstruction, FLUX, ElevenLabs, and OpenMontage link
- publish-pack MP4 exists in `publish/youtube/` with a descriptive title-based filename, not `final.mp4`
- pinned comment posted **and pinned within minutes of publishing**, phrased as a question
- publish at the channel's regular time slot — consistent cadence improves cold seeding
- schedule the 48-72h analytics check (see "Post-Publish Analytics")

## Render And QA Checklist

Before final delivery:

- HyperFrames lint: zero errors and zero warnings.
- HyperFrames validate: no console errors.
- HyperFrames inspect: include `0`, the hook, tiny-mistake beat, long-word captions, consequence beat, and the loop junction (last second).
- Snapshot `0.00s` and confirm the first exported frame is not black.
- Swipe test: review only the first 2 seconds at phone size — strongest image already on screen, hook words visible, Ken Burns motion already running. If those 2 seconds would not stop a thumb, fix the hook before anything else.
- Visual-event scan: confirm no interval longer than 3 seconds without a perceptible visual change (scene change, punch-in, pan direction change, or color-pop word).
- Loop test: play the last second and the first second back to back, twice. The junction must be invisible — matching framing, no fade-out, no audio tail, top bar unmoved.
- Dead air: narration gaps stay tight throughout, and the audio tail after the last word is ≤0.3s.
- SFX sync: every riser/impact/whoosh lands within ±0.05s of its visual event; no SFX inside the last 0.5s.
- Snapshot `0.00s` and confirm the top context bar is visible, near the top, horizontal, and states place plus date/year.
- Snapshot at least one hot, gold, and cool emphasized caption. Confirm all colors read clearly over the image and still have a clean outline.
- Snapshot or contact-sheet every image transition: just before, during, and just after the boundary. Confirm there are no white/black flashes and captions stay above images during crossfades.
- Confirm still-image motion is visibly active on phone-sized review: pan and zoom should be noticeable but not frantic.
- Snapshot T-heavy captions such as `THE`, `NOT THE`, `TINY`, and `THE KERKOPORTA`; confirm the vertical outline is continuous.
- Check no text clipping.
- Check the top context bar does not overlap central captions and does not render vertically centered in the frame.
- Check no artificial dark overlay/vignette layered on top of a scene (naturally moody or low-key source images are fine).
- Check no text shadow/ghosting.
- Check the caption entrance effect matches the reference pop: quick upward/back-eased word entry, then upward fade-out.
- FFprobe final MP4: 1080x1920, 30 fps, H.264 video, AAC stereo audio.
- Loudness: around `-16 LUFS`, true peak near or below `-1.5 dBTP`.
- Caption count: all spoken words represented, max 3 words per group.
- If local transcription is unavailable, explicitly record that ElevenLabs native timestamps were used for word-sync verification.

## Post-Publish Analytics

The pipeline does not end at upload. Check YouTube Studio 48-72 hours after publishing and use the numbers to steer the next video:

- **Viewed vs Swiped Away**: low → the first 2 seconds failed. Fix the hook image and the first spoken line, not the middle of the video.
- **Retention at the 3-second mark and average retention**: target above 65%; viral Shorts average around 76%. A mid-video cliff means missing re-hooks or a reveal placed too early.
- **Average percentage viewed above 100%**: the loop works — identify what the junction did and protect it as an invariant.
- **Engaged views vs views**: a large gap means scroll-bys — again a hook problem, not a content problem.
- **Comments**: weighted heavier than subscriptions in the Shorts feed. If low, sharpen the pinned question and the spoken comment trigger.

Record findings in `publish/youtube/post-publish-notes.md` (metric snapshot + one hypothesis + one change for the next video) so each production starts from data instead of intuition.

## Common Mistakes

| Mistake | Fix |
|---|---|
| TTS too long, then FFmpeg speed-up | Revise script or set ElevenLabs speed before generation |
| Captions miss spoken words | Use ElevenLabs `with-timestamps` and generate groups from every word |
| Text looks like subtitles | Use large centered 1-3 word groups |
| Captions feel visually flat | Keep most words off-white, but color key danger/consequence words hot, tiny-mistake/reveal words gold, and system/process words cool |
| Image has an artificial dark overlay/vignette baked into the prompt or CSS | Remove the overlay/vignette; a naturally moody or low-key generated image does not need brightness correction |
| A render comes out genuinely underexposed or muddy, not stylistically moody | Correct only that case with a light filter such as `brightness(1.08-1.12) saturate(1.03-1.05)`; do not apply this as a default house style to every image |
| Text has ugly duplicate shadow | Set `text-shadow: none`; use clean stroke |
| T letters lose vertical outline | Do not use CSS `-webkit-text-stroke` for final captions; use inline SVG text with stroke/fill and visible overflow |
| Short words spread too far apart | Use dynamic SVG widths for every word, not a fixed wide viewBox; reduce row gap and never group across sentence-ending punctuation |
| Words touch after compacting short-word spacing | Increase SVG box padding/width, not only the flex gap; check `MORE THAN THREE`, `CAME FROM AN`, and `ON IT` as regression frames |
| Signature line or `Subscribe for more` spoken or shown on screen | Neither belongs in the narration or on screen: they live in title/description/pinned comment only. The last spoken line is the loop bridge |
| Missing top context bar | Add a top horizontal rule plus compact place/date metadata visible from `t=0`, e.g. `CONSTANTINOPLE · 1453` or left/right labels such as `CITIGROUP · USA` and `2020` |
| Top context bar lacks place or date | Revise the metadata text so it includes both orientation facts: where the event happened and when it happened |
| Top context bar competes with captions | Keep it small at the top (`top` around 58px, rule around 114px), with central captions vertically centered and never overlapping it |
| First frame is black | Make the first scene visible in CSS and set it visible at timeline `0`; do not fade in from opacity 0 at frame zero |
| Hook takes 5 seconds to land | The swipe decision happens in ~1.5-2s: strongest image at `t=0`, ≤8-word shock line, first caption within 0.3s, then `Rewind` to earn the context |
| Image transitions flash | Replace hard cuts with short crossfades and review before/during/after frames at every boundary |
| Some images stay up forever | Rebalance scene timings so most stills hold about 5-8 seconds; split long beats with motion or emphasis |
| Static hold with no visual event for >4 seconds | Add a punch-in, pan direction change, or color-pop word — something perceptible at least every 3 seconds |
| Video signals its own ending (long music fade, spoken CTA, silence pad) | Loops die. End on the loop bridge with ≤0.3s audio tail and the final frame matching `t=0` |
| Tiny mistake revealed too early | Hold the reveal to 60-70% of runtime; re-hook with negations (`Not the walls. Not the fleet.`) before it |
| Caveat placed before the reveal deflates it | Reveal first, then `At least, that's what one account says.` — never `according to one account, someone missed...` |
| No SFX layer | Add riser + impact + whooshes synced to visual events within ±0.05s; volume 0.25-0.35, never over voice, none in the last 0.5s |
| Pinned comment is a generic thanks | Make it a debate question, posted and pinned within minutes; comments outweigh subscriptions in the Shorts feed |
| Hashtag spam or `#Shorts` reflex | 3-5 relevant hashtags; more than 15 disables all of them; `#Shorts` is no longer needed for classification |
| Pipeline stops at upload | Run the 48-72h analytics check (swipe ratio, 3s retention, loop %, comments) and record one change for the next video |
| Ken Burns feels static | Increase scale delta and pan distance enough to read on a phone, e.g. end scale around 1.18-1.24 with varied x/y pan |
| Missing upload metadata | Create `publish/youtube/` with the descriptive upload MP4, metadata JSON, upload fields, description, tags, pinned comment, and checklist before final handoff |
| Publish video is named `final.mp4` | Copy/rename the upload-ready file to a title-based name such as `<slug>-youtube-short.mp4` inside `publish/youtube/` and point metadata to that path |
| Caption motion feels flat or just fades | Use the `the-door-that-ended-rome` HyperFrames/GSAP effect: group `power2.out` pop-in, per-word `back.out(1.9)` from low/small to normal, group `power2.in` upward fade-out |
| AI/tool disclosure skipped | Set altered/synthetic content to true and include disclosure plus tool credits in the YouTube description: FLUX, ElevenLabs, and OpenMontage link |
| Audio feels weak | Check loudness and normalize to Short-ready level |
| Captions spell out numbers/dates as words ("NINETEEN SEVENTY-SIX", "THREE HUNDRED BILLION") | Convert to digits for the caption only (`1976`, `$300 BILLION`); merge the underlying spoken-word run into one timed caption token. Audio keeps the natural spoken form. See "Number And Date Display". |
| Generating more than 6 images without asking | Stop at 6. Ask the user before generating any additional images. |
| Asking the user to pick an image model at the start, or generating the hook image with `FLUX dev` | The split is fixed policy: `FLUX pro` for the beat-1 hook image (swipe-stopper + thumbnail), `FLUX dev` for the other 5. No opening question. |
| Regenerating approved images | Freeze approved images and build with them |
| Blocker check-in gets no response (e.g. provider out of credit) | Do not stall and do not regenerate/spend more/swap providers on silence. Finish the pipeline with what's already generated and paid for; flag the specific issue in decision log, `final_review.json`, and `upload-checklist.md`. See "Blocker And Non-Response Handling". |
| SVG text stacks vertically ("A" above "M") | SVG element has no explicit `width` — browser defaults to 300px, scaling a narrow viewBox by 3-4× and causing word-level flex wrapping. Fix: one SVG per display line with explicit `width="960" height="{h}"` attributes always set. |
| Captions overflow or are too large | Using single-line layout for 3-word groups. Fix: always use 2-line layout for 3-word groups; use 2+1 if `est(first 2 words) ≤ 900px`, else 1+2. |
| Captions positioned at bottom like subtitles | Using `top: Npx` or `bottom: Npx` static positioning. Fix: `top:0; bottom:0; flex-direction:column; align-items:center; justify-content:center` for true vertical centering. |
| ffmpeg loudnorm fails in PowerShell (exit 183) | `-f null NUL` on Windows returns ERROR_ALREADY_EXISTS. Fix: always run loudnorm via Bash (Git Bash / WSL) with `-f null /dev/null`. |
| Audio sample rate doubles to 96 kHz after normalization | Re-encoding audio without specifying sample rate. Fix: always add `-ar 48000` to the ffmpeg normalization command. |
| A long-word pair (e.g. `CODEBREAKERS DECODED`) clips past the frame edge | The sizing formula's floor of 76px doesn't guarantee fit once the target ceiling is corrected to the real 960px container. Fix: use the 960px-target formula above and split the offending 2-word line into two 1-word lines if it still overflows at the 60px floor. |
| First frame is black even though CSS sets the first scene to `opacity:1` | HyperFrames' native `data-start`/`data-duration` clip system hides an element until its own `data-start`, overriding CSS opacity. If the first image's `data-start` is set to the narration's first word timestamp (e.g. `0.046`) instead of `0`, the frame is black until that moment. Fix: force the first scene's `data-start="0"` regardless of when the first word begins. |
| Mid-video frame goes black between two scene images | Deriving each cut's start/end directly from word timestamps leaves the natural silence between sentences (up to ~0.5s) as a gap where neither image's clip window is active. Fix: make cuts contiguous (`cut[i].end = cut[i+1].start`) and extend each image's clip `data-duration` past its nominal end by the crossfade length (~0.5s) so the outgoing image stays clip-visible long enough to actually fade out. |
| A top-bar element (date stamp, title/location label, etc.) renders vertically centered in the middle of the screen instead of near the top | The element's class list combines a custom positioned class (e.g. `.top-meta`, `.air-data`) with the shared `.clip` utility class used by the HyperFrames timeline system, which sets `position:absolute; inset:0` (top/right/bottom/left all `0`). If the custom rule only overrides `top`/`left`/`right`, `bottom:0` from `.clip` still applies, so the box stretches from `top:Npx` all the way to the frame bottom — and `align-items:center` then centers the label in the middle of that tall box, not near the top. Fix: any rule that combines a custom absolute-positioned class with `.clip` must explicitly set `bottom: auto; width: auto; height: auto;` alongside its `top/left/right`, so the box shrinks to its content height instead of inheriting `.clip`'s full-frame `inset`. Verify with a `hyperframes snapshot` at `t=0` before rendering — a mispositioned top bar is visible immediately. |
