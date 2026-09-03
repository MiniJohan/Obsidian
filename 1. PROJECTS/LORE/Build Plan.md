# LORE — Build Plan

Chunked delivery. Confirm each chunk working before the next begins.
Claude owns backend logic. Zon owns all frontend styling.

---

## Status
> **Current chunk:** 1 — Backend skeleton
> **Next step:** Write `llm.py`, then `main.py`
> **Blockers:** None

---

## Phase 1 — Clean Core

### Chunk 1 — Backend skeleton
- [x] `lore_data/memory.json` — pre-seeded with identity, projects, empty facts/history
- [x] `memory.py` — load, save, add_fact, remove_fact, append_history
- [ ] `llm.py` — build_system_prompt, build_messages, ask_llm via Ollama
- [ ] `main.py` — FastAPI app, /command POST, /health GET, CORS, lifespan startup
- [ ] **Confirm:** `uvicorn main:app --reload --port 5500` starts. POST /command returns LLM response JSON.

---

### Chunk 2 — Voice pipeline
- [ ] `voice.py` — Kokoro TTS, am_fenrir voice, sentence chunking, producer-consumer audio queue, is_speaking gate
- [ ] `whisper.py` — faster-whisper CUDA, base model, int8, transcribe() function
- [ ] `listener.py` — mic capture loop, amplitude threshold, silence detection, posts to /command
- [ ] **Confirm:** Speak → transcribed → /command hit → LLM responds → Kokoro speaks it back. Speaking gate prevents feedback loop.

---

### Chunk 3 — React frontend
- [ ] `D:\LORE\frontend\` — Vite + React scaffold
- [ ] `vite.config.js` — proxy /api → http://localhost:5500
- [ ] `App.jsx` — state machine (IDLE / LISTENING / THINKING / SPEAKING), wires to /api/command
- [ ] `Waveform.jsx` — canvas + Web Audio API, reacts to real mic/audio data per state
- [ ] `ResponseText.jsx` — fade-in/hold/fade-out current response
- [ ] `StatusLabel.jsx` — state label, bottom-right
- [ ] **Zon:** Style everything — colors, layout, font, spacing
- [ ] **Confirm:** `npm run dev` starts. Waveform reacts to mic. Response text updates. State label tracks correctly.

---

### Chunk 4 — Launch setup
- [ ] `launch_lore.bat` — starts uvicorn + opens Chrome in app mode to localhost:5500
- [ ] `lore_hotkey.ahk` — global hotkey brings LORE to focus, POSTs to /trigger-listen
- [ ] `/trigger-listen` endpoint added to `main.py`
- [ ] **Confirm:** Double-click bat → backend + UI start. Hotkey from any window triggers listen. Escape dismisses.

---

## Phase 2 onward
Spec written when Phase 1 is complete and stable. See Roadmap.md.
