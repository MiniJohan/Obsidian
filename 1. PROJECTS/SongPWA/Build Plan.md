# Build Plan — Song Player

Chunked delivery. Each chunk is backend/logic only. Zon handles all frontend styling independently. Claude delivers a chunk → Zon confirms it works → next chunk.

---

## Chunk 1 — Supabase Setup

**Deliverables:**
- SQL script: `tracks` table + RLS policy + index
- Supabase Storage: `audio` bucket (private) + `artwork` bucket (private)
- Bucket configuration notes (MIME type allowlist, size limits)

**Why first:** Schema must be locked before any JS is written. Everything downstream depends on it.

---

## Chunk 2 — Auth Flow

**Deliverables:**
- `js/auth.js` — `signIn()`, `signUp()`, `signOut()`, `getSession()`, `onAuthStateChange()`
- `js/views/login.js` — login view wired to `auth.js` (logic only, no styles)
- Session persistence via Supabase's built-in `localStorage` management

**Why:** Auth gates the whole app. Must work before upload or library can be tested end-to-end.

---

## Chunk 3 — Upload Flow

**Deliverables:**
- `js/storage.js` — `uploadAudio()`, `uploadArtwork()`, `getSignedUrl()`
- `js/db.js` — `insertTrack()`, `getTrack()`
- `js/views/upload.js` — file picker, metadata inputs, progress indicator, submit handler
- Duration extraction via `<audio>` `onloadedmetadata` before upload

**Why:** Core write path. No library without data. Also stress-tests Supabase Storage config.

---

## Chunk 4 — Library View

**Deliverables:**
- `js/db.js` — `getLibrary()` (fetch all user tracks, sorted by `created_at desc`)
- `js/views/library.js` — render track grid, tap handler, signed URL fetch for artwork
- Empty state handling

**Why:** Core read path. Confirms data round-trips correctly before touching the player.

---

## Chunk 5 — Player

**Deliverables:**
- `js/player.js` — audio element singleton, `play()`, `pause()`, `seek()`, `setQueue()`, `next()`, `prev()`, auto-advance on `ended`
- `js/ui/player-bar.js` — bottom bar DOM wiring, seek bar drag handler
- Media Session API — `navigator.mediaSession` metadata + action handlers (play, pause, next, prev)
- Signed URL generation for audio playback

**Why:** The actual product. Blocked on library and storage working first.

---

## Chunk 6 — PWA Setup

**Deliverables:**
- `manifest.json` — name, icons, `display: standalone`, `theme_color`, `background_color`
- `sw.js` — app shell cache (cache-first for CSS/JS/HTML; network-only for audio/API)
- iOS meta tags in `index.html`

**Why:** Makes it installable. Has no logical dependencies so it's done last.

---

## Chunk 7 — Deploy

**Deliverables:**
- Vercel project config
- Environment variable setup (`SUPABASE_URL`, `SUPABASE_ANON_KEY`)
- Supabase URL allowlist (production domain)

---

## Open Decisions (Pre-Build)

| # | Question | Impact |
|---|---|---|
| 1 | **Project name** | Affects manifest, folder, Supabase project name |
| 2 | **Accepted audio formats** | Determines bucket MIME allowlist |
| 3 | **Upload size cap** | Supabase free tier: 50MB/file max |
| 4 | **Artwork required or optional?** | Affects upload UX and library empty state design |
