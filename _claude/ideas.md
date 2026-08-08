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

**Movie / book / game tracker**
Personal Letterboxd-style log. Rate, review, and filter your media history. No social features — just yours.
Stack: Supabase + vanilla JS or React
Learns: Filtering/sorting UI, rating systems, personal data ownership

**Custom new tab page**
Replace the browser's default new tab with your own: clock, weather, quick links, daily quote or task.
Stack: Vanilla JS, Chrome Extension (manifest v3)
Learns: Browser extension APIs, a completely different deployment target

**Countdown dashboard**
Track multiple named countdowns (events, deadlines, goals). Dark minimal display, persistent.
Stack: Vanilla JS + Supabase or localStorage
Learns: Date/time math, simple but satisfying to build and use daily

**Invoice generator**
Fill in client name, line items, rate → generates a clean PDF invoice. No backend needed.
Stack: Vanilla JS + jsPDF or similar
Learns: PDF generation in the browser, practical for freelance work

**Typing speed trainer**
Custom text snippets (code, prose, your own notes) to practise typing. Track WPM and accuracy over time.
Stack: Vanilla JS + Supabase for history
Learns: Real-time input handling, stats tracking, gamification basics

**Read-later / bookmarking tool**
Save URLs with a title, tags, and notes. Filter and search later. Simpler and more personal than Pocket.
Stack: FastAPI + Supabase or browser extension
Learns: Tagging systems, full-text search, optionally extension APIs

**Color palette manager**
Save, name, and organise color palettes. Click to copy hex. Export as CSS variables.
Stack: Vanilla JS + localStorage or Supabase
Learns: Clipboard API, color theory tooling, dead simple but genuinely useful

**Personal API status dashboard**
Monitor your own deployed projects (Vercel, Supabase, VALE). Ping endpoints, show uptime, alert on failure.
Stack: Python + FastAPI + minimal frontend
Learns: HTTP health checks, background polling, real ops thinking

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

**Duplicate file finder**
Scans a directory recursively, finds files with identical content (hash-based, not just name-based). Reports or deletes them.
Stack: Python stdlib only
Learns: File hashing, recursive traversal, a genuinely useful cleanup tool

**Bulk file renamer**
Define a pattern (prefix, date, sequence number) and batch-rename files in a folder. Preview before applying.
Stack: Python + `rich` for preview table
Learns: Regex, string formatting, safe file operations (preview before commit)

**System resource monitor**
Custom dark terminal dashboard showing CPU, RAM, GPU usage, disk I/O, and network — tailored to your machine.
Stack: Python + `psutil` + `rich` live display
Learns: System APIs, live terminal rendering, your RTX shows up here too

**Auto backup script**
Watches specified folders, compresses and copies to a backup destination on a schedule. Logs what changed.
Stack: Python + `schedule` + `shutil`
Learns: Scheduling, archiving, idempotent file operations

**Font previewer**
Drop fonts into a folder, browse them with sample text in a minimal UI. No more installing fonts to preview them.
Stack: Python + `tkinter` or a tiny web UI via FastAPI
Learns: Font rendering, either desktop GUI or local web UI

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

**Local document Q&A (RAG)**
Drop PDFs or markdown files into a folder → ask questions about them in plain English. VALE answers from the docs.
Stack: Python + Ollama + simple vector search (ChromaDB or pgvector)
Learns: RAG pipeline, embeddings, chunking — one of the most practical AI patterns right now

**Git commit message generator**
Run a script after staging files → it reads the diff → VALE suggests a commit message. One keystroke to accept.
Stack: Python CLI + Ollama
Learns: Reading git internals from Python, practical AI-in-workflow integration

**Code review bot**
Paste or pipe code → VALE gives structured feedback: bugs, style issues, improvements. CLI or web UI.
Stack: Python + VALE FastAPI
Learns: Structured prompt design, output parsing

**"What should I build next?" oracle**
Feed it your current skill level, mood, and time available → it suggests a project from your ideas list and gives a one-day plan.
Stack: VALE + your ideas.md as context
Learns: RAG on your own notes, surprisingly useful as your project list grows

---

## 🎨 Creative / Left-field

**Generative wallpaper tool**
Dark, minimal generative art that exports as a wallpaper image. Noise fields, particle systems, geometric patterns.
Stack: Vanilla JS + Canvas API
Learns: Canvas 2D API, math-driven visuals, no backend at all

**Pomodoro timer with session stats**
25/5 cycles, tracks focus sessions over time, shows weekly streaks. Minimal, dark, iPhone-friendly.
Stack: Vanilla JS + Supabase
Learns: Timer logic, session persistence, simple gamification

**Text-based adventure engine**
Build a small engine for writing branching narrative games. Define scenes, choices, and outcomes in JSON.
Stack: Vanilla JS (engine) + JSON (content)
Learns: State machines, parser design, a genuinely fun rabbit hole

---

## Open
- [ ] Pick a direction (learning vs useful vs cool)
- [ ] Pick a platform (web/PWA, Python CLI/desktop, or either)
- [ ] Start a project file once decided
