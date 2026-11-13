---
name: brainaid-app-trailer
description: Turn a real website or web app into a selling, viral-style vertical ad (Reels/TikTok/Shorts, 1080x1920) the way an agency would - analyse the product, propose three different advertising concepts with real key frames, let the user pick, show a full visual storyboard with automated technical checks, and only after approval voice, score, animate (Remotion or HyperFrames) and verify the MP4. Use when asked for an app/product/shop/venue ad, promo video, trailer or Reels from a URL, "сделай рекламный ролик по сайту", website-to-video, or to fix/improve such a video. Brand-specific art direction (no stock phone-and-gradient look), six motion styles with their own effects, real photos/results from the site, nothing invented.
compatibility: Claude Code and OpenAI Codex. Node.js 22+, npm, FFmpeg/ffprobe. Chrome is fetched by HyperFrames and reused by Remotion. Voice and music need the user's own key in the project's .env — OPENROUTER_API_KEY (OpenRouter: Gemini TTS, Lyria) or KIE_API_KEY (kie.ai: Gemini TTS, Suno); without keys use own recordings/track.
metadata:
  author: BrainAid AI News
  version: '0.9.0'
---
# BrainAid App Trailer

Work like a creative agency, not like an effects generator:
**URL → product analysis → three concepts with key frames → the user picks → storyboard + technical checks →
the user approves → voice, music → MP4 → verification.** Nothing paid and no render before the approval.

The ad sells **one idea** and **shows the result** (real photos, real interface, real numbers from the site), builds to
**one climax** and lets calm moments breathe. Effects are a budget, not a goal. Every claim, number and picture comes
from the captured site; when the site has no proof of results, ask the user for real material instead of inventing it.

## The process (run inside the generated project)

| Step | Who / command | Output |
|---|---|---|
| 0. Scaffold | `node <skill>/bin/new.mjs --url https://site --out ./ad-site --brand Name --lang ru --engine auto` | independent project |
| 1. Install | `npm install` (+ once: `npx hyperframes models install parakeet` for word timings) | |
| 1b. Keys | `npm run keys`. If a key is missing, **ask the user to open `.env` in the project and paste their own key** (OpenRouter or kie.ai — links are inside the file). Never ask for the key in chat, never print, log or commit it | `.env` (created by `new.mjs` from `env.example`) |
| 2. Capture | `npm run capture` | `capture/`: page shots, `captures.json` (elements, palette, logo, **proofs**), `proofs-sheet.jpg` |
| 2b. Footage | real video of the work (the client's files, their public channel — with permission): list it in `clips.json` (`id`, `file`, `source`), `npm run clips` | `capture/clips-sheet.jpg`: a frame per second of every clip, to choose moments |
| 3. Analyse | **you**: read the page text, look at `capture/proofs-sheet.jpg`, `capture/clips-sheet.jpg` and the shots | what is sold, to whom, what proves it |
| 4. Concepts | **you** write `concepts.json` (3 concepts), then `npm run check:concepts` and `npm run concepts` | `out/concepts.jpg`, `out/concepts/<id>.jpg` — real key frames of each idea |
| 5. Pick | **the user** chooses; `npm run pick -- B` | `script.json`, `brief.json`, `art.json`, style in the config |
| 6. Storyboard | `npm run storyboard` (free: timing estimated from the text) | `out/storyboard.jpg` (every shot), `out/qa.json`, `out/critique.md` |
| 7. Review | **you**: fix what the checks and your eyes found, re-run 6; fill `out/critique.md` as an *opinion* | |
| 8. Approve | **the user** says yes in their own words → `npm run approve -- --by "<name>"` | `review.json` (unlocks paid steps for exactly these inputs) |
| 9. Voice, music | `npm run voice`, `npm run music` (state the cost first, ledger `work/spend.json`) | real voice timings, track |
| 10. Build, render | `npm run build`, then `npm run render` (`-- --engine remotion|hyperframes`) | `out/ad-<engine>[-<style>].mp4` |
| 11. Verify | `npm run verify` | `out/verification-*.json`, contact sheet; watch it with sound |

`voice`, `music` and `render` refuse to run until `review.json` matches the current script/brief/art/style
(`--force-unapproved` exists only for when the user explicitly says to skip the approval). Changing the script after
approval locks them again — show the new storyboard. Concept previews (`npm run storyboard -- --concept A`) never unlock anything.

## Two modes — choose by the material

- **Script mode** (the table above): the ad is built from the site itself — page, interface, real elements, a few photos.
  Best for apps, SaaS, services whose proof is the interface.
- **Edit-list mode** (`spot/`, [references/spot.md](references/spot.md)): the ad is cut from **real footage and photos**
  (the client's work, results a product made, the site's own videos) like an editor would: a shot list with sources
  and moments, slow motion, a grade per shot, cuts on the music's hits, motivated transitions (flash, anamorphic
  streak, dip, glass panel along a frame line, three rising panes, rise, focus pull), typed overlays (titles, a row on
  the beat, the real site in a phone with taps and highlights, a source picture that bursts into results, a frosted
  glass end panel, a pill), music cues that can stop dead, voice lines fitted to their windows. Best for physical
  products, venues, services with visual results, generators of photo/video.

Edit-list flow: `npm run spot:strips -- capture/…` (a frame per second of every video) → **three concepts**
(`spot/concepts.json` → `npm run spot:concepts` → `out/concepts.jpg`) → the user picks → write `spot/spot.json` →
`npm run spot:site` (real mobile screens) → `spot:check` → `spot:build` → `spot:sheet -- comp …` (storyboard, free) →
fix → **the user approves** → `spot:approve -- --by "<name>"` → `spot:voice`, `spot:music` (paid, state the cost) →
retime to the real voice and the music hits → `spot:build` → `spot:render` → `spot:qa -- --speech` → look at the MP4 frames.
Approval covers the creative content (pictures, words, voice lines, music briefs), not timings — retiming stays unlocked.

## Step 4 — three concepts (the part that makes it an ad)

Read [references/creative-direction.md](references/creative-direction.md) first. Each concept in `concepts.json` has:
`id`, `name`, `idea` (one sentence a viewer could retell), `angle` (offer, manifesto, community, transformation,
enemy, demo, ritual…), `why` (the marketing logic: whose barrier it removes, why it is remembered), `brief`
(audience, problem, promise, emotion, tone, cta, hero proof + why), `style`, `art`, `script`, optional `keyframes`.
The three must be **different ideas with different looks** — `check:concepts` rejects the same angle or the same
style + art direction twice. Show the user `out/concepts.jpg` with your recommendation and why; let them choose.

## Art direction — per brand, never a template

`art` decides what the ad is made of; the style decides how it moves. Knobs ([references/art-direction.md](references/art-direction.md)):
`device` phone|screen · `camera` tilt|flat · `bg` style|solid|photo (+`bgProof`) · `palette` style|site ·
`mask` none|hex|round · `transitions` style|cut · `captions` karaoke|quiet|off · `grain` on|off · `end` minimal|style ·
`typeFrom` (borrow another style's type). Derive it from the brand: its colours (the brand's own dark colour becomes the
background), its shapes (a honeycomb brand gets hexagon frames and a honeycomb mosaic), its material (a photo-led
venue does not need a phone at all). `deriveArt()` in `scripts/art.mjs` proposes a start and explains it.

## Motion styles (how it moves)

| id | its own moves | best for |
|---|---|---|
| `neon-hype` | speed-line punch, smash + neon echo, laser scan, neon tube sticker, neon-sign end. The only style that glows. | viral promo, price shock, AI services |
| `brutal-pop` | colour-panel wipe, slap/stamp letters, bouncing arrow, starburst badge, stacked cards, comic end | consumer apps, offers, Gen-Z |
| `lux-editorial` | light leak, mask/tracking reveals, hairline underline, magazine caption, gallery, gold frame | premium, B2B, experts, manifesto |
| `terminal-dev` | boot-screen cut, typing/decoding, crosshair, terminal windows, digital rain, `$` end | dev tools, SaaS, AI agents |
| `y2k-chrome` | star wipe, jelly + chrome shine + flare, sparkles, holo flip, OS windows, marquee | creative/AI art, fashion, youth |
| `paper-collage` | torn-paper wipe, cut-out letters, marker circle + note, polaroids, doodles, scrapbook | community, education, lifestyle |

`npm run styles` still builds the same ad in every style (`out/styles-board.jpg`) when the user wants to compare looks.

## Scenes (script beats)

| scene | what the viewer sees | fields |
|---|---|---|
| `hook` | huge lines over the real page — or over a real photo with `photo: "p1"` | `lines`, `accent`, `at`, `evidence`, `photo` |
| `slam` | words on the background; optional strike and a second group | `lines`, `strike`, `at`, `second:{lines,at}` |
| `footage` | **real video**: pieces of the clips as hard cuts on spoken words (stabilised, one grade, 9:16, slow push), optional big `lines` in a darkened `textZone` (top/bottom), labels; may be the first beat (the hook over real video) | `shots:[{clip, from, at, label, rate}]`, `lines`, `accent`, `textZone` |
| `result` | **the product's result**: a real photo full width (band) or full bleed, slow push, editorial lower third; optional before/after wipe | `proof`, `caption`, `compare`, `layout`, `at` |
| `montage` | real photos as hard cuts with labels, or `layout: "mosaic"` — they gather into one shape (a honeycomb with `mask: hex`) | `proofs[]`, `labels[]`, `ats[]`, `layout` |
| `hold` | a calm pause: one line, no effects — rhythm contrast before the call | `line`, `sub` |
| `reveal` | the interface flies in, logo on the brand word, chips | `chips`, `at` |
| `zoom` | the camera flies to real elements: outline, spotlight, label | `targets:[{find, at, label}]` |
| `scroll` | fast scroll through the real page with section labels | `from`, `to` |
| `cards` | real cards cut from the page | `cards:[...]`, `label`, `at` |
| `number` | a real number counts up, with the source card | `value`, `label`, `find`, `at` |
| `steps` | numbered list | `items:[{text, at}]` |
| `cta` | the real button pressed, then the end card (minimal by default) | `button`, `line`, `at`, `endcard` |

Every beat may carry `climax: true` (exactly one usually), `energy` 0–1 (the effect budget: hook and climax 1, calm
shots ≈ 0; flashes, shakes, glitches, particles and scene-change effects scale with it), `tail` (seconds to let the
moment breathe after the words) and `intent` (why this shot exists — printed on the storyboard; it also marks a
repetition as deliberate). Director rules (no same shot twice in a row, not too much interface, one climax) are
**notes**, not bans: break them on purpose and say why in `intent`. Grounding is strict: `find`/`button`/`cards` are
real elements, every number on screen is on the page, every `proof` id is a captured picture.

## Quality: measured vs judged

- **Measured** (`out/qa.json`, printed on the storyboard): text cut by the frame, text over text, text in the Reels
  interface zones, glyphs too small, low luminance contrast (median and share of the background), missing fonts or
  pictures, blank frames, reading time, too-fast cuts, length; after render `npm run verify` (codec, size, fps,
  −14 LUFS, peaks, silence/black, speech recognised). Errors block `approve` unless the user accepts them (`--accept-qa`).
- **Judged** by people: idea, hierarchy, rhythm, brand fit. Your `out/critique.md` is an opinion to discuss, never
  presented as a score of design quality. Look at the storyboard yourself and fix what you see.
  Details: [references/quality.md](references/quality.md).

## Engines — Remotion or HyperFrames, one shared pipeline

`--engine remotion` (React, `remotion/Ad.jsx`, Studio), `--engine hyperframes` (HTML/GSAP, `template/index.html`),
`--engine auto` (chooses and prints why). Both render the same `work/plan.json` with the same stylesheet and the same
director layer (energy, art direction, result/montage/hold, minimal end); storyboards and concept frames come from
the chosen engine. **Remotion licence:** free for individuals, companies with ≤ 3 employees, non-profits and
evaluation; other companies need a Company License — ask, then set `remotion.license`; `auto` stays on HyperFrames
until it is set. Comparison and details: [references/engines.md](references/engines.md).

## Delivering

Give the MP4 path, the chosen concept and why, engine and licence status, duration, what is real (pages, elements,
photos used), voice/music provenance and cost (`work/spend.json`), verification results, what you checked by eye
and what not. Honesty: synthetic voice unless recorded by the user; AI music or the user's licensed track.
Safety: public pages only, same origin; no logins, purchases or form submissions; secrets only via env or `.env` —
the user pastes their own key into `.env`; you never ask for it in chat, print it or put it in a file that is committed.
Other people's faces, brands and logos stay out of the ad unless the owner has the rights; a client's material is used
only with the client's permission and is not published elsewhere (e.g. in a public repository) without it.

## References

- [Edit-list mode (spot): shot list, transitions, overlays, sound, QA](references/spot.md)
- [Keys and paid services: OpenRouter, kie.ai](references/keys.md)
- [Real footage: clips, preparation, both engines](references/footage.md)
- [Creative direction: brief, angles, three concepts, rubric](references/creative-direction.md)
- [Art direction knobs and how to derive them from a brand](references/art-direction.md)
- [Quality: technical gate vs creative review](references/quality.md)
- [Script playbook: hooks, triggers, structure](references/script-playbook.md)
- [Motion and sound standards](references/creative-standards.md)
- [Workflow details and troubleshooting](references/workflow.md)
- [Engines: Remotion vs HyperFrames, licence, comparison](references/engines.md)
- [HyperFrames notes](references/hyperframes.md)
- [What the tests prove](references/testing.md)
