# Patterns

UX and interaction patterns. What to show when something is happening, empty, or wrong.

---

## Loading States

### Inline skeleton
Use for content that loads async (lists, grids, cards).
```html
<div class="skeleton"></div>
```
```css
.skeleton {
  background: var(--bg-2);
  border-radius: var(--r-sm);
  position: relative;
  overflow: hidden;
}
.skeleton::after {
  content: '';
  position: absolute;
  inset: 0;
  transform: translateX(-100%);
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255,255,255,0.04),
    transparent
  );
  animation: shimmer 1.5s infinite;
}
@keyframes shimmer {
  to { transform: translateX(100%); }
}
```

### Full-screen loading
```html
<div class="loading-screen">
  <div class="spinner"></div>
</div>
```
```css
.loading-screen {
  position: fixed;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--bg);
}

.spinner {
  width: 24px;
  height: 24px;
  border: 2px solid var(--border-2);
  border-top-color: var(--text-2);
  border-radius: 50%;
  animation: spin 0.6s linear infinite;
}
@keyframes spin { to { transform: rotate(360deg); } }
```

### Button loading state
```css
.btn[aria-busy="true"] {
  opacity: 0.6;
  pointer-events: none;
  cursor: not-allowed;
}
```
Swap button text to "Saving…" in JS. Don't just disable without feedback.

---

## Empty States

```html
<div class="empty-state">
  <p class="empty-title">Nothing here yet</p>
  <p class="empty-body">Add something to get started.</p>
  <button class="btn btn-primary">Add first item</button>
</div>
```
```css
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--space-3);
  padding: var(--space-8) var(--space-5);
  text-align: center;
}

.empty-title {
  font-size: var(--text-md);
  font-weight: 600;
  color: var(--text);
}

.empty-body {
  font-size: var(--text-base);
  color: var(--text-2);
  max-width: 240px;
  line-height: 1.5;
}
```

**Rules:**
- Never show an empty state mid-load. Show skeleton first.
- The empty state CTA should be the primary action (add, upload, create).
- Keep copy brief. One-line title, one-line body at most.

---

## Error States

### Form field error
```html
<div class="field">
  <input class="input error" type="email">
  <p class="field-error">Invalid email address.</p>
</div>
```
See [[Components]] for field CSS.

### Full-page error
```html
<div class="error-state">
  <p class="error-title">Something went wrong</p>
  <p class="error-body">We couldn't load this. Try again.</p>
  <button class="btn btn-ghost" onclick="retry()">Retry</button>
</div>
```
```css
/* Same structure as .empty-state, use error colour for title */
.error-state .error-title { color: var(--error); }
```

### Toast / inline notification
```html
<div class="toast toast-error">Upload failed. Try again.</div>
<div class="toast toast-success">Saved.</div>
```
```css
.toast {
  position: fixed;
  bottom: calc(var(--space-5) + env(safe-area-inset-bottom));
  left: 50%;
  transform: translateX(-50%);
  max-width: calc(100% - var(--space-8));
  padding: var(--space-3) var(--space-5);
  border-radius: var(--r-full);
  font-size: var(--text-sm);
  font-weight: 500;
  z-index: 200;
  white-space: nowrap;
}

.toast-success {
  background: rgba(34,197,94,0.15);
  color: var(--success);
  border: 1px solid rgba(34,197,94,0.3);
}
.toast-error {
  background: rgba(239,68,68,0.15);
  color: var(--error);
  border: 1px solid rgba(239,68,68,0.3);
}
```

Show toasts for 3s. Fade in/out with opacity transition.

---

## Auth States

### Auth gate (unauthenticated)
- Redirect to `/auth.html` or show auth modal
- Never show broken/empty app shell to unauthenticated users
- On load: check session first, render content second

### Session check pattern (vanilla JS)
```js
async function requireAuth() {
  const { data: { session } } = await supabase.auth.getSession();
  if (!session) {
    window.location.href = '/auth.html';
    return null;
  }
  return session;
}
```

---

## Confirmation Patterns

### Destructive action — two-click confirm
Don't show a modal for every delete. Instead:
1. First tap: button changes to "Tap again to delete" (red, different label)
2. Second tap (within 3s): executes
3. After 3s: resets

```js
let confirmTimer;
btn.addEventListener('click', () => {
  if (btn.dataset.confirm === 'pending') {
    clearTimeout(confirmTimer);
    deleteItem();
    btn.dataset.confirm = '';
    btn.textContent = 'Delete';
  } else {
    btn.dataset.confirm = 'pending';
    btn.textContent = 'Tap again to delete';
    confirmTimer = setTimeout(() => {
      btn.dataset.confirm = '';
      btn.textContent = 'Delete';
    }, 3000);
  }
});
```

Used in Hǫll lightbox (two-click delete).

---

## Transitions

### Page / section fade
```css
.fade-in {
  animation: fadeIn 200ms var(--ease) forwards;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(6px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

### Slide up (for modals/sheets)
```css
@keyframes slideUp {
  from { transform: translateY(100%); }
  to   { transform: translateY(0); }
}
.modal { animation: slideUp 250ms var(--ease); }
```

### Rules
- Duration: 150ms for micro (button press), 250ms for UI (modal), 400ms for page (rare)
- Never animate layout properties (width, height, top) — animate transform and opacity only
- `will-change: transform` on elements that animate frequently

---

## Offline / PWA

### Offline banner
```html
<div class="offline-banner" id="offlineBanner" hidden>
  You're offline. Changes will sync when reconnected.
</div>
```
```css
.offline-banner {
  position: fixed;
  top: env(safe-area-inset-top);
  left: 0;
  right: 0;
  padding: var(--space-2) var(--space-4);
  background: var(--warning);
  color: #000;
  font-size: var(--text-sm);
  font-weight: 600;
  text-align: center;
  z-index: 500;
}
```
```js
window.addEventListener('offline', () => { banner.hidden = false; });
window.addEventListener('online',  () => { banner.hidden = true; });
```

---

## References

→ [[Components]] — component CSS
→ [[Color]] — status colour tokens
→ [[Spacing & Layout]] — safe areas
