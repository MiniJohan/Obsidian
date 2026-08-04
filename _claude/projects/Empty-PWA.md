---
status: v1-complete
last-updated: 2026-08-04
---

# Empty PWA

> Minimal iPhone-first thought capture and checklist app.
> Live on GitHub Pages. Supabase backend with email/password auth.
> V1 complete. Parked. Return after Mise ships.

---

## GitHub Repo
- **URL:** https://github.com/MiniJohan/Empty
- **Live:** https://minijohan.github.io/Empty/
- **Branch:** main
- **Fetch files:** `https://github.com/MiniJohan/Empty/blob/main/FILENAME`

> Claude: fetch blob page per file. `style.css` not accessible via prior URLs — ask user to paste.

---

## Stack
| Layer | Tools |
|---|---|
| Frontend | Vanilla HTML / CSS / JS |
| Backend | Supabase (Postgres + Auth) |
| Hosting | GitHub Pages |
| Auth | Email + password |
| Service Worker | `service_worker.js` |

---

## V1 Features (complete)

- [x] Email + password auth, persistent session
- [x] Auto-create account on first login
- [x] Thought capture with multi-item split (comma / semicolon / newline)
- [x] Toggle done / urgent
- [x] Swipe left to delete (with collapse animation)
- [x] Inline edit on tap
- [x] Voice input — language auto-detects from device (`navigator.language`), overridable in settings
- [x] Search mode (type `/` to filter)
- [x] Settings sheet: voice language, clear completed, sign out
- [x] RLS with `user_id` on all queries
- [x] Service worker registered correctly

---

## Future Considerations (not planned)

### Due dates / reminders
Would require:
- Server-side push subscriptions (Web Push API)
- iOS 16.4+ for PWA push support
- User permission flow
- Background processing

Not a small feature. Also shifts the app's mental model from "capture tool" toward full task manager.
Decide intent first, then build. Revisit after Mise V1.

### Multi-list support
Schema change: new `lists` table, `list_id` FK on `thoughts`.
Good next step if the app gets daily use and the single list feels limiting.

### AI rewrite via Ollama
Cloudflare Tunnel → local Ollama → rewrite thought in place.
VALE already has Ollama running. Tunnel is the only missing piece.

### Timestamps
`created_at` likely exists already (Supabase default).
Just needs UI: relative time display ("2 hours ago").

---

## Auth — Why Email + Password

Magic link was the original approach. Abandoned after root cause diagnosis:

iOS standalone PWA and Safari have **permanently isolated localStorage**.
No config change, no Supabase setting, no code workaround fixes this.
Every magic link opens Safari. The session is created in Safari.
The PWA's localStorage is empty. The user sees the login screen again.

Android doesn't have this isolation — which is why Zon's friend had no issues.

**Solution:** email + password. Everything happens inside the PWA's own context.

Supabase setting required: **Authentication → Providers → Email → Confirm email: OFF**

---

## Bug History

| Bug | Root cause | Fix |
|---|---|---|
| Auth loop on iOS | iOS localStorage isolation between Safari and PWA | Replaced magic link with email + password |
| PKCE failed | Code verifier in PWA, exchange attempted in Safari | Removed PKCE |
| OTP sent link not code | Supabase free tier requires SMTP to edit email templates | Abandoned OTP |
| Button click did nothing | `passwordInput` not declared as variable | Added to element declarations |
| Password field unstyled | `#password-input` not in index.html | Added to login-box |
| Service worker never registered | Registered as `./sw.js`, file is `service_worker.js` | Fixed path |
| Mic failed for non-Swedish users | Language hardcoded `sv-SE` | `getVoiceLang()` reads `navigator.language` |
