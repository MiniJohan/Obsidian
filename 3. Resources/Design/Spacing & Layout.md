# Spacing & Layout

Spacing scale, mobile-first layout, and iPhone-specific rules.

---

## Spacing Scale

Based on a 4px base unit. Multiples grow non-linearly above 32px.

```css
--space-1:   4px;
--space-2:   8px;
--space-3:   12px;
--space-4:   16px;
--space-5:   24px;
--space-6:   32px;
--space-7:   48px;
--space-8:   64px;
--space-9:   96px;
--space-10: 128px;
```

### When to use each

| Token | Use |
|---|---|
| `--space-1` | Icon gaps, inline badge padding |
| `--space-2` | Tight label padding, small gaps |
| `--space-3` | Button padding (vertical), compact list items |
| `--space-4` | Default padding unit — inside cards, inputs |
| `--space-5` | Section content padding, list item vertical spacing |
| `--space-6` | Between major sections |
| `--space-7` | Page-level top/bottom padding |
| `--space-8+` | Hero sections, large visual breathing room |

---

## Border Radius Scale

```css
--r-sm:   6px;     /* Badges, chips, small buttons */
--r-md:   12px;    /* Cards, inputs, standard buttons */
--r-lg:   20px;    /* Bottom sheets, modals, large cards */
--r-xl:   28px;    /* Full-rounded large surfaces */
--r-full: 9999px;  /* Pill buttons, avatars */
```

**Rule:** Don't apply `--r-md` or above to the overall app container.
Rounding the page itself looks like a forced design choice on mobile.

---

## Layout — Mobile First

### Max-width wrapper
```css
.container {
  width: 100%;
  max-width: 480px;      /* Feels right for iPhone-primary apps */
  margin: 0 auto;
  padding: 0 var(--space-4);
}
```

For content-dense apps (music, photo), no max-width — full bleed is fine.

### Page structure (standard)
```css
body {
  display: flex;
  flex-direction: column;
  min-height: 100dvh;     /* dvh handles iOS browser chrome correctly */
}

.app-header {
  position: sticky;
  top: 0;
  z-index: 10;
}

.app-content {
  flex: 1;
  overflow-y: auto;
}

.app-nav {
  position: sticky;
  bottom: 0;
}
```

---

## iPhone-specific

### Safe area insets
```css
/* Apply to fixed/sticky elements */
.app-header {
  padding-top: max(var(--space-4), env(safe-area-inset-top));
}

.app-nav {
  padding-bottom: max(var(--space-3), env(safe-area-inset-bottom));
}
```

### Minimum tap target
```css
min-height: 44px;
min-width: 44px;
```
All interactive elements — buttons, links, list items — must meet this.
Use `padding` to reach it if the visual size is smaller.

### Scrolling
```css
-webkit-overflow-scrolling: touch;   /* Momentum scroll on iOS */
overscroll-behavior: none;           /* Prevent pull-to-refresh bleeding */
```

### Viewport meta (required)
```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
```
`viewport-fit=cover` is required for safe area env() vars to work.

---

## Grid

### Simple column grid
```css
.grid-2 { display: grid; grid-template-columns: repeat(2, 1fr); gap: var(--space-3); }
.grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: var(--space-3); }
```

### Masonry (used in Hǫll)
```css
.masonry {
  columns: 2;
  column-gap: var(--space-2);
}
.masonry > * {
  break-inside: avoid;
  margin-bottom: var(--space-2);
}
```

---

## Whitespace Philosophy

Whitespace is structure. It's not emptiness waiting to be filled.

- Generous vertical spacing between sections signals separation without dividers.
- Tight horizontal padding reduces the "app" feel. Give content room to breathe.
- The temptation is always to fill gaps. Resist.

**Common mistake:** adding content to reduce whitespace instead of accepting that some screens should feel spacious.

---

## References

→ [[Principles]] — why whitespace matters
→ [[Components]] — spacing applied to real elements
→ [[_index]] — full variables block
