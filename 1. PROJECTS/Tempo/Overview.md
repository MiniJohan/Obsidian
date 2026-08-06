# Tempo — Schedule PWA

> A dark, minimal PWA for creating and following personal daily schedules. Template-driven, repeat-rule aware, iPhone-first.

---

## Status
`Planning` — Full design phase. No code started.

## Related Files
- [[Data Model]] — Supabase schema
- [[UI & Design]] — Views, design tokens, UX flows
- [[Roadmap]] — Phases and feature scope

---

## What It Is

Tempo is a personal schedule planner, not a calendar app. The distinction matters:
- **Calendar apps** manage events, invites, and time-based notifications
- **Tempo** manages your *day's structure* — repeating routines, named time blocks, daily templates you follow

You build a template once ("Workout Day", "Deep Work", "Rest Day"), then apply it to a date or a repeat rule. On the day itself, you open the app and follow the timeline.

---

## Core Principles

- **Space > Features** — progressive disclosure, nothing shown until needed
- **Dark minimal** — consistent with Empty PWA aesthetic
- **iPhone-first** — full-height timeline views, bottom sheets, thumb-friendly
- **Template-driven** — schedules are reusable patterns, not one-off events
- **Offline capable** — today's schedule and templates cached locally

---

## Tech Stack

| Layer | Choice |
|---|---|
| Frontend | Vanilla HTML / CSS / JS (no framework) |
| Backend | Supabase (Postgres + Auth) |
| Auth | Magic link |
| Offline | Service Worker + IndexedDB |
| Hosting | GitHub Pages (or Cloudflare Pages) |

---

## File Structure

```
/
├── index.html
├── manifest.json
├── sw.js
├── css/
│   ├── reset.css
│   ├── variables.css
│   └── app.css
├── js/
│   ├── app.js           — router, init
│   ├── auth.js          — magic link
│   ├── db.js            — supabase queries
│   ├── schedule.js      — date resolution logic
│   ├── views/
│   │   ├── today.js
│   │   ├── upcoming.js
│   │   └── templates.js
│   └── ui/
│       ├── timeline.js  — renders timeline component
│       ├── sheet.js     — bottom sheet controller
│       └── toast.js
└── assets/
    └── icons/
```

---

## Honest Design Note

Tempo is inherently denser than Empty — a timeline *must* show time. The "space > features" principle still applies but means: show block titles only by default, reveal time + notes on tap, hide all edit controls until invoked. The baseline density is slightly higher by necessity, but the discipline is the same.
