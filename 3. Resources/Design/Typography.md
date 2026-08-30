# Typography

Type system for dark, minimal, iPhone-first interfaces.

---

## Font Stack

### UI (default)
```css
font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif;
```
System font renders natively on iOS/macOS (SF Pro), Windows (Segoe UI), Android (Roboto).
No web font load, no FOUT, no licensing. Feels native on every device.

### Monospace
```css
font-family: 'SF Mono', 'Fira Code', 'Cascadia Code', ui-monospace, monospace;
```
For code blocks, timestamps, IDs, technical metadata.

---

## Size Scale

```css
--text-xs:   11px;   /* Metadata, labels, badges */
--text-sm:   13px;   /* Secondary body, captions */
--text-base: 15px;   /* Primary body text */
--text-md:   17px;   /* Slightly prominent body */
--text-lg:   20px;   /* Section headers, card titles */
--text-xl:   24px;   /* Page titles (secondary) */
--text-2xl:  30px;   /* Page titles (primary) */
--text-3xl:  36px;   /* Hero / display */
```

**Rule of thumb:** Most of the interface lives at `--text-sm` and `--text-base`.
`--text-lg` and above are used rarely — for page-level anchors only.

---

## Weight Scale

| Weight | Value | Usage |
|---|---|---|
| Regular | 400 | Body copy, secondary text |
| Medium | 500 | List items, card content |
| Semibold | 600 | Section headers, labels |
| Bold | 700 | Page titles, primary CTAs |

**Avoid 800+ (Black)** — rarely reads well at small sizes on dark backgrounds.

---

## Line Height

```css
/* Tight — headings */
line-height: 1.1;

/* Snug — short labels, metadata */
line-height: 1.3;

/* Normal — body copy */
line-height: 1.5;

/* Relaxed — long-form reading */
line-height: 1.7;
```

---

## Letter Spacing

```css
/* Uppercase labels and small caps */
letter-spacing: 0.08em;

/* Default body — no override needed */
letter-spacing: normal;

/* Display / hero — slightly negative */
letter-spacing: -0.02em;
```

---

## Hierarchy in Practice

```css
/* Page title */
.title {
  font-size: var(--text-2xl);
  font-weight: 700;
  letter-spacing: -0.02em;
  line-height: 1.1;
  color: var(--text);
}

/* Section header */
.section-label {
  font-size: var(--text-xs);
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--text-3);
}

/* Body */
.body {
  font-size: var(--text-base);
  font-weight: 400;
  line-height: 1.5;
  color: var(--text);
}

/* Secondary text */
.meta {
  font-size: var(--text-sm);
  font-weight: 400;
  color: var(--text-2);
}

/* Timestamp / label */
.label {
  font-size: var(--text-xs);
  color: var(--text-3);
}
```

---

## Rules

- **Max 3 size steps per screen.** More than that = no hierarchy.
- **Weight > size** for differentiation. Bumping from 400→600 reads clearer than bumping 15px→18px.
- **Never set text smaller than 11px** (accessibility + legibility).
- **Avoid centring body text.** Centre for single-line labels only.
- **Don't mix too many weights.** 400 + 600 usually enough. Add 700 for one anchor element.

---

## References

→ [[Principles]] — why sparse typography
→ [[Color]] — text colour tokens
→ [[_index]] — base CSS variables
