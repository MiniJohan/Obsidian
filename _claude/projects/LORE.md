# LORE — Personal Local AI Assistant

> Status: Phase 1 — Ready to build
> Renamed from: VALE (full redesign, 2026-09-01)
> Source of truth: `D:\Obsidian\Vaults\void\1. PROJECTS\LORE\`
> Code location: `D:\LORE\` (fresh start — `D:\VALE\` is old, not migrated)

---

## What It Is

Fully local personal AI assistant. No cloud. Global hotkey summons it from any window. Speak or type. It answers, speaks back, and gets out of the way. Modular capability system — each new feature is a clean Python file addition.

---

## Key Design Decisions

| Decision | Choice | Reason |
|---|---|---|
| Name | LORE | Replaced VALE — fits knowledge/growth concept; pairs well with vault named `void` |
| Frontend | React + Vite | Zon learning React; state complexity (4 app states, waveform, fade logic) suits it |
| UI trigger | Chrome app mode + AutoHotkey | No Electron overhead; plays to Zon's frontend skills |
| Waveform | Canvas + Web Audio API | Real mic/audio data, not CSS animation |
| Action system | Removed entirely | Keyboard shortcuts faster than voice; adds latency, no value |
| Three.js particles | Removed | Theatrical; not Zon's aesthetic |
| Personality ("sir") | Removed | Tool, not a character |
| TTS voice | am_fenrir (Kokoro) | Kept from VALE — it works well |
| Accent color | `#00d4d4` cold cyan | Cold, precise, fits dark minimal aesthetic |
| memory.json | Pre-seeded from day one | Personal assistant should know you before the first message |

---

## Scope of Claude's Work

**Claude owns:**
- All backend Python: `main.py`, `llm.py`, `voice.py`, `whisper.py`, `listener.py`, `memory.py`
- Capability system: `capabilities/__init__.py`, `base.py`, and each capability file
- `vite.config.js` proxy setup
- `launch_lore.bat` and `lore_hotkey.ahk`
- `lore_data/memory.json` — pre-seeded
- Build guidance per chunk

**Zon owns:**
- All CSS — colors, layout, spacing, typography
- Frontend component visual design

---

## Build State

| Phase | Status |
|---|---|
| Phase 1 — Clean core | ⏳ Ready to start |
| Phase 2 — Real memory | 🔲 Not started |
| Phase 3 — Capabilities (code + projects) | 🔲 Not started |
| Phase 4 — Vault bridge | 🔲 Not started |
| Phase 5+ — Open additions | 🔲 TBD |

---

## Decision Log

| Date | Decision | Reasoning |
|---|---|---|
| 2026-09-01 | Renamed VALE → LORE | Full redesign; name fits knowledge/growth concept |
| 2026-09-01 | Frontend: React + Vite | Zon learning React; state complexity suits it |
| 2026-09-01 | Removed action system | Slower than keyboard shortcuts; zero real-world value |
| 2026-09-01 | Removed Three.js | Theatrical; doesn't match Zon's aesthetic |
| 2026-09-01 | Capability module system | Modular by design — add a file, extend the system |
| 2026-09-01 | memory.json pre-seeded | Personal assistant should know you from day one |
| 2026-09-01 | Accent: cold cyan #00d4d4 | Cold, precise; fits dark minimal without being generic |
