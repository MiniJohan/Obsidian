# Tempo — Build Plan

> Self-directed build. Every phase ends with something visible and interactive in the browser. No phase is "just backend."

---

## Ground Rules

- **Hardcode first, wire later** — fake data in the UI before touching Supabase
- **No Supabase until Phase 8** — build on in-memory JS objects first; the UI will teach you what the schema actually needs
- **One phase at a time** — don't start Phase N+1 until Phase N feels solid
- **Ask Claude for bugs/fixes** — implementation is yours, unsticking is fair game

---

## Phase 1 — Shell & Scaffold
> *What you'll see: An app that looks like an app.*

- [ ] `index.html` — semantic skeleton, viewport meta, font import (Inter)
- [ ] `css/variables.css` — all design tokens (bg, surface, text, accent, radius, etc.)
- [ ] `css/reset.css` — box-sizing, margin/padding reset, base body styles
- [ ] `css/app.css` — layout: full-height, safe area insets, bottom nav
- [ ] Bottom nav — 3 tabs (Today / Upcoming / Templates), icons or text, active state
- [ ] Tab switching — JS shows/hides `.view` sections, no router needed yet
- [ ] `js/app.js` — entry point, tab logic

**Done when:** Three empty dark screens you can switch between. Looks like a real app.
**Time estimate:** 1–2h

---

## Phase 2 — Timeline Component
> *What you'll see: The heart of the app, alive with hardcoded data.*

- [ ] `js/ui/timeline.js` — function that takes a list of block objects and renders a timeline
- [ ] Hour marks — left column, `--text-faint`, 06:00 → 23:00
- [ ] Block rendering — height proportional to duration (decide your px-per-minute unit, e.g. `1.2px/min`)
- [ ] Block positioning — `top` calculated from start_time offset from 00:00
- [ ] Block styles — `--surface-raised` bg, 3px left border in `--accent`, rounded, title text
- [ ] Hardcoded block data — 3–4 fake blocks to work with
- [ ] Current time indicator — thin red line + small circle, calculated from `new Date()`
- [ ] Auto-scroll — on load, scroll timeline so current time is ~30% from top of viewport
- [ ] Wire timeline into Today view

**Done when:** You open the app and see a real-looking schedule with a red "now" line in the right place.
**Time estimate:** 3–4h

---

## Phase 3 — Block Interaction
> *What you'll see: Blocks that respond. The app feels alive.*

- [ ] Tap block → expand in-place: reveal start/end time + notes text (CSS transition, not a modal)
- [ ] Tap again (or tap elsewhere) → collapse
- [ ] "Active" block (current time falls inside it) — slightly brighter or accent-highlighted
- [ ] `js/ui/sheet.js` — reusable bottom sheet component
  - [ ] Opens from bottom with translate animation
  - [ ] Drag handle at top
  - [ ] Tap backdrop → close
  - [ ] Drag down → close
- [ ] `+` FAB button → opens sheet (empty for now, just the shell)

**Done when:** Blocks expand/collapse, the bottom sheet slides up smoothly. App feels genuinely interactive.
**Time estimate:** 2–3h

---

## Phase 4 — Creation UI (Static)
> *What you'll see: The full creation flow, even though nothing saves yet.*

- [ ] Bottom sheet step controller — tracks current step (1/2/3), renders different content
- [ ] Step 1: Origin picker — "Start fresh" / "Use a template" (radio or tap cards)
- [ ] Step 2a (fresh): Name input + date picker + inline block builder
  - [ ] "Add block" row: title input + start/end time inputs
  - [ ] Can add multiple blocks, can remove
- [ ] Step 2b (template): template list picker (hardcoded for now)
- [ ] Step 3: Repeat rule UI
  - [ ] Toggle "Repeat this schedule"
  - [ ] Frequency: Daily / Weekly / Monthly / Specific dates
  - [ ] Weekly: day chips (M T W T F S S)
  - [ ] End condition: Never / On date / After N
- [ ] "Save" button — logs to console, closes sheet (no real save yet)
- [ ] Back button between steps

**Done when:** You can walk through the full creation flow end-to-end. Looks complete. Saves nothing.
**Time estimate:** 3–4h

---

## Phase 5 — Templates View
> *What you'll see: A library of schedule templates with visual previews.*

- [ ] Template card component — name + mini horizontal timeline + metadata
- [ ] Mini timeline: horizontal bar, colored blocks proportional to duration (SVG or CSS flex)
- [ ] Metadata line: `{n} blocks · {total}h` in `--text-dim`
- [ ] Hardcoded 2–3 template cards to work with
- [ ] Tap card → template detail view (show full block list, Edit button)
- [ ] Template edit view — block list, can reorder visually (drag optional for now)
- [ ] Empty state — "No templates yet · tap + to create one"
- [ ] `+` in Templates view → opens creation sheet at Step 2a

**Done when:** Templates view feels like a complete screen. Mini timeline previews are accurate and proportional.
**Time estimate:** 2–3h

---

## Phase 6 — Upcoming View
> *What you'll see: A 5-day overview. The full visual shell is done.*

- [ ] Day strip — horizontal row, 5 days centered on today
- [ ] Day chip — day name + date number, dot if has schedule (hardcoded dots for now)
- [ ] Tap day chip → show that day's content below
- [ ] Compact block list — `time | color bar | title | duration` per row
- [ ] Tap row → expand notes
- [ ] Empty state per day — "Nothing scheduled"
- [ ] Left/right swipe or arrow to move the 5-day window

**Done when:** All three views look finished. App could be demoed visually right now.
**Time estimate:** 2h

---

## Phase 7 — Local State (No DB)
> *What you'll see: Everything actually works. Creates, saves, persists in memory.*

- [ ] `js/store.js` — in-memory state object: `{ templates: [], rules: [], overrides: [] }`
- [ ] Wire template creation → pushes to `store.templates`
- [ ] Wire block creation within templates → pushes to template's blocks array
- [ ] Templates view reads from store, re-renders on change
- [ ] `js/schedule.js` — `getScheduleForDate(date, store)` — returns template + blocks for that date
  - [ ] Handle: once, daily, weekly, monthly, specific
- [ ] Today view reads from `getScheduleForDate(today)`
- [ ] Upcoming view reads from `getScheduleForDate` for each of the 5 days
- [ ] Delete template — removes from store, re-renders
- [ ] `sessionStorage` persistence — optional, survives page refresh during dev

**Done when:** You can create a template, apply it to days, see it appear on Today and Upcoming. Full app loop works, no DB needed.
**Time estimate:** 3–5h (this is the most logic-heavy phase)

---

## Phase 8 — Supabase
> *What you'll see: Data survives refresh. Auth gate on entry.*

- [ ] Supabase project setup
- [ ] Run schema SQL (from Data Model.md)
- [ ] Enable RLS + add policies
- [ ] `js/auth.js` — magic link flow, session check on load
- [ ] Auth gate — if no session, show email input; if session, show app
- [ ] `js/db.js` — functions mirroring store: `getTemplates()`, `createTemplate()`, `getBlocks()`, etc.
- [ ] Swap `store.js` calls for `db.js` calls (one function at a time)
- [ ] Test: create template → refresh → still there

**Done when:** App works identically to Phase 7 but data lives in Supabase. Auth works.
**Time estimate:** 3–4h

---

## Phase 9 — PWA
> *What you'll see: Installable. Works offline. Feels native.*

- [ ] `manifest.json` — name, icons, theme_color (`#0a0a0a`), display: standalone
- [ ] `sw.js` — cache shell (HTML, CSS, JS) on install
- [ ] Cache today's schedule + templates in IndexedDB on load
- [ ] Serve cached data if offline
- [ ] Test install on iPhone (Safari → Share → Add to Home Screen)

**Done when:** Installed on phone. Opens full-screen. Today view loads offline.
**Time estimate:** 2–3h

---

## Phase 10 — Polish
> *What you'll see: The gap between "works" and "feels good."*

- [ ] Transitions between views (fade or slide)
- [ ] Block completion — tap checkbox to mark done (stored as override)
- [ ] Per-template color picker
- [ ] Scroll position memory (return to same position when switching tabs)
- [ ] Haptic feedback on mobile (where supported)
- [ ] Final empty state polish
- [ ] Test on actual iPhone

---

## Quick Reference

| Phase | Focus | Visual Output | Est. Time |
|---|---|---|---|
| 1 | Scaffold | Dark shell, 3 tabs | 1–2h |
| 2 | Timeline | Live timeline, now-line | 3–4h |
| 3 | Interaction | Tap expand, bottom sheet | 2–3h |
| 4 | Creation UI | Full flow, no save | 3–4h |
| 5 | Templates | Cards, mini preview | 2–3h |
| 6 | Upcoming | 5-day view | 2h |
| 7 | Local state | Fully functional, no DB | 3–5h |
| 8 | Supabase | Persistent, auth | 3–4h |
| 9 | PWA | Installable, offline | 2–3h |
| 10 | Polish | Feels native | open |
