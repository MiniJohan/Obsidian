---
status: active
last-updated: 2026-08-04
---

# Empty PWA

> Minimal iPhone-first thought capture and checklist app.
> Live on GitHub Pages. Supabase backend with email/password auth.

---

## GitHub Repo
- **URL:** https://github.com/MiniJohan/Empty
- **Live:** https://minijohan.github.io/Empty/
- **Branch:** main
- **Fetch files:** `https://github.com/MiniJohan/Empty/blob/main/FILENAME`

> Claude: fetch blob page for each file needed. Cannot access style.css via prior URLs — ask user to paste it.

---

## Stack
| Layer | Tools |
|---|---|
| Frontend | Vanilla HTML / CSS / JS, PWA manifest |
| Backend | Supabase (Postgres + Auth) |
| Hosting | GitHub Pages |
| Auth | Email + password (`signInWithPassword` / `signUp`) |
| AI (planned) | Ollama via Cloudflare Tunnel |

---

## Files
| File | Purpose |
|---|---|
| `index.html` | Shell, PWA meta, login + app + settings screens |
| `app.js` | All logic — auth, settings, render, CRUD, voice, search |
| `style.css` | All styles (Claude cannot access directly — ask user to paste) |
| `manifest.json` | PWA manifest |
| `service_worker.js` | Offline caching |

---

## Auth
**Method:** Email + password

**Why not magic link:** iOS standalone PWA and Safari have completely isolated localStorage.
Magic links always open Safari. Session never reaches the PWA. Permanent iOS behavior.

**Flow:**
1. Email + password → continue
2. App tries `signInWithPassword` first
3. If "Invalid login credentials" → `signUp` to create, then sign in
4. `onAuthStateChange` → `showApp()`
5. `getSession()` on startup covers returning users

**Supabase settings:**
- Authentication → Providers → Email → **Confirm email: OFF**

---

## Features (V1 complete)

- [x] Email + password auth, persistent session
- [x] Thought capture with multi-item split (comma / semicolon / newline)
- [x] Toggle done / urgent
- [x] Swipe left to delete (with collapse animation)
- [x] Inline edit on tap
- [x] Voice input — language auto-detects from device, configurable in settings
- [x] Search mode (type `/` to filter)
- [x] Settings sheet:
  - Voice language selector (auto / Swedish / English / etc.)
  - Clear completed thoughts
  - Sign out
- [x] Service worker registered correctly (`service_worker.js`)
- [x] RLS with `user_id` on all queries

---

## What's Needed Next

### 1. Timestamps
- [ ] Confirm `created_at` column on `thoughts` (Supabase default)
- [ ] Add `updated_at` with auto-update trigger
- [ ] Display relative timestamps ("2 hours ago")

### 2. Multiple Lists
```sql
create table lists (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users not null,
  name text not null,
  created_at timestamptz default now()
);
alter table thoughts add column list_id uuid references lists(id);
```
- [ ] Create table, add FK, migrate existing thoughts
- [ ] UI: list switcher, create/rename/delete

### 3. AI Rewrite via Ollama
- [ ] Cloudflare Tunnel → local Ollama (port 11434)
- [ ] Secure endpoint
- [ ] Rewrite button per thought

---

## Known Issues (all resolved)

| Issue | Fix |
|---|---|
| Magic link opens Safari, session never reaches iOS PWA | Switched to email + password auth |
| PKCE cross-context failure on iOS | Removed PKCE, switched auth method |
| Service worker registered as `./sw.js` | Fixed to `./service_worker.js` |
| `passwordInput` not declared as variable | Added to element declarations |
| `#password-input` missing from `index.html` | Added to login-box |
| Mic language hardcoded `sv-SE` — fails for non-Swedish users | `getVoiceLang()` reads device language, overridable in settings |

---

## Decision Log

### 2026-08-02
Obsidian vault + Claude MCP integration set up.

### 2026-08-03
Auth changed from magic link to email/password. iOS localStorage isolation is permanent.

### 2026-08-04
Settings sheet added: voice language, clear completed, sign out.
Mic language now uses `navigator.language` as default — fixes cross-device failures.
V1 feature set complete. Moving to Mise (RecipePWA) next.
