# Color

Dark palette system. High contrast. One accent. No noise.

---

## Core Palette

### Backgrounds — layered from pure black up
```css
--bg:    #000000;   /* Page / app background */
--bg-1:  #0a0a0a;   /* Slightly elevated surface */
--bg-2:  #111111;   /* Cards, panels */
--bg-3:  #1a1a1a;   /* Input fields, hover states */
--bg-4:  #222222;   /* Active/pressed states */
```

**Rule:** Background layers exist to create depth without colour.
One or two levels above `--bg` is usually enough. Don't add more.

### Text
```css
--text:    #ffffff;   /* Primary — headings, body */
--text-2:  #a0a0a0;   /* Secondary — descriptions, metadata */
--text-3:  #555555;   /* Tertiary — timestamps, placeholders, muted */
--text-inv: #000000;  /* Inverted — text on light/accent surfaces */
```

### Borders
```css
--border:   #1f1f1f;   /* Default dividers */
--border-2: #2a2a2a;   /* Stronger dividers, card edges */
--border-3: #383838;   /* Focus rings, selected states */
```

### Accent
```css
--accent:        #ffffff;          /* Default: white on black */
--accent-text:   #000000;          /* Text on accent bg */
```

For projects where white accent isn't distinct enough, use a single hue:
```css
/* Option A — blue */
--accent: #4f8ef7;

/* Option B — neutral warm */
--accent: #e8e0d4;
```

**Rule:** One accent per project. If you need a second colour, it's a status colour, not an accent.

### Status / Semantic
```css
--success:  #22c55e;   /* Green — saved, confirmed, online */
--warning:  #f59e0b;   /* Amber — caution, partial */
--error:    #ef4444;   /* Red — failed, deleted, critical */
--info:     #60a5fa;   /* Blue — informational */
```

Status colours should appear only on text/icons/badges — not as background fills.
Exception: error states on inputs (border + faint background tint).

---

## Contrast Rules

| Text colour | Bg colour | Minimum ratio | Passes |
|---|---|---|---|
| `--text` (#fff) | `--bg` (#000) | 21:1 | ✅ AAA |
| `--text-2` (#a0a0a0) | `--bg` (#000) | ~6.6:1 | ✅ AA |
| `--text-3` (#555555) | `--bg` (#000) | ~4.3:1 | ⚠️ AA large only |

`--text-3` is for large text / icons only. Never for body copy.

---

## Full Custom Properties Block

```css
:root {
  --bg:         #000000;
  --bg-1:       #0a0a0a;
  --bg-2:       #111111;
  --bg-3:       #1a1a1a;
  --bg-4:       #222222;

  --text:       #ffffff;
  --text-2:     #a0a0a0;
  --text-3:     #555555;
  --text-inv:   #000000;

  --border:     #1f1f1f;
  --border-2:   #2a2a2a;
  --border-3:   #383838;

  --accent:     #ffffff;
  --accent-text:#000000;

  --success:    #22c55e;
  --warning:    #f59e0b;
  --error:      #ef4444;
  --info:       #60a5fa;
}
```

---

## Usage Patterns

### Input fields
```css
background: var(--bg-3);
border: 1px solid var(--border-2);
color: var(--text);

/* Focus */
border-color: var(--border-3);
outline: none;
```

### Cards / panels
```css
background: var(--bg-2);
border: 1px solid var(--border);
```

### Primary button
```css
background: var(--accent);   /* white */
color: var(--accent-text);   /* black */
```

### Ghost / secondary button
```css
background: transparent;
border: 1px solid var(--border-2);
color: var(--text);
```

### Destructive button
```css
background: transparent;
border: 1px solid var(--error);
color: var(--error);
```

---

## Anti-patterns

- ❌ Gradients on backgrounds
- ❌ Semi-transparent overlays for depth (use bg layers instead)
- ❌ Coloured backgrounds for cards
- ❌ More than one accent colour
- ❌ Using `--text-3` for interactive labels

---

## References

→ [[Principles]] — why one accent
→ [[Components]] — colour in use
→ [[_index]] — master variables block
