# Tempo — UI & Design

---

## Design Tokens

```css
:root {
  /* Backgrounds */
  --bg:            #0a0a0a;
  --surface:       #111111;
  --surface-raised:#1a1a1a;

  /* Borders */
  --border:        #1e1e1e;
  --border-dim:    #161616;

  /* Text */
  --text:          #f0f0f0;
  --text-dim:      #555555;
  --text-faint:    #2a2a2a;

  /* Accent (default; overridden per template) */
  --accent:        #6c63ff;
  --accent-dim:    rgba(108, 99, 255, 0.15);

  /* Functional */
  --time-line:     #ff4d4d;
  --destructive:   #ff3b30;

  /* Typography */
  --font:          'Inter', system-ui, sans-serif;
  --font-mono:     'JetBrains Mono', monospace;

  /* Spacing */
  --radius:        12px;
  --radius-sm:     8px;
  --sheet-radius:  20px;
}
```

---

## Views

### 1. Today (Default View)

Full-height vertical timeline of the current day.

**Layout:**
```
┌─────────────────────────────────┐
│  Thu Aug 6            [ + ]     │  ← header: date + add button
├─────────────────────────────────┤
│                                 │
│  08:00  ┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄  │
│                                 │
│  09:00  ██████████████████████  │  ← block
│         Deep Work               │
│         ──────────────────────  │
│  10:30  ██████████████████████  │
│                                 │
│  11:00 ─────── now ───────────  │  ← current time indicator (red)
│                                 │
│  12:00  ██████████████████████  │
│         Lunch                   │
│                                 │
│  13:00  ┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄  │
│                                 │
└────────────────────────────────┘
        [ Today ] [ Week ] [ Templates ]
```

**Behavior:**
- Scroll position initializes at current time (1hr before now)
- Hour marks: `--text-faint`, small, left-aligned
- Blocks: `--surface-raised` background, `--accent` left border (3px), title in `--text`
- Tap block → expand in-place: shows duration + notes (if any)
- Long-press block → edit mode (resize handles, delete)
- Current time: thin red line with a small filled circle on the left
- Empty state (no schedule): centered dim text "No schedule today" + subtle "+" nudge

**Block Sizing:**
- Height is proportional to duration (1 hour = fixed px unit, ~60px)
- Minimum block height: 48px (even for 15-min blocks)
- Title truncates at one line; expands on tap

---

### 2. Upcoming View

A compact multi-day overview.

**Layout:**
```
┌─────────────────────────────────┐
│  ← Aug  [6] [7] [8] [9] [10] → │  ← horizontal scrollable day strip
├─────────────────────────────────┤
│                                 │
│  Thursday, Aug 6                │
│                                 │
│  09:00 ██ Deep Work   2h        │  ← compact block row
│  11:00 ██ Break       30m       │
│  12:00 ██ Lunch       1h        │
│                                 │
│  — No more blocks —             │
│                                 │
└─────────────────────────────────┘
```

**Day Strip:**
- 5 days visible, scrollable horizontally
- Selected day: pill highlight with `--accent`
- Days with schedules: small dot below the date number
- Days without: no dot

**Block Rows:**
- Compact: time | color bar | title | duration
- Not a full proportional timeline — just a list with times
- Tap a row: expands notes
- No proportional sizing here (too small)

---

### 3. Templates View

Library of saved schedule templates.

**Layout:**
```
┌─────────────────────────────────┐
│  Templates              [ + ]   │
├─────────────────────────────────┤
│                                 │
│  ┌─────────────────────────┐   │
│  │ 🟣 Deep Work Day         │   │
│  │ ████ ██ ████ ██         │   │  ← mini timeline preview
│  │ 5 blocks · 8h scheduled  │   │
│  └─────────────────────────┘   │
│                                 │
│  ┌─────────────────────────┐   │
│  │ 🟢 Rest Day              │   │
│  │ ████ ████               │   │
│  │ 2 blocks · 3h scheduled  │   │
│  └─────────────────────────┘   │
│                                 │
└─────────────────────────────────┘
```

**Template Card:**
- Background: `--surface`
- Accent color dot on left of name
- Mini timeline: horizontal bar, colored blocks proportional to duration
- Metadata line: `{n} blocks · {total}h scheduled` in `--text-dim`
- Tap: opens template detail / edit view
- Swipe left: delete (with confirm)

---

## Creation Flow (Bottom Sheet)

A 3-step bottom sheet triggered by any "+" button.

### Step 1 — Schedule Origin

```
┌────────────────────────────────┐
│  ▔▔▔▔▔▔▔▔                      │  ← drag handle
│  New Schedule                  │
│                                │
│  ○  Start from scratch         │
│  ○  Use a template             │
│                                │
│                 [ Continue → ] │
└────────────────────────────────┘
```

### Step 2a — Build from scratch

Add blocks inline:
```
Date:  [ Thu Aug 7    ]
Name:  [ My Schedule  ]

Blocks:
  [ Morning Run    ] [ 07:00 ] → [ 07:45 ]
  [ Breakfast      ] [ 08:00 ] → [ 08:30 ]
  [ + Add block ]
```

### Step 2b — From template

```
Pick a template:
  ○ Deep Work Day
  ○ Rest Day
  ○ Workout Day

Apply to:  [ Date picker ]
```

### Step 3 — Repeat Rule

```
Repeat this schedule?  [ toggle ]

If on:
  Frequency:
  ○ Daily
  ○ Weekly on: [ M ] [ T ] [ W ] [ T ] [ F ] [ S ] [ S ]
  ○ Monthly (same day each month)
  ○ Specific dates…

  Ends:
  ○ Never
  ○ On date: [ picker ]
  ○ After __  occurrences
```

Then: `[ Save Schedule ]`

---

## Navigation

Bottom tab bar — 3 tabs:
```
[ Today ]  [ Upcoming ]  [ Templates ]
```

- Active tab: `--accent` icon/label
- Inactive: `--text-dim`
- No label text below icons (icon-only, minimal)

FAB ("+") — floats above tab bar, aligned right.

---

## Empty States

| Context | Message |
|---|---|
| Today, no schedule | "No schedule today · tap + to add one" |
| Templates, none yet | "No templates yet · tap + to create one" |
| Upcoming, day empty | "Nothing scheduled" (small, dim) |

All empty states: centered, `--text-dim`, no heavy UI.

---

## Interaction Notes

- **Tap** block: expand details in-place
- **Long-press** block: enter edit mode (resize handles appear)
- **Swipe left** template card: reveal delete
- **Bottom sheet**: drag to dismiss, tap backdrop to dismiss
- **No modals** — always bottom sheets or in-place expand
- **No toast spam** — only on destructive actions (delete)
