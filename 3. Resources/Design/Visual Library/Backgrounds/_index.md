# Backgrounds

Static and animated backgrounds. CSS gradients, noise textures, mesh gradients, geometric patterns, photographic darks.

→ [[../_index|Visual Library]] · [[../_templates/Background|Template]]

---

## Tags for this category

| Tag | Meaning |
|---|---|
| `gradient` | Linear, radial, or conic gradient |
| `mesh` | Mesh gradient |
| `noise` | Noise texture |
| `grain` | Film grain overlay |
| `geometric` | Lines, grids, shapes |
| `photographic` | Photography as background |
| `blur` | Blurred / bokeh |
| `animated` | Motion or animation |
| `tileable` | Can tile seamlessly |
| `css` | Buildable in pure CSS |
| `dark` | Works on dark |
| `steal` | Actively planning to use |

---

## By type

### CSS-only backgrounds
### Texture / noise
### Gradients (mesh / radial)
### Geometric / patterns
### Photographic
### Animated

---

## All entries

*Add new entries below as wikilinks.*


---

## Quick CSS patterns to remember

```css
/* Radial spotlight */
background: radial-gradient(ellipse at 50% 0%, #1a1a2e 0%, #000 70%);

/* Subtle grid */
background-image: linear-gradient(#1f1f1f 1px, transparent 1px),
                  linear-gradient(90deg, #1f1f1f 1px, transparent 1px);
background-size: 40px 40px;

/* Noise overlay (SVG) */
background-image: url("data:image/svg+xml,...");
opacity: 0.04;
mix-blend-mode: overlay;
```

---

## Adding a new entry

1. Create note: `Background type — Description.md`
2. Insert [[../_templates/Background|Background template]]
3. Fill in CSS reconstruction — even approximate
4. Add wikilink above
