---
name: 半调案卷-16x9-有语音
description: '用半调案卷（Mole/halftone-dossier）横版 16:9、有口播无音乐做短片时使用；动画须对齐语音与外部 SRT。'
---
# 半调案卷-16x9-有语音

Reusable recipe for Lemo-Opuscar **halftone-dossier** films: landscape **1920×1080 (16:9)**, **spoken VO yes**, **music no**, animations locked to voice + external SRT.

## When to use

- User asks for `半调案卷-16x9-有语音` / this skill, or wants Mole / 半调案卷 / halftone-dossier with voiceover on 16:9.
- Do **not** use for silent cuts, vertical 9:16, or scores with BGM (make/choose another skill named `…-无语音` or a different ratio).

## Locked look & constraints

- **Style:** Lemo `halftone-dossier` — cream paper, navy / mustard / red print inks, stamps, exhibit chrome, corner halftone dots as decoration.
- **People:** redrawn (halftone) portraits only — never anime/source screenshots.
- **Picture FX off:** per-frame paper dust / TV snow (`drawFx` no-op); continuous beat pulse off. Stamp slam shakes optional.
- **Subtitles:** export `.srt` only — **do not burn** captions into frames (stamps/titles as graphic design OK).
- **Audio:** Chinese edge-tts VO (default `zh-CN-YunyangNeural`, rate about `+5%`); **no music / no score bed**.
- **Pace:** every title pop, stamp, wipe, and scene cut lands on spoken phrases (use TTS durations + word timings).

## Pipeline (Lemo-Opuscar)

1. Load Cursor skill `lemo-opuscar-2`; run setup; set `LIB` / `LEMO_OPUSCAR_HOME`.
2. Project under `/workspace/<slug>/` with `index.html` exposing `window.DUR`, `window.render(t)`, `window.READY`.
3. Write `voice/lines.json` from the user’s口播稿 (`id`, `text`, optional `say` for years/numbers); keep subtitle `text` as written.
4. `python $LIB/core/tts/tts_zh.py voice/lines.json voice/` then `asr_check.py --lang zh`; fix or set `asr` overrides when listen-OK.
5. Build `placements` / `cues.json` from `dur.json` (+ short gaps between lines; slightly longer before major section opens).
6. Retimeline `index.html` scenes and all local anim times to placements; set `DUR` from total VO length.
7. `python $LIB/core/render/srt.py cues.json <name>.srt` (external only).
8. Render silent picture (`build.sh video` / `video.mjs` @ 30fps 1920×1080).
9. Assemble VO wav from line clips at `t0`; mux with `sh $LIB/core/render/mux.sh silent.mp4 voice.wav final.mp4 30 0` (**grain 0**).
10. Deliver final mp4 + srt; update TREATMENT / NOTES / CREDITS (credit Edge TTS voice).

## Reference implementation

Prior working project pattern: `/workspace/kenshin-part3-mole-style/` (do not hardcode that topic’s script into this skill — copy structure and FX rules only).

## Naming sibling skills

Follow `风格名-比例-有无语音`, e.g. `半调案卷-9x16-有语音`, `半调案卷-16x9-无语音`.
