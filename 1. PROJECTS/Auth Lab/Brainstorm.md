# Auth Lab — Brainstorm

> A space to explore different authentication methods as coding projects.
> No commitment yet — catalogue ideas, rate interest, figure out what to build.

---

## What is this?

I want to understand authentication deeply by building it, not just consuming it through Supabase.
Each entry here is a candidate project — could be a standalone demo, a reusable module, or the foundation for a future app.

**Open questions before picking one:**
- [ ] Should it be a standalone auth demo or part of an actual app?
- [ ] What stack? (FastAPI backend, React/Vanilla frontend, or both?)
- [ ] Is the goal learning, a reusable module, or a portfolio piece?
- [ ] Should it tie into SongPWA, Income, or something new?

---

## Methods — Catalogue

### 🟢 Email + Password
The standard. Hash password (bcrypt/argon2id), store in DB, issue JWT or session cookie.

- **Complexity:** Low–Medium
- **Stack fit:** FastAPI + Supabase or pure FastAPI + Postgres
- **Already done via:** Supabase (abstracted). Worth building raw once to understand the internals.
- **Could become:** A reusable FastAPI auth module. Drop into any future project.
- **Interesting angle:** Build it from scratch — Argon2id hashing, JWT issuance, refresh tokens, the full flow.

---

### 🟢 Magic Link (Email OTP)
User enters email → gets a one-time link → clicks it → logged in. No password.

- **Complexity:** Low–Medium
- **Stack fit:** FastAPI + email provider (Resend, SendGrid)
- **Gotcha:** Fails on iOS PWAs (Safari vs standalone localStorage isolation). Fine for web.
- **Could become:** Web-only auth for a clean no-password experience.
- **Interesting angle:** Implement token expiry, single-use enforcement, and the email send pipeline yourself.

---

### 🟡 TOTP / 2FA (Authenticator App)
Generate a time-based one-time password (like Google Authenticator). Used as a second factor on top of password auth.

- **Complexity:** Medium
- **Stack fit:** FastAPI + `pyotp` library, any frontend
- **Could become:** A 2FA layer bolted onto an existing email+password system.
- **Interesting angle:** Understand the TOTP spec (RFC 6238), QR code enrollment, backup codes. Very satisfying to implement.

---

### 🟡 OAuth / Social Login (Google, GitHub, Discord)
Redirect to provider → user approves → you get a token → exchange for user info.

- **Complexity:** Medium
- **Stack fit:** FastAPI + `authlib` or `python-social-auth`
- **Could become:** Login for a public-facing app (Income direction? SongPWA if shared?)
- **Interesting angle:** Build the OAuth dance from scratch rather than using a library. Understand `state`, `code_verifier`, `PKCE`.

---

### 🟡 Passkeys / WebAuthn (FIDO2)
No password. User authenticates with biometrics (Face ID, fingerprint) or hardware key. The browser handles the cryptography.

- **Complexity:** Medium–High
- **Stack fit:** FastAPI + `py_webauthn`, React or Vanilla JS frontend
- **Could become:** A genuinely modern auth system. Where passwords are heading.
- **Interesting angle:** This is the future. Understanding WebAuthn is legitimately valuable. Complex but well-documented.

---

### 🔴 JWT From Scratch (Custom Token Auth)
Issue signed tokens (HS256 or RS256), validate them server-side, implement refresh token rotation.

- **Complexity:** Medium (but easy to get wrong)
- **Stack fit:** Any backend. Pure FastAPI.
- **Note:** Not an auth method on its own — pairs with email+password or OAuth. But building the token layer yourself is very educational.
- **Could become:** A stateless API auth layer for any of my projects.

---

### 🔴 Session-Based Auth (Server-Side Sessions)
No JWTs. Server stores the session, client gets a session cookie. Old-school but rock-solid.

- **Complexity:** Medium
- **Stack fit:** FastAPI + Redis or Postgres session store
- **Could become:** Auth for a server-rendered app or API where you want server-side control over sessions.
- **Interesting angle:** Understand why sessions fell out of fashion (scalability), why they're still valid, and how they compare to JWTs.

---

### 🔴 Certificate / mTLS Auth
Client presents a certificate. Server validates it. No passwords, no tokens.

- **Complexity:** High
- **Stack fit:** Nginx / FastAPI with client cert verification
- **Could become:** Internal tool auth (VALE, local AI stack?)
- **Interesting angle:** Very niche. Makes sense for machine-to-machine or high-security internal tools.

---

### 🔴 Wallet / Cryptographic Auth (Web3-style, no blockchain)
User signs a challenge with a private key. Server verifies the signature. No password, no third party.

- **Complexity:** High
- **Stack fit:** FastAPI + any frontend with Web Crypto API
- **Note:** Doesn't require blockchain at all. Can be done with standard Ed25519 keypairs.
- **Could become:** Genuinely interesting security experiment. Very unusual to see outside Web3.

---

## Shortlist — What's Actually Worth Building

| Method | Interest | Learning Value | Practical Use |
|---|---|---|---|
| Email + Password (raw) | ⭐⭐⭐ | High — foundational | High — reusable everywhere |
| TOTP / 2FA | ⭐⭐⭐⭐ | High — satisfying to implement | High — 2FA is standard |
| Passkeys / WebAuthn | ⭐⭐⭐⭐⭐ | Very high — future of auth | Medium — complex setup |
| OAuth (from scratch) | ⭐⭐⭐ | High — demystifies the dance | High — needed for public apps |
| JWT from scratch | ⭐⭐ | Medium | High — but already abstracted |
| Magic Link (raw) | ⭐⭐ | Medium | Medium — web-only limitation |

---

## Candidate: What to Build First

**Recommendation (unconfirmed):** Email + Password → JWT flow, built raw in FastAPI.
- Foundational. Every other method builds on it.
- Directly applicable to future projects (Income direction, SongPWA backend if it grows).
- Short enough to complete in 1–2 sessions.
- Add TOTP 2FA on top as a natural second chunk.

**Alternative if I want something more interesting:** WebAuthn / Passkeys.
- Modern. Barely anyone builds this from scratch.
- Would be a genuine portfolio differentiator.
- Heavier upfront learning curve.

---

## Notes

*(Add thoughts here as you think about it)*

