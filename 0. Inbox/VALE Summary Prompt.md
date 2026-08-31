### VALE — Local AI Assistant · Project State Summary

#### What This Is

VALE is a fully local, voice-controlled home AI assistant built in Python. No cloud dependencies — everything runs on the local machine. The user's name is Zon (frontend dev background, expanding into Python/backend). Hardware: Windows PC, RTX 5060 Ti 16GB GPU, VS Code.

---

#### File Structure

```
D:\VALE\
├── brain.py          — FastAPI app + all routes (port 5500)
├── actions.py        — load_actions(), execute(), open_app(), open_url(), run_command()
├── memory.py         — load_memory(), save_memory(), forget_memory(), write_log(), _trim_and_save_history()
├── llm.py            — load_personality(), build_system_prompt(), ask_llm()
├── voice.py          — speak(), split_into_chunks(), is_speaking gate, Kokoro TTS instance
├── whisper.py        — transcribe() via faster-whisper CUDA
├── listener.py       — mic capture loop → posts to /command endpoint
├── actions/
│   └── actions.json  — trigger-action definitions
├── UI/
│   ├── index.html
│   ├── style.css
│   └── script.js
└── vale_data/
    ├── memory.json
    ├── log.json
    └── personality.json
```

---

#### Tech Stack

|Layer|Tool|Notes|
|---|---|---|
|Backend|FastAPI + uvicorn|Port 5500, reload=True|
|LLM|Ollama qwen2.5:14b|100% GPU, RTX 5060 Ti|
|TTS|Kokoro ONNX|Voice: am_fenrir, sentence chunking, producer-consumer audio queue|
|STT|faster-whisper|base model, device=cuda, compute_type=int8|
|Audio|sounddevice|Mic capture + playback|
|Memory|JSON files|vale_data/ folder|
|UI|Vanilla HTML/CSS/JS|Dark minimal, monospace|

---

#### How It Works — Full Pipeline

```
Mic → listener.py (amplitude threshold 0.015, silence limit 1.5s)
    → whisper.py transcribe() [CUDA]
    → POST /command
    → brain.py handle_command()
    → llm.py ask_llm() [Ollama, qwen2.5:14b]
    → if action: execute() → open app/url/command
    → if chat: conversational response
    → voice.py speak() [Kokoro, am_fenrir]
    → memory.py write_log() + save/forget memory
    → JSON response → UI
```

Speaking gate (`is_speaking` flag in `voice.py`) pauses mic capture while VALE is speaking to prevent feedback loops.

---

#### Key Configuration

- **Port:** 5500
- **Ollama model:** qwen2.5:14b (9.5GB VRAM)
- **Kokoro voice files:** `D:\VALE\voice\kokoro-v1.0.onnx` + `voices-v1.0.bin`
- **Kokoro voice:** am_fenrir, speed=1.0, volume=0.9
- **Whisper:** base, cuda, int8
- **Mic amplitude threshold:** 0.015
- **Silence limit:** 1.5s
- **Max conversation history:** 20 turns (trimmed per session)
- **Log max entries:** 100 (rolling)

---

#### Action Types in `actions.json`

Three supported types:

- `open_app` — `subprocess.Popen(["cmd", "/c", "start", "", target])`
- `open_url` — `webbrowser.open(target)`
- `run_command` — `subprocess.Popen(target, shell=True)` (for apps needing full path + args like Discord)

JSON shape per action:

json

```json
{
  "id": "open_chrome",
  "triggers": ["open chrome", "chrome", "launch chrome"],
  "type": "open_app",
  "target": "chrome"
}
```

---

#### Memory System

`vale_data/memory.json` shape:

json

```json
{
  "user": {
    "name": null,
    "location": null,
    "preferences": {}
  },
  "facts": [],
  "conversation_history": []
}
```

VALE writes its own memory via optional `remember` field in LLM JSON response. Forgetting via optional `forget` field. Both handled in `handle_command()` in `brain.py`.

---

#### Personality System

`vale_data/personality.json` controls tone, reference word ("sir"), action confirmation phrases, greetings, and fallbacks. Gets injected into `build_system_prompt()` on every request. Editable without restarting the server.

LLM responds in strict JSON only — two shapes:

json

```json
{"type": "action", "id": "open_chrome", "confirmation": "Sure.", "message": "Opening Chrome now."}
{"type": "chat", "message": "Here's my answer."}
```

Optional `remember` / `forget` fields added to either shape when VALE learns or forgets something.

---

#### Current Working State ✅

- Full voice pipeline (speak → hear → respond vocally)
- Intent detection (action vs conversational)
- JSON memory with save/forget
- Rolling log
- Speaking gate
- Personality-driven responses
- Web UI (dark minimal, status indicator, text log, input)

---

#### What's In Progress Right Now 🔲

**Backend:** Splitting LLM action response into separate `confirmation` + `message` fields so the frontend can display them sequentially. `speak()` still gets the combined string. `/command` returns both fields separately.

**UI Redesign:** Moving to concept B layout:

- Fullscreen Three.js abstract geometric particle system (canvas background)
- Top-left: VALE name + status indicator + text input (minimal, no card/border)
- Bottom-right: single line of text, no border, no card
- Sequential fade: confirmation fades in → holds → fades out → message fades in → holds → fades out
- No cards anywhere — pure text overlaid on particles

---

#### Roadmap Remaining

- 🔲 Backend confirmation/message split (in progress)
- 🔲 UI redesign — concept B layout
- 🔲 Three.js abstract geometric particle system
- 🔲 Sequential confirmation → action text fade
- 🔲 State-reactive particles (idle / listening / thinking / speaking) — planned for later
- 🔲 Volume control via `pycaw`
- 🔲 System info via `psutil`
- 🔲 Homelab migration (split architecture: mini PC always-on + main PC GPU via network)
- 🔲 Voice toggle, personality modes (polite/neutral/etc.)
  
  This is what i got from the chat specificly created for VALE before obsidian prompted me after i said i wanted a summary. Thats why there wasnt anything in obsidian, but shouldnt there have been some context in your own memory (Claude).