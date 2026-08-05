---
status: active
last-updated: 2026-08-05
---

# Recipe PWA

> A calm personal food companion.
> Effortless recipe collection and "what should I eat?" decision support.
> PWA-first. Personal use now, multi-user ready later.

> Full brief: [[RecipePWA-brief]]

---

## Vision

Not a calorie tracker. Not a diet app. Not a social network.

Two questions the app must answer beautifully:
1. **What should I eat?**
2. **Where did I save that recipe?**

Design inspiration: Apple apps, Linear, Notion, Things, Bear, Arc Browser.
Feel: minimal, modern, premium, calm, fast, intentional.
Theme: light only. Grayscale palette. Inter. Generous whitespace.

---

## Status

**Phase:** Foundation / Pre-build

Nothing has been decided on tech, naming, or architecture yet.
All decisions below are open and need to be made before code begins.

---

## Open Foundation Decisions

- [x] Name + branding → **Mise**
- [x] Product positioning (tagline, identity) → *A quieter way to think about food*
- [x] Information architecture (screens, navigation model) → no tab bar V1, Library + Detail + Create/Edit sheet + inline search, 2-col grid cards
- [x] Frontend framework choice → **React + Vite**
- [x] Backend + hosting strategy → **Supabase** (DB + auth + image storage) + **Vercel** hosting
- [x] Database (local-first vs cloud-first) → **Supabase Postgres**, cloud-first + service worker read cache for offline
- [x] Authentication strategy → **Magic link** (Supabase Auth), persistent JWT session, RLS + user_id on all tables from day one
- [x] Offline / PWA strategy → **Service worker read cache** via vite-plugin-pwa + Workbox
- [x] Image storage approach → **Supabase Storage**
- [x] Project structure + folder conventions → src/pages, src/components, src/hooks, src/lib, src/styles
- [x] Deployment pipeline → **Vercel** from GitHub

---

## Version 1 Scope (MVP)

Core recipe library only. No tracking. No social. No AI (yet).

| Feature | Status |
|---|---|
| Recipe library view | ✅ Done |
| Recipe cards | ✅ Done |
| Recipe detail page | ✅ Done |
| Recipe search | ✅ Done |
| Favorites | ✅ Done |
| Create recipe | ✅ Done |
| Edit recipe | ✅ Done |
| Image support | ✅ Done |
| Tags | ✅ Done |
| Basic categories | ✅ Done |
| Offline caching | ✅ Done |
| Vercel deployment + PWA install | ✅ Done |
| V1 code review + fixes | ✅ Done |

---

## Version 2 (Planned — Not Soon)

Optional lightweight food awareness.
- Log what you ate today
- Gentle observations only ("add some vegetables", "try more variety")
- No calories, no macros, no scores, no streaks, no guilt

---

## Future Possibilities (Ideas Only)

- Recipe URL / AI importing
- Shopping lists
- Meal planning
- Recipe sharing
- AI recommendations
- Voice input
- Home screen widgets
- Cloud sync

---

## Design Language

- Light theme only
- Grayscale color palette
- Inter typography
- Generous whitespace
- Rounded corners
- Minimal animations
- Subtle depth
- No gradients, no decoration

---

## Architecture Principles

- Simple. No enterprise patterns.
- Personal first, multi-user ready structurally.
- No big rewrite needed when scaling.
- Refactor when needed, not before.

---

## Decision Log

### 2026-08-03 — Project created
Transferred project brief into Obsidian. App is in pre-planning phase.
No code has been written. Foundation decisions are the immediate next step.
Brief saved to [[RecipePWA-brief]].

### 2026-08-05 — V1 complete + code review
- Auth switched from magic link to email/password
- Deployed to Vercel, installed as PWA on iPhone
- Code review surfaced and fixed 5 issues:
  - iOS scroll lock in Modal (position:fixed technique)
  - Orphaned images in Storage on recipe edit (now deleted before upload)
  - Blob URL memory leak in RecipeForm (URL.revokeObjectURL added)
  - Blank white screen on app load (replaced with Mise wordmark)
  - No sign out button (added to Library header, subtle)
- Minor known gaps (acceptable for V1): window.confirm on delete, div-as-button on RecipeCard, no getSession() error handling
