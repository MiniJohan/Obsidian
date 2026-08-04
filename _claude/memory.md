# Claude Memory

## Identity
- Name: Zon
- Role: Frontend dev (HTML/CSS/JS), expanding into Python + backend
- OS: Windows, NVIDIA RTX 5060 Ti 16GB
- Aesthetic: Dark, minimal, no clutter

## Active Projects

| Project | Status | Stack | Location |
|---|---|---|---|
| [[VALE]] | Active | FastAPI, Ollama, Kokoro TTS, Whisper | D:\VALE\ |
| [[Empty PWA]] | V1 complete — parked | Vanilla JS, Supabase, GitHub Pages | https://github.com/MiniJohan/Empty |
| [[Mise]] | Active build | React + Vite, Supabase, Vercel | RecipePWA in Obsidian |

## Tech Stack
| Layer | Tools |
|---|---|
| Frontend | HTML/CSS/JS (vanilla), React + Vite |
| Backend | Python, FastAPI, Supabase |
| AI local | Ollama (qwen2.5:14b), Cloudflare Tunnel, Whisper, Kokoro ONNX |
| Hosting | GitHub Pages (Empty), Vercel (Mise) |

---

## GitHub Access
Before any debugging, code review, or architecture work — always check if the repo is accessible. Never guess at code structure when files can be read directly.

| Method | When | How |
|---|---|---|
| **Public repo URL** | Repo is public | Fetch blob page: `https://github.com/USER/REPO/blob/main/FILE` |
| **Local clone** | Repo cloned on Zon's machine | Ask for local path, read via brain MCP |
| **Paste** | Private, not cloned | Ask Zon to paste relevant files |

**Rules:**
- No GitHub MCP in the connector directory — don't suggest it
- Always ask for repo URL or local path before diagnosing any code issue
- `style.css` for Empty PWA can't be fetched via blob — ask user to paste it

---

## Key Decisions — Empty PWA

**Auth: email + password** (not magic link)
iOS standalone PWA and Safari have permanently isolated localStorage.
Magic links open in Safari. Session never reaches the PWA. Not fixable in code.
Email + password keeps everything inside the PWA's own storage context.

**Mic language: `navigator.language` default**
Was hardcoded `sv-SE`. Caused silent failures for non-Swedish users.
Now reads device language, overridable in settings.

---

## Communication preferences
- Direct and honest. Disagree when wrong.
- Explain reasoning, not just solutions.
- Prioritize understanding over copy-paste code.
- Design feedback: dark minimal wins. Reject anything cluttered.
