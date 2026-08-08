# Project Ideas
Last updated: 2026-08-08

---

## 🌐 Web / PWA

**Link shortener with analytics**
Define a short slug, redirect to a long URL, track every click (timestamp, referrer, country).
Stack: FastAPI + Supabase
Learns: URL routing, redirect logic, basic analytics

**Budget tracker**
Log income/expenses, categorize, see monthly summaries. Recurring transactions, categories, date math.
Stack: Supabase + vanilla JS or React
Learns: Relational data modelling, date arithmetic, aggregation queries

**Ambient soundscape PWA**
Layer audio tracks (rain, fire, café) with individual volume sliders. iPhone-first, zero backend.
Stack: Vanilla JS + Web Audio API
Learns: Web Audio API, PWA fundamentals, no backend needed

**Personal devlog / changelog**
Log what you built and learned each week. Markdown-powered, minimal UI.
Stack: Static or Supabase-backed
Learns: Writing habit, could double as portfolio signal

---

## 🐍 Python / Desktop

**File auto-organizer**
Watches a folder (e.g. Downloads), moves files into subfolders by type and date automatically.
Stack: Python + `watchdog` library
Learns: File system events, background processes, no UI needed

**Clipboard history manager**
Captures everything you copy into a local SQLite DB. Search history from terminal or tray app.
Stack: Python + SQLite
Learns: Background daemons, local persistence, tray apps

**CLI task manager**
Personal task list in the terminal. Add/complete/list tasks. Pretty output via `rich`.
Stack: Python + `rich` + SQLite
Learns: CLI design, data persistence, building tools you actually use

---

## 🤖 AI / VALE-adjacent

**AI weekly journal summarizer**
Write rough daily notes → VALE summarizes the week and tags recurring themes.
Stack: VALE FastAPI integration + Supabase or local files
Learns: Prompt engineering, structured outputs, VALE extension

**Screenshot explainer**
Screenshot anything (error, UI, chart) → send to Ollama vision model → get explanation back.
Stack: Python desktop script or tray app + Ollama vision
Learns: Multimodal API, desktop scripting

**Stable Diffusion prompt builder**
Dark minimal UI for constructing and saving image generation prompts. Tag pickers, style modifiers, history.
Stack: Vanilla JS + Supabase (or localStorage)
Learns: Frontend-only is fine, RTX can run SD natively if you want to extend it

---

## Open
- [ ] Pick a direction (learning vs useful vs cool)
- [ ] Pick a platform (web/PWA, Python CLI/desktop, or either)
- [ ] Start a project file once decided
