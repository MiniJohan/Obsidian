# Nodus — UI & Design

---

## Design Philosophy

The interface should feel like a **quiet room** rather than a productivity dashboard.

Negative space is intentional, not a failure to fill space. When the canvas is empty, it should look empty on purpose — sparse, clean, inviting interaction.

The visual identity comes from:
- Near-black surfaces
- Careful contrast jumps in the gray scale
- Minimal permanent UI
- Typography that recedes until needed
- A single muted accent that functions as a signal, not decoration

---

## Color System

Primary palette is grayscale. No gradients. No glow. No colored panels.

```css
/* Backgrounds */
--bg-base:         #0D0E10;  /* canvas background — deepest surface */
--bg-surface:      #131518;  /* node default surface */
--bg-raised:       #191B1F;  /* node selected / elevated surface */

/* Borders */
--border-subtle:   #222428;  /* very subtle borders, nearly invisible */
--border-default:  #292C31;  /* standard node borders */
--border-strong:   #3A3D44;  /* highlighted / selected borders */

/* Text */
--text-muted:      #777C85;  /* timestamps, secondary info, hints */
--text-secondary:  #A8ADB5;  /* relationship labels, captions */
--text-primary:    #E7E9EC;  /* node titles — main readable text */
--text-strong:     #F5F5F3;  /* emphatic labels, selected node title */

/* Accent — one only */
--accent:          #A88F98;  /* muted rose — selected states, active inputs */
--accent-dim:      #7A6470;  /* softer version for subtle indicators */
```

### Principles

- Never use pure `#000000` or pure `#FFFFFF`
- Surfaces should have 3 distinct levels: base, surface, raised
- Text should have 3 distinct levels: muted, secondary, primary
- Avoid using color for categorization — use shape and position

---

## Accent Color

One accent only. Muted rose/pink (`#A88F98`) is the default direction.

It should appear **desaturated and quiet** from a distance. Up close it reads as signal.

Use for:
- Selected node ring / border
- Active relationship (selected state)
- Focused input border
- Relationship-creation affordance dot
- Subtle active indicators

Do not use for: general decoration, headers, backgrounds, large fills.

Alternative accent directions to evaluate during build:
- Muted lavender: `#8F8FAA`
- Muted sage: `#7A9488`
- Muted blue-gray: `#7A8EA8`
- Muted amber: `#A89A6A`

---

## Typography

Minimal typographic system. Avoid decorative fonts.

Direction:
- UI font: System UI stack or a clean geometric sans (Inter, Geist, or system default)
- Monospace: Only for IDs or code-adjacent display if needed

Scale:
```
Node title:       14–15px, weight 450–500, --text-primary
Node description: 12–13px, weight 400,     --text-secondary
Relation label:   11–12px, weight 400,     --text-muted → --text-secondary on hover
UI hints:         11px,    weight 400,     --text-muted
```

Avoid large headings anywhere in the canvas UI. Nodes are compact. Typography should feel dense without feeling crowded.

---

## Node Appearance

Nodes should feel like **objects floating in space**, not dashboard cards.

### Default state

- Background: `--bg-surface` (`#131518`)
- Border: `1px solid --border-default` (`#292C31`)
- Border radius: `6px` (restrained, not pill-shaped, not sharp)
- Padding: `12px 16px`
- Drop shadow: none or extremely subtle (`0 1px 3px rgba(0,0,0,0.4)`)
- Min width: ~120px
- Displays: title only

### Selected state

- Background: `--bg-raised` (`#191B1F`)
- Border: `1px solid --accent` (`#A88F98`)
- Title becomes `--text-strong`
- May reveal: description (if set), connection count

### Hover state

- Border lightens slightly: `--border-strong`
- Connection affordance dot appears (subtle, colored with `--accent-dim`)

### Editing state (title being typed)

- Inline input replaces title text
- Border becomes `--accent`
- Cursor visible

### Example visual layout

```
┌─────────────────────┐
│                     │
│  Research           │   ← --text-primary, 14px
│                     │
└─────────────────────┘
              ↑
         --bg-surface, --border-default
```

Selected:

```
┌─────────────────────┐ ← --accent border
│                     │
│  Research           │   ← --text-strong
│                     │
│  Things I need to   │   ← --text-secondary, 12px (description)
│  investigate        │
│                     │
│  12 connections     │   ← --text-muted, 11px
│                     │
└─────────────────────┘
```

---

## Relationship Appearance

Relationships are rendered as lines (SVG) between nodes.

### Normal state

```
──────────────────
```
- Color: `--border-default` or slightly lighter
- Stroke width: `1.5px`
- Label: hidden

### Hovered state

```
────── inspired by ──────
```
- Color: `--text-muted` → `--text-secondary`
- Label: appears centered on the line
- Label style: 11px, `--text-muted`

### Selected state

```
━━━━━━━━━━━━━━━━━━━━━━━━
```
- Color: `--accent`
- Stroke width: `2px`
- Label: visible, `--text-secondary`

### Directional

```
──────────────────→
```
- Arrowhead: minimal, not chunky
- Reverse direction possible later

### Secondary / weak (future)

```
- - - - - - - - -
```
- Dashed or reduced opacity
- For lower-weight relationships

### Design principle

> At a glance, show structure. During interaction, show meaning.

Avoid using multiple colors to distinguish relationship types. Use stroke weight and opacity first.

---

## Information Density at Zoom Levels

| Zoom | Node shows | Relationships show |
|---|---|---|
| Far (`< 0.5`) | Shape only, small title | Lines only |
| Medium (`0.5–1.5`) | Title | Lines, label on hover |
| Close (`> 1.5`) | Title + description | Lines + labels always visible |

This does not need to be fully implemented in MVP. The architecture should not prevent it.

---

## Canvas / Empty State

Canvas background: `--bg-base` (`#0D0E10`). Flat. No grid. No dots. Pure space.

Empty state hint (before first node created):

```
Double-click anywhere to create
```

- Centered on canvas
- `--text-muted`, 13px
- Disappears once any node exists
- No additional onboarding, no modals, no setup flows

---

## Motion

Transitions should be **quick and functional**, not decorative.

| Interaction | Duration | Easing |
|---|---|---|
| Node selection highlight | 100ms | ease-out |
| Relationship label fade in | 150ms | ease |
| Node creation appearance | 120ms | ease-out |
| Editing state transition | 80ms | ease |
| Zoom (pinch / wheel) | 0ms (direct) | — |
| Pan (drag) | 0ms (direct) | — |

Pan and zoom should have **zero latency** — they are direct manipulation, not animated.

Avoid:
- Bounce / spring physics
- Entrance animations on page load
- Position transitions on drag (nodes follow the cursor directly)
- Scale animations for emphasis

---

## Contextual UI Philosophy

No permanent toolbars on the canvas.

Controls appear contextually:

| Context | What appears |
|---|---|
| Empty canvas | "Double-click to create" hint |
| Node hovered | Connection affordance dot |
| Node selected | Delete / edit controls (minimal) |
| Relationship selected | Label edit / delete controls |
| Editing node | Confirm / cancel keyboard hints |

This should feel like **minimum viable chrome** — just enough to make functionality discoverable, nothing that decorates the canvas permanently.
