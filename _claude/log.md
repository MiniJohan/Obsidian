# Dev Log

Running journal of what was built, decided, and fixed.
Most recent at the top.

---

## 2026-08-04

### Empty PWA — Settings + mic fix
- Added settings sheet: voice language selector, clear completed, sign out
- Mic was hardcoded `sv-SE` — now reads `navigator.language` as default, overridable in settings
- Fixes mic failures for non-Swedish users (friend on Android, sister on whatever device)
- User fixed overlap between settings button and mic button post-delivery
- **V1 complete. Empty PWA parked.**

### Due dates / reminders — deferred
- Considered for Empty PWA, decided against adding now
- Push notifications on iOS PWA require server-side push subscriptions + iOS 16.4+ + user permission — not a small addition
- Due dates without notifications are half a feature
- Adding them shifts Empty from capture app toward full task manager — unclear if that's wanted
- Revisit after Mise V1 ships

---

## 2026-08-04 (earlier)

### Empty PWA — Bug fixes (auth)
- `passwordInput` variable missing from element declarations — click handler crashed silently on button press
- `#password-input` field was never added to index.html — same root cause
- Both fixed, auth working on iOS and Android

---

## 2026-08-03

### Empty PWA — Auth overhaul (magic link → email/password)
- Root cause confirmed: iOS PWA and Safari have permanently isolated localStorage
- Magic link opens in Safari → session stored there → PWA localStorage is empty on startup
- Attempted fixes in order: PKCE flow → implicit flow → OTP (blocked by Supabase SMTP requirement on free tier)
- Final solution: `signInWithPassword` / `signUp` — everything stays in PWA context
- Supabase "Confirm email" must be OFF for auto-signup to work
- Broken service worker fixed: `./sw.js` → `./service_worker.js`

### Obsidian scaffold
- memory.md, active.md, log.md, projects/ folder created
- GitHub access methodology added to memory.md

---

## Pre-2026-08-03

### Empty PWA — initial state
- Live on GitHub Pages: https://minijohan.github.io/Empty/
- Supabase connected, RLS with user_id on thoughts table
- Features working: thought capture, toggle done/urgent, swipe delete, inline edit, voice (sv-SE), search mode (/)

### Mise — foundation complete
- Name: Mise (mise en place — everything in its place)
- Tagline: "A quieter way to think about food"
- Stack: React + Vite, Supabase (DB + auth + storage), Vercel
- Auth: magic link (Mise is a proper web app — iOS storage isolation not an issue here)
- IA: no tab bar in V1, Library → Recipe Detail (push nav), Create/Edit modal sheet, inline search, 2-col card grid
- Project scaffolded, useRecipes hook + RecipeCard component delivered
- Next: Library screen (grid layout + Library page component)
