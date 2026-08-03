# Claude Memory

## Identity
- Name: Zon
- Role: Frontend dev (HTML/CSS/JS), expanding into Python + backend
- OS: Windows, NVIDIA RTX 5060 Ti 16GB
- Aesthetic: Dark, minimal, no clutter

## Active Projects
- [[VALE]] — Local AI home assistant. FastAPI (port 5500), Ollama qwen2.5:14b,
  Kokoro ONNX TTS, faster-whisper STT. Location: D:\VALE\
- [[Empty PWA]] — iPhone-first thought capture + checklist. Supabase backend,
  magic-link auth. Next: timestamps, multi-list support, Ollama AI rewrite.
- [[RecipePWA]] — Calm personal food companion PWA. Recipe library + "what should I eat?"
  In pre-planning phase. No tech decisions made yet. Light theme, grayscale, Inter.

## Tech Stack
| Layer | Tools |
|---|---|
| Frontend | HTML, CSS, vanilla JS |
| Backend | Python, FastAPI |
| AI local | Ollama, Cloudflare Tunnel, Whisper, Kokoro ONNX |

---

## GitHub Access
Before doing any deep debugging, code review, or architecture work on a project — always check if the repo is accessible. Never guess at code structure when files can be read directly.

### How to access
| Method | When to use | How |
|---|---|---|
| **Public repo URL** | Repo is public on GitHub | `web_fetch` raw files via `https://raw.githubusercontent.com/USER/REPO/BRANCH/path/file` |
| **Local clone via brain MCP** | Repo is cloned on Zon's machine | Ask for the local path, read files via brain filesystem tools |
| **Paste** | Private, not cloned | Ask Zon to paste the relevant files |

### Rules
- No GitHub MCP connector exists in the directory — do not suggest it as an option.
- Always ask: *"Can you share the GitHub URL or local repo path?"* before diagnosing any code issue.
- If the repo path isn't in the project file, ask for it and save it once provided.

---

## Communication preferences
- Direct and honest. Disagree when wrong.
- Explain reasoning, not just solutions.
- Prioritize understanding over copy-paste code.
- Design feedback: dark minimal wins. Reject anything cluttered.
