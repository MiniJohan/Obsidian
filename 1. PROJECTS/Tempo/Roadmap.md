# Tempo — Roadmap

---

## V1 — Core (Build This First)

> Minimum viable scheduler. Templates + rules + today view.

- [ ] Project setup (HTML/CSS/JS scaffold, manifest, service worker skeleton)
- [ ] Supabase project + schema (templates, blocks, schedule_rules, overrides)
- [ ] Magic link auth flow
- [ ] Design tokens + CSS reset
- [ ] Today view — timeline component
- [ ] Date resolution logic (`getScheduleForDate`)
- [ ] Templates view — CRUD
- [ ] Block creation within templates
- [ ] Schedule creation — "once" and "daily" repeat types
- [ ] Weekly repeat (weekday picker)
- [ ] Monthly repeat
- [ ] Specific dates repeat
- [ ] Upcoming view — 5-day strip
- [ ] Offline: service worker caches today + templates
- [ ] Empty states

---

## V2 — Depth

> Make the existing features more useful.

- [ ] Block completion (check off as you go through the day)
- [ ] Override individual occurrences (skip / replace)
- [ ] Duplicate templates
- [ ] Reorder blocks within a template (drag)
- [ ] Per-template accent color picker
- [ ] Current-block auto-scroll on Today view open

---

## V3 — Intelligence (Later)

> Nice-to-haves, not core.

- [ ] AI rewrite of block titles (via Cloudflare Tunnel → local Ollama, like Empty)
- [ ] Push notifications for upcoming blocks
- [ ] Time tracking — mark actual vs planned
- [ ] Basic stats view (% of schedule followed this week)
- [ ] iCal export

---

## Deferred / Won't Do V1

- Collaboration / shared schedules
- Calendar sync (Google Cal, etc.)
- Sub-tasks within blocks
- Mobile native app
- Multiple color themes
