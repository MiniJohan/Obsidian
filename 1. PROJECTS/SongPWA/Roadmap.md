# Roadmap — Song Player

---

## V1 — Personal Library (MVP)

### Auth
- [ ] Login / register (email + password)
- [ ] Persist session across app close

### Upload
- [ ] Pick audio file (MP3 / M4A / WAV / FLAC)
- [ ] Enter title (required) + artist (optional)
- [ ] Optional: pick cover art image
- [ ] Upload progress indicator
- [ ] Duration extracted client-side via `<audio>` `onloadedmetadata`
- [ ] Insert track row to DB after storage upload completes

### Library
- [ ] 2-column grid: square cover art tiles
- [ ] Placeholder tile for tracks with no artwork
- [ ] Track title + artist shown below tile (truncated)
- [ ] Sort by date added (default)
- [ ] Tap tile → play track immediately
- [ ] FAB (+ button) → open upload flow

### Player
- [ ] Persistent bottom player bar (visible whenever a track is loaded)
- [ ] Play / Pause
- [ ] Seek bar with current time + remaining time
- [ ] Track title + artist + mini cover art
- [ ] Media Session API — lock screen controls, now-playing metadata
- [ ] Auto-advance to next track when current ends

### Queue
- [ ] Tap track → play that track, queue the rest of the library in order
- [ ] Next / Previous buttons

### PWA
- [ ] `manifest.json` — installable, standalone display
- [ ] `sw.js` — app shell cache (HTML, CSS, JS — not audio)
- [ ] iOS-specific meta tags (`apple-mobile-web-app-*`)

---

## V2 — Quality of Life

- [ ] Shuffle mode
- [ ] Loop (single track / whole queue)
- [ ] Search + filter library
- [ ] Edit track metadata (title, artist, cover art)
- [ ] Delete track (removes DB row + storage objects)
- [ ] Playback position memory (resume where you left off)
- [ ] Recently played list

---

## V3 — Enhancements

- [ ] Playlists (create, add track, play playlist)
- [ ] Waveform visualization (Web Audio API)
- [ ] Batch upload
- [ ] Full-screen expanded player view
- [ ] Sleep timer
