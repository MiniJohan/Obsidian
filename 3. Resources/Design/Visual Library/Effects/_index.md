# Effects

Isolated visual effects. Grain, glow, glass, noise, blur, blend modes, gradient borders, shimmers.
Priority: save reconstructable effects. If you can't describe *how* it works, it's hard to use it.

→ [[../_index|Visual Library]] · [[../_templates/Effect|Template]]

---

## Tags for this category

| Tag | Meaning |
|---|---|
| `grain` | Film grain, noise texture overlay |
| `glow` | Light glow, bloom |
| `glass` | Glassmorphism, frosted glass |
| `blur` | Blur effects, backdrop-filter |
| `gradient` | Gradient as effect (not background) |
| `noise` | SVG/CSS noise filters |
| `blend-mode` | Mix-blend-mode tricks |
| `border` | Gradient borders, neon borders |
| `shimmer` | Shimmer / skeleton loading |
| `shader` | GLSL / WebGL shader |
| `css` | Achievable in CSS alone |
| `mobile-safe` | Performs well on mobile |
| `mobile-caution` | Performance risk on mobile |

---

## Performance tiers

### Mobile-safe (CSS, SVG)
### Use with care (backdrop-filter, heavy filter)
### Desktop-only (WebGL, complex shaders)

---

## All entries

*Add new entries below as wikilinks.*


---

## Performance note

`backdrop-filter: blur()` is the most common performance trap. It works fine on iOS Safari but can tank mid-range Android. Always test. If it's decorative, make it `@media (prefers-reduced-motion: no-preference)` and provide a flat fallback.

---

## Adding a new entry

1. Create note: `Effect name — Source.md`
2. Insert [[../_templates/Effect|Effect template]]
3. Reconstruct the CSS — this is the most valuable part of the note
4. Add wikilink above
