# UI & Design — Song Player

---

## Design Principles

Inherits the same language as Empty, Mise, and Tempo:
- Dark, minimal, nothing decorative
- Space is the feature — breathe
- Every element earns its place
- No modals. Toasts for feedback. Bottom sheets for flows.

---

## Color Tokens

| Token | Value |
|---|---|
| `--bg` | `#0a0a0a` |
| `--surface` | `#141414` |
| `--border` | `#1f1f1f` |
| `--text` | `#f5f5f5` |
| `--muted` | `#666` |
| `--accent` | TBD — white or a single soft color |
| `--player-bg` | `#111` (slightly distinct from page bg) |

---

## Views

### 1. Library (default / home)

- Full-height scrollable 2-column grid
- Square tiles: cover art fills the tile; no border radius or shadow
- Below each tile: track title (white, truncated 1 line) + artist (muted, truncated 1 line)
- Tracks without artwork: solid dark tile + generic music icon centered
- Bottom padding accounts for player bar when active
- **FAB** bottom-right corner: `+` → opens upload flow
- **Empty state** (no tracks yet): centered icon + `"No tracks yet"` in `--muted` + subtle `"Tap + to upload"` below

### 2. Upload (bottom sheet)

One continuous flow, not stepped:
1. Tap `+` → sheet opens
2. File picker tap target (large, full-width)
3. Once file selected: title input (auto-focused), artist input
4. Cover art: secondary optional tap target
5. Upload progress bar (replaces file picker area)
6. Auto-dismiss on success, toast confirmation

### 3. Player Bar (persistent, bottom)

Always visible once a track is loaded. Fixed above iOS home indicator.

```
┌─────────────────────────────────────────────────┐
│ [art] Track Title                  [⏮] [⏸] [⏭] │
│       Artist Name                               │
│       [━━━━━━●━━━━━━━━━━━━━━━] 1:23 / 3:45     │
└─────────────────────────────────────────────────┘
```

- Height: ~80–90px
- Seek bar: full width, draggable
- Cover art: 48×48px square, no radius
- Tap anywhere except controls → V2: expand to full-screen player

### 4. Login View

- Minimal: email field, password field, sign in button
- Toggle: "Don't have an account? Sign up" (swaps to sign up mode)
- No logo, no decorative elements
- Same dark bg as everything else

---

## UX Rules

- **No hamburger menus.** No nav bars. Library is the whole app in V1.
- **No settings screen** in V1. Nothing to configure yet.
- **Toasts only** for success/error feedback. No blocking dialogs.
- **Skeleton loading** for library grid while fetching (dim placeholder tiles).
- **Upload progress** shown in-sheet — do not dismiss until complete or failed.
- **Seek bar** on player bar should be thumb-height friendly — at least 20px tap target even if visually thinner.
