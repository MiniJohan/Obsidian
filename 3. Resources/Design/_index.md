# Design Vault

Personal web-design knowledge system. Dark, minimal, iPhone-first.

---

## Knowledge base

| Note | What it covers |
|---|---|
| [[Principles]] | Core philosophy — what to build toward and what to avoid |
| [[Typography]] | Font stack, size scale, weight, line height, hierarchy |
| [[Color]] | Dark palette, CSS custom properties, contrast rules |
| [[Spacing & Layout]] | Spacing scale, mobile-first layout, iPhone safe areas |
| [[Components]] | Reusable UI patterns with code snippets |
| [[Patterns]] | UX interactions — loading, empty, error, gestures |
| [[Inspiration]] | Reference sites, apps, and designers |

## Visual Library

| Section | What it holds |
|---|---|
| [[Visual Library/_index\|Visual Library]] | Hub — all categories, templates, and workflow |
| [[Visual Library/3D Scenes/_index\|3D Scenes]] | Renders, materials, lighting references |
| [[Visual Library/Animations/_index\|Animations]] | Motion, transitions, micro-interactions |
| [[Visual Library/Backgrounds/_index\|Backgrounds]] | Gradients, noise, texture, patterns |
| [[Visual Library/Effects/_index\|Effects]] | Grain, glow, glass, blur, blend modes |
| [[Visual Library/Components/_index\|Components]] | UI elements worth studying |
| [[Visual Library/Typography/_index\|Typography]] | Fonts, pairings, type-led layouts |
| [[Visual Library/Websites/_index\|Websites]] | Full-site design references |
| [[Visual Library/Screenshots/_index\|Screenshots]] | App and interface captures |
| [[Visual Library/Other/_index\|Other]] | Print, packaging, physical design |

---

## Quick Reference

### CSS Custom Properties — Base Template

```css
:root {
  /* Backgrounds */
  --bg:         #000000;
  --bg-1:       #0a0a0a;
  --bg-2:       #111111;
  --bg-3:       #1a1a1a;

  /* Text */
  --text:       #ffffff;
  --text-2:     #a0a0a0;
  --text-3:     #555555;

  /* Borders */
  --border:     #1f1f1f;
  --border-2:   #2a2a2a;

  /* Accent */
  --accent:     #ffffff;

  /* Spacing */
  --space-1:    4px;
  --space-2:    8px;
  --space-3:    12px;
  --space-4:    16px;
  --space-5:    24px;
  --space-6:    32px;
  --space-7:    48px;
  --space-8:    64px;

  /* Radius */
  --r-sm:       6px;
  --r-md:       12px;
  --r-lg:       20px;
  --r-full:     9999px;

  /* Type */
  --font:       -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  --font-mono:  'SF Mono', 'Fira Code', monospace;

  /* Transitions */
  --ease:       cubic-bezier(0.4, 0, 0.2, 1);
  --fast:       150ms var(--ease);
  --mid:        250ms var(--ease);
  --slow:       400ms var(--ease);
}
```

### Font Size Scale

```css
--text-xs:   11px;
--text-sm:   13px;
--text-base: 15px;
--text-md:   17px;
--text-lg:   20px;
--text-xl:   24px;
--text-2xl:  30px;
--text-3xl:  36px;
```

---

## Projects Using This System

- [[Hǫll]] — photo/video vault
- [[SongPWA]] — music player
- [[Tempo]] — daily planner
- [[Mise]] — recipe PWA (React, reference build)

---

*Last updated: 2026-08-30*
