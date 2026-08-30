# Components

Reusable UI patterns. Dark theme, minimal, iPhone-first.
Each component includes the HTML structure and core CSS.

---

## Buttons

### Primary
```html
<button class="btn btn-primary">Save</button>
```
```css
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-2);
  height: 44px;
  padding: 0 var(--space-5);
  border-radius: var(--r-md);
  font-size: var(--text-base);
  font-weight: 600;
  cursor: pointer;
  border: none;
  transition: opacity var(--fast);
}
.btn:active { opacity: 0.7; }

.btn-primary {
  background: var(--accent);
  color: var(--accent-text);
}
```

### Ghost / Secondary
```css
.btn-ghost {
  background: transparent;
  border: 1px solid var(--border-2);
  color: var(--text);
}
.btn-ghost:hover { border-color: var(--border-3); }
```

### Destructive
```css
.btn-danger {
  background: transparent;
  border: 1px solid var(--error);
  color: var(--error);
}
```

### Icon button
```css
.btn-icon {
  width: 44px;
  height: 44px;
  padding: 0;
  border-radius: var(--r-md);
  background: var(--bg-2);
  border: 1px solid var(--border);
  color: var(--text-2);
}
```

---

## Inputs

```html
<div class="field">
  <label class="field-label">Email</label>
  <input class="input" type="email" placeholder="you@example.com">
</div>
```
```css
.field {
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}

.field-label {
  font-size: var(--text-sm);
  font-weight: 500;
  color: var(--text-2);
}

.input {
  height: 48px;
  padding: 0 var(--space-4);
  background: var(--bg-3);
  border: 1px solid var(--border-2);
  border-radius: var(--r-md);
  color: var(--text);
  font-size: var(--text-base);
  font-family: var(--font);
  outline: none;
  transition: border-color var(--fast);
}
.input::placeholder { color: var(--text-3); }
.input:focus { border-color: var(--border-3); }

/* Error state */
.input.error { border-color: var(--error); }
.field-error {
  font-size: var(--text-xs);
  color: var(--error);
}
```

### Textarea
```css
.textarea {
  /* Inherits .input */
  height: auto;
  min-height: 100px;
  padding: var(--space-3) var(--space-4);
  resize: vertical;
  line-height: 1.5;
}
```

---

## Cards

### Basic card
```html
<div class="card">
  <p class="card-title">Title</p>
  <p class="card-meta">Metadata</p>
</div>
```
```css
.card {
  background: var(--bg-2);
  border: 1px solid var(--border);
  border-radius: var(--r-md);
  padding: var(--space-4);
}

.card-title {
  font-size: var(--text-base);
  font-weight: 500;
  color: var(--text);
}

.card-meta {
  font-size: var(--text-sm);
  color: var(--text-2);
  margin-top: var(--space-1);
}
```

### Tappable card
```css
.card-tap {
  cursor: pointer;
  transition: background var(--fast);
}
.card-tap:active { background: var(--bg-3); }
```

---

## List Items

```html
<ul class="list">
  <li class="list-item">
    <span class="list-item-label">Label</span>
    <span class="list-item-value">Value</span>
  </li>
</ul>
```
```css
.list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.list-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-4) 0;
  border-bottom: 1px solid var(--border);
  min-height: 44px;
}
.list-item:last-child { border-bottom: none; }

.list-item-label { font-size: var(--text-base); color: var(--text); }
.list-item-value { font-size: var(--text-base); color: var(--text-2); }
```

---

## Navigation — Bottom Bar

```html
<nav class="bottom-nav">
  <a class="nav-item active" href="#">Library</a>
  <a class="nav-item" href="#">Upload</a>
  <a class="nav-item" href="#">Settings</a>
</nav>
```
```css
.bottom-nav {
  display: flex;
  align-items: center;
  justify-content: space-around;
  height: 56px;
  padding-bottom: max(var(--space-3), env(safe-area-inset-bottom));
  background: var(--bg-1);
  border-top: 1px solid var(--border);
  position: sticky;
  bottom: 0;
}

.nav-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-1);
  font-size: var(--text-xs);
  font-weight: 500;
  color: var(--text-3);
  text-decoration: none;
  padding: var(--space-2) var(--space-4);
  min-width: 44px;
}
.nav-item.active { color: var(--accent); }
```

---

## Modal / Bottom Sheet

```html
<div class="modal-overlay">
  <div class="modal">
    <div class="modal-header">
      <p class="modal-title">Confirm</p>
      <button class="btn-icon modal-close">✕</button>
    </div>
    <div class="modal-body">
      <!-- content -->
    </div>
  </div>
</div>
```
```css
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.7);
  backdrop-filter: blur(4px);
  display: flex;
  align-items: flex-end;   /* bottom sheet */
  z-index: 100;
}

.modal {
  width: 100%;
  background: var(--bg-1);
  border-top: 1px solid var(--border);
  border-radius: var(--r-lg) var(--r-lg) 0 0;
  padding: var(--space-5);
  padding-bottom: max(var(--space-6), env(safe-area-inset-bottom));
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: var(--space-5);
}

.modal-title {
  font-size: var(--text-md);
  font-weight: 600;
  color: var(--text);
}
```

---

## Badges / Chips / Pills

```html
<span class="badge">Active</span>
<span class="badge badge-success">Online</span>
<span class="badge badge-error">Failed</span>
```
```css
.badge {
  display: inline-flex;
  align-items: center;
  height: 22px;
  padding: 0 var(--space-2);
  border-radius: var(--r-sm);
  font-size: var(--text-xs);
  font-weight: 600;
  background: var(--bg-3);
  color: var(--text-2);
}

.badge-success { background: rgba(34,197,94,0.15); color: var(--success); }
.badge-error   { background: rgba(239,68,68,0.15);  color: var(--error); }
.badge-warning { background: rgba(245,158,11,0.15); color: var(--warning); }
```

---

## Filter Pills

Used in Hǫll, SongPWA, Mise.
```html
<div class="filters">
  <button class="pill active">All</button>
  <button class="pill">Photos</button>
  <button class="pill">Videos</button>
</div>
```
```css
.filters {
  display: flex;
  gap: var(--space-2);
  overflow-x: auto;
  scrollbar-width: none;
  padding-bottom: 2px;
}
.filters::-webkit-scrollbar { display: none; }

.pill {
  flex-shrink: 0;
  height: 34px;
  padding: 0 var(--space-4);
  border-radius: var(--r-full);
  font-size: var(--text-sm);
  font-weight: 500;
  background: var(--bg-2);
  border: 1px solid var(--border-2);
  color: var(--text-2);
  cursor: pointer;
  transition: all var(--fast);
}
.pill.active {
  background: var(--accent);
  color: var(--accent-text);
  border-color: var(--accent);
}
```

---

## Search Input

```html
<div class="search-wrap">
  <input class="search" type="search" placeholder="Search…">
</div>
```
```css
.search-wrap { position: relative; }

.search {
  width: 100%;
  height: 40px;
  padding: 0 var(--space-4);
  background: var(--bg-2);
  border: 1px solid var(--border);
  border-radius: var(--r-full);
  color: var(--text);
  font-size: var(--text-sm);
  font-family: var(--font);
  outline: none;
  -webkit-appearance: none;
}
.search::placeholder { color: var(--text-3); }
.search:focus { border-color: var(--border-3); }
```

---

## References

→ [[Color]] — token definitions
→ [[Spacing & Layout]] — spacing tokens
→ [[Patterns]] — interaction states
