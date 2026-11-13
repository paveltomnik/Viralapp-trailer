# BrainAid App Trailer — ad Reels from a website link

**A skill for Claude Code (and Codex): give it a website link, get a selling vertical ad (1080×1920) with sound,
voice and editing.** It works like an agency: product analysis → three different ad ideas with real key frames →
you pick → storyboard with automated checks → you approve → voice, music, edit → the finished MP4 is verified.
Nothing paid runs before you approve.

[Русский README](README_RU.md) · BrainAid AI News · version 0.9.0 · MIT

<p align="center"><img src="docs/examples/brainaid-trailer.gif" width="270" alt="BrainAid trailer parody made with the skill"></p>

## Example: "Starring — you" (BrainAid)

`https://brainaid.ru` → the skill found the bot's real results on the site (trend videos, photos, the source selfie) →
three ideas → a blockbuster-trailer parody was chosen → storyboard → approval → a 28 s ad.

| Three concepts with key frames | Storyboard before any spend | Frames of the final MP4 |
|---|---|---|
| ![](docs/examples/brainaid-concepts.jpg) | ![](docs/examples/brainaid-storyboard.jpg) | ![](docs/examples/brainaid-trailer-frames.jpg) |

## What it does

- **Captures the site**: pages, real elements (buttons, prices), palette, fonts, photo and video proof.
- **Three concepts**, different ideas and looks, framed with the real material, with a recommendation.
- **Two editing modes**:
  - *script mode* — the ad is made of the site and its interface (apps, services, SaaS): 6 motion styles, hook / zoom /
    scroll / cards / number / cta scenes, **HyperFrames** or **Remotion**;
  - *edit-list mode* (`spot`) — the ad is cut from **real footage and photos**: shots with source moments, slow motion,
    a grade per shot, cuts on the music hits, motivated transitions (flash, anamorphic streak, dip, glass panel along a
    frame line, three rising panes, focus pull), the live site in a phone with a tap on the real button, a source photo
    bursting into its results, a frosted-glass end panel, letterbox and grain, music that stops dead at the turn,
    voice lines fitted to their windows.
- **Checks**: type inside the frame and out of the Reels UI zones, overlaps, duration, frames, −14 LUFS, peaks, black
  and frozen frames, audio gaps, motion, every voice word recognised in the final mix.
- **Honesty**: every number and claim comes from the site (the source is recorded); synthetic voice is labelled; music
  is generated or your own licensed track.

## Install

Requires **Node.js 22+**, **FFmpeg** (with ffprobe), Git. Chrome is downloaded automatically (HyperFrames).

```bash
git clone <this repository URL> brainaid-app-trailer
cd brainaid-app-trailer
python install_skill.py --target claude --scope user
```

Without Python: copy the folder to `~/.claude/skills/brainaid-app-trailer`. For Codex: `--target codex`.

## Keys — your own, in `.env`

Only **voice** and **music** cost money; capture, concepts, storyboards, render and checks run without keys. Every ad
project gets a `.env` file with hints — **open it and paste your own key** for one of the services:

| Service | Line in `.env` | Gives | Key |
|---|---|---|---|
| **OpenRouter** | `OPENROUTER_API_KEY=` | Gemini TTS voice, Google Lyria music | https://openrouter.ai/keys |
| **kie.ai** | `KIE_API_KEY=` | Gemini TTS voice, Suno V6 music (credits; see Billing for payment methods) | https://kie.ai/api-key |

Never paste a key into the chat; `.env` is git-ignored and keys are never printed. Check with `npm run keys`.
Typical cost per ad: voice ≈ $0.01–0.03, music ≈ $0.06–0.08 per track; all spend goes to `work/spend.json`, capped by `budgetUsd`.

## Use

Open Claude Code anywhere and write: *"Make an ad video for https://example.com — goal: sell …"*. Claude scaffolds
the project, captures the site, shows three concepts and the storyboard, and asks for your approval.
Manual run: `node bin/new.mjs --url https://site --out ./ad-site --brand Name --lang en`, then `npm install`,
`npm run keys`, `npm run capture` — see [SKILL.md](SKILL.md).

## Docs

[SKILL.md](SKILL.md) · [Edit-list mode](references/spot.md) · [Keys](references/keys.md) · [Footage](references/footage.md) ·
[Creative direction](references/creative-direction.md) · [Art direction](references/art-direction.md) · [Quality](references/quality.md) ·
[Engines](references/engines.md) — **Remotion** is free for individuals, companies of up to 3 people, non-profits and
evaluation; others need a Company License. HyperFrames is the default.

## Safety

Public pages of the same site only; no logins, purchases or form submissions. Other people's faces, brands and logos
stay out without rights; a client's material only with their permission. Secrets only in `.env`.

Tests: `node --test tests/*.test.mjs` (55 offline tests).
