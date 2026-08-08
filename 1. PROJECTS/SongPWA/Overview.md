# Song Player — [Name TBD]

> A dark, minimal personal music player PWA. Upload your own tracks, browse your library, listen. Nothing else.

---

## Status
`Planning` — No code started.

## Related Files
- [[Data Model]] — Supabase schema + storage buckets
- [[UI & Design]] — Views, design tokens, UX flows
- [[Roadmap]] — Feature scope, V1 / V2 split
- [[Build Plan]] — Implementation order (chunked)

---

## What It Is

A personal music player — SoundCloud stripped to its core. Upload audio files, add metadata, play them back through a persistent bottom-player interface. Optimized for iPhone. No social features, no discovery feed, no algorithmic anything.

Your music. A clean player. That's it.

---

## Core Principles

- **Dark minimal** — same aesthetic as Empty, Mise, Tempo. Space > features.
- **iPhone-first** — bottom player, thumb-friendly controls, full-height library grid
- **Personal only** — no sharing, no public profiles, no external feeds
- **Upload-centric** — your files, your storage, your player
- **Offline-capable** — app shell cached via Service Worker; audio streams on demand

---

## Tech Stack

| Layer | Choice |
|---|---|
| Frontend | Vanilla HTML / CSS / JS (no framework) |
| Auth | Supabase Auth — email + password |
| Database | Supabase Postgres |
| Storage | Supabase Storage (audio + artwork, private buckets) |
| Hosting | Vercel |
| Media | HTML5 `<audio>` API + Media Session API |
| Offline | Service Worker (app shell only — audio too large to cache) |

---

## Auth Note

**Email + password only. No magic links.**
Magic links open in Safari, session never reaches the PWA context (known iOS standalone mode issue — same failure pattern as Empty and Mise). Email + password with Supabase email confirmation OFF is the proven pattern.

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
│   ├── app.js            — router, init, global state
│   ├── auth.js           — email+password auth, session management
│   ├── db.js             — Supabase DB queries (tracks CRUD)
│   ├── storage.js        — Supabase Storage (upload, signed URLs)
│   ├── player.js         — audio playback, queue, Media Session API
│   ├── views/
│   │   ├── library.js    — track grid, tap-to-play
│   │   ├── upload.js     — file pick, metadata form, upload progress
│   │   └── login.js      — auth view
│   └── ui/
│       ├── player-bar.js — persistent bottom player
│       ├── sheet.js      — bottom sheet controller
│       └── toast.js
└── assets/
    └── icons/
```
