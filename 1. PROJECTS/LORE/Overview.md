# LORE — Personal Local AI Assistant

> Personal AI that lives on your machine, knows you, answers anything, and grows one capability at a time.

---

## Status
`Phase 1 — Ready to build`
Previously: VALE (scrapped and redesigned in full, 2026-09-01)

## Related Files
- [[Data Model]] — Memory schema, capability system
- [[UI & Design]] — Visual layout, waveform, interaction model
- [[Roadmap]] — 5 build phases
- [[Build Plan]] — Phase 1 chunked implementation

---

## What It Is

A fully local, voice-and-text AI assistant built for a developer desktop. No cloud. No subscriptions. Everything runs on local hardware.

LORE lives in the background as a Chrome app window (no browser UI), summoned by a global hotkey. Speak or type. It answers, speaks back, and gets out of your way.

It knows who you are, what you're working on, and grows more capable as you build it — one module at a time.

---

## Core Principles

- **Local-first** — completely free, no API costs, no cloud dependencies
- **Modular by design** — capabilities are self-contained files, add one and it's live
- **Developer-native** — knows your stack, projects, and can talk through code decisions
- **Ambient, not intrusive** — hotkey to summon, Escape to dismiss, no persistent UI noise
- **Minimal** — pure black, one accent, functional. No theatrical effects.

---

## Tech Stack

| Layer | Choice | Notes |
|---|---|---|
| Backend | FastAPI + uvicorn | Port 5500 |
| LLM | Ollama qwen2.5:14b | 100% GPU, RTX 5060 Ti 16GB |
| STT | faster-whisper (CUDA) | base model, int8 |
| TTS | Kokoro ONNX | am_fenrir voice |
| Audio | sounddevice | mic capture + playback |
| Frontend | React + Vite | replaces old vanilla UI |
| Memory | JSON (lore_data/) | pre-seeded with identity |
| Trigger | Chrome app mode + AutoHotkey | global hotkey, no taskbar noise |

---

## File Structure (Target)

```
D:\LORE\
├── backend/
│   ├── main.py               — FastAPI app + /command route
│   ├── llm.py                — system prompt builder, ask_llm()
│   ├── voice.py              — Kokoro TTS pipeline
│   ├── whisper.py            — faster-whisper CUDA transcription
│   ├── listener.py           — mic capture loop + speaking gate
│   ├── memory.py             — load/save/update lore_data/memory.json
│   ├── capabilities/
│   │   ├── __init__.py       — capability registry, loaded at startup
│   │   ├── base.py           — BaseCapability class
│   │   ├── general.py        — catch-all, always active
│   │   ├── code.py           — code + stack questions (Phase 3)
│   │   └── projects.py       — active project context (Phase 3)
│   └── lore_data/
│       ├── memory.json       — identity + facts + history (pre-seeded)
│       └── log.json
└── frontend/
    ├── src/
    │   ├── App.jsx           — state machine: IDLE/LISTENING/THINKING/SPEAKING
    │   ├── components/
    │   │   ├── Waveform.jsx  — canvas, Web Audio API, state-reactive
    │   │   ├── ResponseText.jsx — fade-in/out, current exchange only
    │   │   └── StatusLabel.jsx  — state label, bottom-right
    │   └── hooks/
    │       └── useAudio.js   — mic + TTS audio management
    ├── index.html
    └── vite.config.js        — proxy /api → localhost:5500
```

---

## What Was Cut From VALE

| Feature | Decision | Reason |
|---|---|---|
| Action system (open app/URL) | Removed | Keyboard shortcuts faster; voice adds latency not value |
| Three.js particle background | Removed | Theatrical, not functional, not Zon's aesthetic |
| "sir" personality / butler tone | Removed | LORE is a tool, not a character |
| Vanilla HTML/CSS/JS frontend | Replaced with React + Vite | State complexity needs proper management |
| personality.json tone system | Simplified | Direct + competent by default, no modes |
