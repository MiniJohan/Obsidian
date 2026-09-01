# LORE — UI & Design

---

## Interaction Model

LORE lives at `localhost:5500`, opened in Chrome app mode (no browser chrome, no address bar):

```
chrome --app=http://localhost:5500
```

Launched and focused via a global AutoHotkey hotkey. Same hotkey brings the window to focus and triggers the LISTENING state. Escape or second hotkey press dismisses it. You're back in your editor in under a second.

No Electron. No system tray app. Just a browser window that behaves like a native tool.

---

## App States

| State | Waveform | Status Label |
|---|---|---|
| `IDLE` | Flat line, dimmed | `IDLE` |
| `LISTENING` | Reacts to mic amplitude in real time | `LISTENING` |
| `THINKING` | Slow pulse or idle | `THINKING` |
| `SPEAKING` | Reacts to TTS audio output | `SPEAKING` |

State drives all visual behavior. No manual animation — everything is data-driven.

---

## Layout

```
┌─────────────────────────────────────┐
│                                     │
│                                     │
│        ─────╱╲╱╲╱╲─────            │  ← Waveform.jsx — canvas, Web Audio API
│                                     │     Flatlines at idle
│                                     │     Reacts to mic when LISTENING
│   Response text appears here.       │  ← ResponseText.jsx — centered
│   Fades in, holds, fades out.       │     Current exchange only. No chat log.
│                                     │
│                          LISTENING  │  ← StatusLabel.jsx — bottom-right, small
└─────────────────────────────────────┘
```

Pure black background. No borders. No cards. No panels. Text on black.

---

## Design Tokens

| Token | Value |
|---|---|
| Background | `#000000` |
| Accent | `#00d4d4` — cold cyan |
| Text primary | `#ffffff` |
| Text dim | `#555555` |
| Font | System font stack |
| Waveform color | Accent (`#00d4d4`) |
| Status label | Dim, uppercase, tracking wide, small |

---

## Components

### `Waveform.jsx`
- Canvas element, full width, fixed height (~120px)
- Web Audio API — `AnalyserNode` reads real mic input when LISTENING
- When SPEAKING: `AnalyserNode` reads TTS audio output
- When IDLE / THINKING: flatlines with slight breathing (low amplitude noise or CSS lerp)
- No fake CSS animation. Waveform is always real data or flatline.

### `ResponseText.jsx`
- Displays the current exchange only — no chat log wall
- Fade-in animation when new response arrives
- Holds for readable duration, then fades out
- Max 3–4 lines. Overflow truncates gracefully.
- System font, generous line height (~1.7), moderate size (~18px)

### `StatusLabel.jsx`
- Fixed: bottom-right corner
- Text only: `IDLE` / `LISTENING` / `THINKING` / `SPEAKING`
- Dim color when IDLE, accent color when active
- Uppercase, letter-spacing, font-size ~11px

---

## What's Explicitly Not Here

- ~~Three.js particle system~~ — theatrical, expensive, not Zon's aesthetic
- ~~Chat log / message history~~ — noise, not signal
- ~~Cards, panels, borders~~ — nothing is boxed
- ~~Warm tones / purple accent~~ — cold cyan only
- ~~Animated CSS fake waveform~~ — always real data
