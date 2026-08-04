---
status: active
last-updated: 2026-08-04
---

# Empty PWA

> Minimal iPhone-first thought capture and checklist app.
> Live on GitHub Pages. Supabase backend with email/password auth.

---

## Stack
| Layer | Tools |
|---|---|
| Frontend | Vanilla HTML / CSS / JS, PWA manifest |
| Backend | Supabase (Postgres + Auth) |
| Hosting | GitHub Pages |
| Auth | Email + password (signInWithPassword / signUp) |
| AI (planned) | Ollama via Cloudflare Tunnel |

---

## GitHub Repo
- **URL:** https://github.com/MiniJohan/Empty
- **Live:** https://minijohan.github.io/Empty/
- **Branch:** main
- **Raw file access:** `https://github.com/MiniJohan/Empty/blob/main/FILENAME`

> Claude: fetch blob pages to read file content. Raw URLs via `raw/refs/heads/main/` pattern.
> Do NOT guess at code — fetch the file first.

---

## Files
| File | Purpose |
|---|---|
| `index.html` | Shell, PWA meta, login + app screens |
| `app.js` | All logic — auth, render, CRUD, voice, search |
| `style.css` | All styles |
| `manifest.json` | PWA manifest |
| `service_worker.js` | SW (was broken — `sw.js` path fixed to `service_worker.js`) |

---

## Auth — Current Implementation

**Method:** Email + password via Supabase `signInWithPassword` / `signUp`

**Why not magic link:** iOS standalone PWA and Safari have completely isolated
localStorage. Magic links always open in Safari. Session created in Safari never
reaches the PWA's storage context. This is permanent iOS behavior.

**Flow:**
1. User enters email + password → hits continue
2. App tries `signInWithPassword` first
3. If "Invalid login credentials" → tries `signUp` to create account, then signs in
4. `onAuthStateChange` fires → `showApp()` called
5. `getSession()` on startup covers returning users — reads from PWA's own localStorage

**Supabase settings required:**
- Authentication → Providers → Email → **"Confirm email" must be OFF**
- No redirect URLs needed

---

## What's Been Built

- PWA scaffolded and live on GitHub Pages
- Supabase connected, RLS with `user_id` on `thoughts` table
- Email + password auth (working)
- Session persistence — opens straight to app on return
- Thought capture: write, persist to Supabase
- Toggle done / urgent
- Swipe left to delete
- Inline edit on tap
- Voice input (Swedish, auto-dump on 4s silence)
- Search mode (type `/` to filter)

---

## What's Needed Next

### 1. Timestamps
- [ ] Confirm `created_at` column exists on `thoughts` (Supabase default)
- [ ] Add `updated_at` with auto-update trigger if missing
- [ ] Display relative timestamps in UI ("2 hours ago")

### 2. Multiple Lists
Biggest structural change.

**Schema:**
```sql
create table lists (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users not null,
  name text not null,
  created_at timestamptz default now()
);

alter table thoughts add column list_id uuid references lists(id);
```

- [ ] Create `lists` table
- [ ] Add `list_id` FK to `thoughts`
- [ ] Migrate existing thoughts to a default list
- [ ] UI: list switcher, create/rename/delete list
- [ ] Filter thoughts query by active `list_id`

### 3. AI Rewrite via Ollama
- [ ] Cloudflare Tunnel pointing to local Ollama (port 11434)
- [ ] Secure tunnel endpoint
- [ ] "Rewrite" button per thought
- [ ] POST to tunnel → stream response
- [ ] Update thought in Supabase

---

## Known Issues (resolved)

| Issue | Status | Fix |
|---|---|---|
| Magic link opens Safari on iOS, session never reaches PWA | ✅ Fixed | Switched to email + password auth |
| PKCE code verifier not accessible cross-context on iOS | ✅ Fixed | Removed PKCE, switched auth method entirely |
| Service worker registered as `./sw.js` (file doesn't exist) | ✅ Fixed | Changed to `./service_worker.js` |
| `passwordInput` not declared as variable | ✅ Fixed | Added `const passwordInput = document.getElementById('password-input')` |
| `#password-input` missing from `index.html` | ✅ Fixed | Added input to login-box |

---

## Decision Log

### 2026-08-02 — Second Brain setup
Set up Obsidian vault (Void) as second brain with Claude Desktop MCP integration.

### 2026-08-03 — Auth method changed: magic link → email/password
Magic link auth is fundamentally broken on iOS PWA due to storage partitioning
between Safari and standalone PWA contexts. This is not fixable in code.
Switched to email/password. No Supabase SMTP setup required. No custom domain needed.
Supabase "Confirm email" toggle must be OFF for auto-signup flow to work.

### 2026-08-04 — Bug fixes
Fixed missing `passwordInput` variable declaration and missing `#password-input`
element in `index.html`. Both were omitted in previous fix delivery.
