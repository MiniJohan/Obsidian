# Song Player PWA — [Name TBD]

> Status: Planning — no code started
> Stack: Vanilla HTML/CSS/JS, Supabase (Auth + Postgres + Storage), Vercel
> Source of truth: `D:\Obsidian\Vaults\void\1. PROJECTS\SongPWA\`

---

## What It Is

Dark, minimal personal music player PWA. Upload audio files, browse library, play tracks. iPhone-first. No social features.

---

## Scope of Claude's Work

Backend + infrastructure only. Frontend is Zon's domain.

- Supabase schema (tracks table, RLS, indexes)
- Storage bucket setup (audio + artwork, private)
- `auth.js` — email/password auth
- `db.js` — tracks CRUD
- `storage.js` — upload, signed URLs
- `player.js` — audio playback logic, queue, Media Session API
- `manifest.json` + `sw.js` — PWA setup
- Vercel deploy

---

## Open Decisions (block build start)

| # | Question |
|---|---|
| 1 | Project name (affects manifest, folder, Supabase project) |
| 2 | Accepted audio formats — MP3 only or broader? |
| 3 | Upload file size cap preference? |
| 4 | Artwork required or optional in upload flow? |

---

## Decision Log

| Date | Decision | Reasoning |
|---|---|---|
| 2026-08-08 | Auth: email + password | Magic link fails on iOS PWA (established pattern — see memory.md) |
| 2026-08-08 | Stack: Vanilla JS | iPhone-first, matches Empty/Tempo pattern |
| 2026-08-08 | Storage: private Supabase buckets + signed URLs | Personal app, security > convenience |
| 2026-08-08 | Hosting: Vercel | Consistent with Mise; GitHub Pages adds friction |
