# Design Principles

Core philosophy for every interface I build.

---

## The Aesthetic

**Dark. Minimal. High contrast. iPhone-first.**

Not as a trend — as a constraint that forces clarity.
Every element must earn its place. If it doesn't do work, it doesn't exist.

---

## Core Rules

### 1. Less is more — always
Remove before you add. If something can be implied, don't show it.
Whitespace is not empty space. It's structure.

### 2. Pure black backgrounds
`#000000` or very near it. Not dark grey. Not navy. Black.
It looks correct on OLED screens and holds contrast naturally.

### 3. Text hierarchy through weight and opacity, not size alone
Three levels maximum:
- **Primary** — white, full weight
- **Secondary** — ~60% opacity or muted grey
- **Tertiary** — ~30% opacity, labels and metadata only

### 4. One accent, used sparingly
If everything is accented, nothing is.
White is often the correct accent on a black surface.
Colour only when it carries meaning — status, state, category.

### 5. Borders are for structure, not decoration
Thin (`1px`), dark, purposeful.
When in doubt, skip the border — spacing alone can separate elements.

### 6. iPhone-first, not mobile-first-as-afterthought
Design for a 390px viewport held in one hand.
Bottom navigation, not sidebars. Tappable targets ≥ 44px.
Respect safe areas. Assume notch and home indicator.

### 7. Animation: functional or absent
Motion serves meaning — state change, orientation, feedback.
Never decorative. Keep it fast (150–250ms). Ease in-out.

### 8. Typography does the visual work
In minimal design, type carries the entire hierarchy.
Get the type right and the layout mostly solves itself.

---

## Anti-patterns

Avoid these by default. Justify exceptions explicitly.

| Anti-pattern | Why |
|---|---|
| Cards with shadows | Looks dated; use flat surfaces and borders |
| Gradients | Noise; use flat colour |
| Rounded corners on everything | Reserve radius for interactive elements |
| Icon + label everywhere | Trust the icon or lose it; don't always double up |
| Hover states as the only affordance | Doesn't exist on mobile |
| Full-width CTA buttons in every section | Signals panic; use sparingly |
| Inline borders on every list item | Usually spacing does it better |
| Multiple accent colours | Pick one |

---

## Decision Filter

When unsure about adding something, ask:

1. Does removing it break comprehension?
2. Does it exist on mobile (touch) as well as desktop?
3. Is it consistent with the rest of the system?
4. Is it animated because it needs to be, or because it's possible?

If any answer is "no / not sure" — cut it first, add back only if missed.

---

## References

→ [[Color]] for palette rules
→ [[Typography]] for type hierarchy
→ [[Spacing & Layout]] for whitespace discipline
