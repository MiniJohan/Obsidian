# LORE — Build Plan

Chunked delivery. Each chunk confirmed working before the next begins.
Claude owns backend. Zon owns frontend styling.

---

## Phase 1 — Clean Core

### Chunk 1 — Backend skeleton
**Claude delivers:**
- `D:\LORE\backend\` folder structure
- `main.py` — FastAPI app, `/command` POST route, CORS for Vite dev server
- `memory.py` — load/save `lore_data/memory.json`, identity pre-seeded
- `llm.py` — `build_system_prompt()` using identity from memory, `ask_llm()` via Ollama
- `lore_data/memory.json` — pre-seeded with Zon's name, stack, OS, active projects

**Confirm:** `uvicorn main:app --reload --port 5500` starts cleanly. POST to `/command` with a text payload returns an LLM response JSON.

---

### Chunk 2 — Voice pipeline
**Claude delivers:**
- `voice.py` — Kokoro TTS, am_fenrir, sentence chunking, producer-consumer audio queue, `is_speaking` gate
- `whisper.py` — faster-whisper CUDA, base model, int8, `transcribe()` function
- `listener.py` — mic capture loop, amplitude threshold, silence detection, posts to `/command`

**Confirm:** Speak into mic → transcribed → `/command` hit → LLM responds → Kokoro speaks it back. Speaking gate prevents feedback loop.

---

### Chunk 3 — React frontend
**Claude delivers:**
- `D:\LORE\frontend\` — Vite + React scaffold
- `vite.config.js` — proxy `/api` → `http://localhost:5500`
- `App.jsx` — state machine (`IDLE` / `LISTENING` / `THINKING` / `SPEAKING`), wires to `/api/command`
- `Waveform.jsx` — canvas + Web Audio API, reacts to real mic/audio data per state
- `ResponseText.jsx` — fade-in/hold/fade-out current response
- `StatusLabel.jsx` — state label, bottom-right

**Zon owns:** All CSS — colors, layout, font, spacing. Claude delivers structure and logic only.

**Confirm:** `npm run dev` starts. UI renders on localhost:5173. Waveform reacts to mic. Response text updates when backend responds. State label tracks correctly.

---

### Chunk 4 — Launch setup
**Claude delivers:**
- `launch_lore.bat` — starts uvicorn + opens Chrome app mode to localhost:5500
- `lore_hotkey.ahk` — AutoHotkey script, global hotkey brings LORE window to focus and POSTs to `/trigger-listen` endpoint
- `/trigger-listen` endpoint added to `main.py`

**Confirm:** Double-click bat → LORE backend + UI start. Hotkey from any window brings LORE to focus and starts listening. Escape dismisses.

---

## Phase 2 onward
Planned in Roadmap. Spec written when Phase 1 is complete and stable.
