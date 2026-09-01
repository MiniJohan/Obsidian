# LORE — Roadmap

Each phase ships as a working, usable system. Nothing is "in progress forever."

---

## Phase 1 — Clean Core
*Goal: The full loop works cleanly. LORE knows who you are.*

- [ ] `D:\LORE\` — fresh project, backend + frontend separated
- [ ] FastAPI `/command` POST endpoint
- [ ] faster-whisper CUDA transcription pipeline
- [ ] Kokoro TTS pipeline (am_fenrir, sentence chunking, producer-consumer queue)
- [ ] Mic listener loop with amplitude threshold + speaking gate
- [ ] React + Vite frontend — Waveform, ResponseText, StatusLabel
- [ ] Vite proxy → FastAPI (port 5500)
- [ ] Chrome app mode launch script
- [ ] AutoHotkey global hotkey — summon + trigger LISTENING, Escape to dismiss
- [ ] `lore_data/memory.json` pre-seeded with Zon's identity and active projects

**Done when:** Hotkey summons LORE → speak → transcribed → LLM responds → spoken back → UI reflects state throughout.

---

## Phase 2 — Real Memory
*Goal: LORE actually remembers across sessions.*

- [ ] Conversation history persists across restarts (trimmed rolling window)
- [ ] LORE can recall facts from previous sessions when relevant
- [ ] `remember` / `forget` fields in LLM response schema work correctly
- [ ] Memory JSON never grows unbounded (auto-trim on save)

**Done when:** Ask LORE something in one session, restart, ask about it again — it knows.

---

## Phase 3 — First Capabilities
*Goal: LORE adjusts its context to what you're asking.*

- [ ] `code.py` — detects code question, injects stack context into system prompt
- [ ] `projects.py` — reads active projects from memory, discusses them intelligently
- [ ] Capability registry auto-loads at startup — adding a file = adding a capability

**Done when:** Ask a React question → LORE knows your stack. Ask about SongPWA → it knows the status.

---

## Phase 4 — Vault Bridge
*Goal: LORE reads your Obsidian vault. Knows what you're working on without being told.*

- [ ] `vault.py` capability reads `_claude/active.md` at startup
- [ ] LORE has current project status baked into context
- [ ] Optional: LORE can write decisions/notes back to vault

**Done when:** Start LORE → it already knows you're in the middle of SongPWA Phase 1, without you saying anything.

---

## Phase 5+ — Open
*Add when ready. Each is one capability file.*

- Weather (local API call)
- System info via `psutil`
- Volume control via `pycaw`
- Note-taking to vault
- Homelab split architecture (always-on mini PC + main GPU via network request)
- Personality tone modes
