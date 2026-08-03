---
status: planning
last-updated: 2026-08-03
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
- [ ] Authentication strategy
- [ ] Offline / PWA strategy
- [ ] Image storage approach
- [ ] Project structure + folder conventions
- [ ] Deployment pipeline

---

## Version 1 Scope (MVP)

Core recipe library only. No tracking. No social. No AI (yet).

| Feature | Status |
|---|---|
| Recipe library view | Not started |
| Recipe cards | Not started |
| Recipe detail page | Not started |
| Recipe search | Not started |
| Favorites | Not started |
| Create recipe | Not started |
| Edit recipe | Not started |
| Image support | Not started |
| Tags | Not started |
| Basic categories | Not started |

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

### 2026-08-03 — Name + positioning decided
- **Name:** Mise (from "mise en place" — everything in its place)
- **Tagline:** *A quieter way to think about food*
- **Positioning:** Calm personal recipe companion. Anti-diet, anti-clutter.
- Rejected: Larder (strong but niche), Plum (too light on meaning)
